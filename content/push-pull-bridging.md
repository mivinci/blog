---
title: "Push、Pull 与桥接的代价：为什么回调式 API 和 async/await 之间必然隔着一个 channel"
date: 2026-08-24
description: 从一个真实的架构问题出发：libx 的同步回调（push 模式）和 libxpp 的 Promise/await（pull 模式）桥接时，为什么必然要引入 channel？答案藏在"谁控制时机"这个问题里。
tags: ["架构", "异步编程", "C++", "并发"]
draft: false
math: false
diagram: true
---

## 0. 一个真实的架构问题

最近在写一个 C++ 网络库的异步封装层时，遇到一个绕不开的问题。

底层 `libx` 是经典的 C 风格事件驱动库：注册回调，事件发生时 EventLoop 调你：

```c
xHttpClientDo(&conf,
              on_response,   // 响应头到达时被调用
              on_data,       // 每收到一段 body 被调用一次
              on_done,       // 请求结束时被调用一次
              arg);
```

上层 `libxpp` 是 Rust 风格的 async/await 封装：

```cpp
auto resp = client.send(std::move(req)).await();
auto body = resp.into_body().bytes().await();
```

两层各自都很干净。问题是：**桥接层怎么写？**

- `on_data` 每次被调用时，数据要放到哪里，`bytes().await()` 才能拿到？
- 如果 `on_data` 来得比 `.await()` 快，多余的 chunk 怎么办？
- 如果 `.await()` 先到，`on_data` 还没来，谁来挂起它、谁来唤醒它？

写完桥接层回头看，发现核心就是一个 channel。而且这不是巧合——这篇文章想说的是：**这不是实现选择，是结构必然**。

## 1. Push 和 Pull 的本质：谁控制时机

先把两个词说精确。教科书里 push/pull 常被描述成"数据流方向"，但方向不是本质——本质是**时机的控制权**。

**Push 模式**：生产者控制时机。数据/事件就绪时，生产者主动调用消费者的接口（回调）。消费者被动接电话。

```cpp
void on_data(const char *data, size_t len, void *arg);  // EventLoop 决定何时调用
```

**Pull 模式**：消费者控制时机。消费者主动发起请求，没有数据就挂起等，有数据被唤醒。消费者主动打电话。

```cpp
auto chunk = body.read().await();  // 消费者决定何时发起，没数据就 park
```

一个"来电式"，一个"取件式"。Epoll 回调、订阅者模式、_INTERRUPT 信号处理是 push；迭代器、`read()` 系统调用、async/await 是 pull。

## 2. 单值情况：Promise 本身就是桥

如果 push 侧只发生**一次**，桥接是平凡的，不需要 channel——`PromiseResolver` 就是桥：

```cpp
// push 侧（EventLoop 线程，时机归它）
void on_done(xHttpCtx *ctx, void *arg) {
  adapter->resolver.resolve(build_response(ctx));
}

// pull 侧（用户线程，时机归它）
auto resp = promise.await();
```

push 侧 resolve，pull 侧 await，看起来没有任何缓冲。但仔细看 `PromiseResolver` 的实现，它内部是一个堆上的共享槽位（我们项目里叫 `WakeState`）：resolve 把值写进槽位并唤醒 waiter，await 没等到就 park 在槽位上。

**容量为 1 的存储 + 一次唤醒**。这就是一个一次性的 channel——只是我们通常不这么叫它。

所以严格地说，单值桥接也"有 channel"，只是它退化成了 Promise 自己，退化到我们不觉得它是缓冲。

## 3. 多值情况：两个时钟不同步

真正的麻烦从多次回调开始。`on_data` 会被 EventLoop 反复调用，每次一段 body；而消费侧 `.await()` 的时机由用户代码决定：

```
on_data(chunk1) ─┐
on_data(chunk2) ─┤   EventLoop 的时钟：网络数据到了就推
on_data(chunk3) ─┘
                 ┐
body.read().await()   用户的时钟：我处理完了才拉
```

**两个时钟不同步**，这是问题的根源。具体会撞上三件事：

1. **生产快于消费**——`on_data` 第二次触发时，第一次的数据还没被拉走。没有中间存储就丢数据。
2. **消费快于生产**——`.await()` 发起时 `on_data` 还没来。消费者必须能挂起，且生产者来数据时能唤醒它。
3. **缓冲不能无限涨**——消费端处理慢（或者根本不读了），生产端不能把内存推爆。满了得让生产者停下来，也就是背压。

“**存储 + 唤醒 + 背压**”，三件套缺一不可。而这三件套的标准封装，就是 channel：

```cpp
// 桥接层：libx 的 push → mpsc channel
int on_data_cb(const char *data, size_t len, void *arg) {
  auto chunk = Bytes::copy(data, len);
  if (adapter->tx.try_send(std::move(chunk)).is_err()) {
    return -1;  // channel 满 → 告诉 libx 暂停（背压）
  }
  return 0;
}

// 消费侧：channel → libxpp 的 pull
Promise<http::Result<Bytes>> Body::bytes() {
  return io::read_all(*this);  // read() 内部 recv().await()
}
```

我们项目里 `Body::from_channel` 就是这个设计：`SendAdapter` 把 `on_data` 的每次 push 转成 `try_send`，channel 满了返回 -1 让 libx 暂停读取 socket——背压一路传导回网络层。

## 4. 更抽象一点：中间态的三要素

把具体实现拿掉，push→pull 的桥接结构是这样的：

```
push（时机归生产者）──▶ [中间态] ──▶ pull（时机归消费者）
```

任何这样的桥，中间态都由两个自由度刻画：**槽数**和**通知方式**。

| 中间态 | 槽数 | 通知 | 背压 |
|--------|------|------|------|
| `PromiseResolver` | 1 | resolve 唤醒 waiter | 无需（只发生一次） |
| `mpsc::channel` | N | send 唤醒 recv 的 waiter | 满则 send 挂起/失败 |
| `AtomicPromiseWaker` | 0 | wake 重投 poll 步骤 | 无（纯事件） |

第三行值得多说一句。刚合入的 `xpp::spawn()`——一个 tokio 风格的任务驱动器——桥接的就是**纯事件**而非数据：EventLoop 的"socket 可读"事件（push）转成"重新 poll 一次"（pull 侧的驱动信号）。数据本身留在内核缓冲区（那是内核提供的"channel"），waker 只传递"该干活了"的通知。槽数为零，因为它搬运的不是数据。

所以更准确的说法是：**push→pull 桥接必然需要一个中间态，其最小构成是"存储 + 通知"；channel 是数据型中间态的标准名，Promise 是它的一次性退化，Waker 是它的零槽特例。**

## 5. 有没有不用 channel 的方式

有，但条件苛刻：**让 pull 吃掉 push**。

generator / `co_yield` 风格就是这种——生产者的每一步都是消费者拉出来的：

```cpp
// C++20 协程：消费者每次 co_await 才驱动生产者跑一段
task<Bytes> stream() {
  while (auto chunk = co_await next_network_event()) {
    co_yield chunk;
  }
}
```

数据不需要缓冲，因为生产者的步进完全由消费者的节奏控制——只有一个时钟了。Rust 的 `async-stream`、Python 的 generator、Unix 管道的惰性语义都属于这一族。

但这要求**生产者可以被逐步驱动**。libx 的回调做不到：回调时机由 EventLoop/epoll 控制，数据到达内核缓冲区时就必须被读走（否则 TCP 窗口关闭，对端阻塞）。网络 I/O 的物理现实是**网络永远是 push 的**——数据不会等你来拉，它只会在门口排队或被拒收。

所以在事件驱动 C 库之上封装 async/await 的场景里，"必然引入 channel"成立。这不是设计缺陷，是 push 源头的物理属性决定的。

## 6. 旁证：tokio 的分层

回头看 tokio，会发现同样的结构藏在每一层：

```
内核 epoll 事件（push）
  → waker 通知（零槽中间态："该 poll 了"）
    → async 任务被调度（pull 侧苏醒）
      → 数据从 socket buffer 读入用户 buffer（内核提供的存储）
        → AsyncReadExt::read（pull）
          → mpsc / broadcast channel（用户态存储，跨任务桥接）
```

tokio 文档里那句著名的"不要在 async 里做阻塞"背后，其实是同一个时钟问题：阻塞调用是"push 时代的同步时钟"，混进 pull 世界就会把整个事件循环的单时钟卡死。

## 7. 总结

回到最初的问题：回调风格和 async/await 之间，为什么必然隔着一个 channel？

1. push 和 pull 的本质区别不是数据流向，是**时机的控制权**。
2. 桥接两个不同时钟的系统，必然需要中间态：**存储 + 通知**，数据速率不匹配时再加**背压**。
3. channel 是这个中间态的标准封装；Promise 是它的一次性单槽退化；Waker 是它的零槽纯通知特例。
4. 唯一的例外是 pull 吃掉 push（generator 风格），但前提是生产者可被逐步驱动——网络 I/O 不满足这个前提。

下次再写桥接层的时候，可以省下"能不能不用 channel"的纠结，直接想三个问题：槽数多少？谁来唤醒谁？满了怎么办？

想清楚这三个，桥接层就写完了。
