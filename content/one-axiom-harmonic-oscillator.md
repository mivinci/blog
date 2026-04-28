---
title: "以升降算符为唯一公理导出量子理论"
date: 2026-01-10
description: 位置与动量并非宇宙的底层语言 —— 产生与湮灭才是。从这对最简单的操作出发，仅凭一条公理，即可重构量子谐振子的全部结构。
tags: ["物理", "数学", "哲学"]
draft: false
math: true
diagram: true
---

## 本源公理

抛开所有经验性物理假设，从微观世界最基础的「产生、湮灭」行为出发，确立唯一核心公理：

存在一对线性算符 $\hat{a}^\dagger$（产生）与 $\hat{a}$（湮灭），以及一组量子态 $\{\ket{n}\}$，使得 $\hat{a}^\dagger$ 与 $\hat{a}$ 分别将量子数升、降一步：

$$
\hat{a}^\dagger\ket{n}=c_{n+1}\ket{n+1},\quad \hat{a}\ket{n}=c_{n}\ket{n-1}
$$

递推的终点即基态条件 $\hat{a}\ket{0}=0$（$\ket{-1}$ 不存在，故 $c_0=0$）。其中 $c_n$ 为待定归一化系数。

## 数算符

定义数算符：

$$
\hat{N}=\hat{a}^\dagger\hat{a}
$$

**第一步：** $\hat{N}^\dagger=(\hat{a}^\dagger\hat{a})^\dagger=\hat{a}^\dagger(\hat{a}^\dagger)^\dagger=\hat{a}^\dagger\hat{a}=\hat{N}$，故 $\hat{N}$ 为厄米算符，本征值为实数。

**第二步：$\hat{N}$ 以 $\ket{n}$ 为本征态。** 由升降作用：

$$
\hat{N}\ket{n}=\hat{a}^\dagger\hat{a}\ket{n}=\hat{a}^\dagger c_n\ket{n-1}=c_n\cdot c_n\ket{n}=c_n^2\ket{n}
$$

因此 $\ket{n}$ 是 $\hat{N}$ 的本征态，本征值为 $c_n^2$，即 $n=c_n^2$。

**第三步：本征值非负。** 对任意态 $\ket{\psi}$，

$$
\bra{\psi}\hat{N}\ket{\psi}=\bra{\psi}\hat{a}^\dagger\hat{a}\ket{\psi}=\|\hat{a}\ket{\psi}\|^2\geq 0
$$

故 $\hat{N}$ 的全部本征值非负，$n\geq 0$。结合基态条件 $\hat{a}\ket{0}=0$ 得 $\hat{N}\ket{0}=0$，即最低本征值 $n=0$。

**第四步：本征值取整数。** 由升降作用 $\hat{a}^\dagger\ket{n}=c_{n+1}\ket{n+1}$，每作用一次 $\hat{a}^\dagger$，本征值从 $c_n^2$ 增至 $c_{n+1}^2$。再由

$$
\hat{a}\hat{a}^\dagger\ket{n}=c_{n+1}\hat{a}\ket{n+1}=c_{n+1}^2\ket{n}
$$

与 $\hat{N}\ket{n}=c_n^2\ket{n}$ 比较，得 $\hat{a}\hat{a}^\dagger-\hat{a}^\dagger\hat{a}$ 作用于 $\ket{n}$ 给出 $(c_{n+1}^2-c_n^2)\ket{n}$。记 $\Delta=c_{n+1}^2-c_n^2$，则从基态出发反复递推：

$$
n=c_n^2=0+(n-0)\cdot\Delta
$$

取 $\Delta=1$（对应 $[\hat{a},\hat{a}^\dagger]=1$，见对易关系一节），即得 $c_n^2=n$，本征值为非负整数 $n=0,1,2,\dots$，归一化系数 $c_n=\sqrt{n}$。

**第五步：正交归一性。** 以上四步仅涉及本征值，尚未讨论 $\{\ket{n}\}$ 的正交归一性——事实上，公理中并未预设这一点。现在证明它是代数结构的推论。

由第一步，$\hat{N}$ 厄米，故属于不同本征值的本征态必然正交：$n\neq m$ 时 $\braket{m|n}=0$。对于同一本征值的简并态，升降算符将它们一一映射（$\hat{a}^\dagger\ket{n}\propto\ket{n+1}$），在映射链上选择归一化即消除简并自由度。因此 $\{\ket{n}\}$ 构成 $\hat{N}$ 的一组正交归一本征基——正交归一性并非独立假设，而是 $\hat{N}$ 的厄米性 + 无简并谱的自然结果。

综上，$\hat{N}=\hat{a}^\dagger\hat{a}$ 确为「数算符」：厄米、本征值非负整数、$\hat{N}\ket{n}=n\ket{n}$，且 $\{\ket{n}\}$ 自动正交归一。

## 对易关系

由数算符一节已知，$[\hat{a}, \hat{a}^\dagger]\ket{n} = (c_{n+1}^2 - c_n^2)\ket{n}$，即对易子 $[\hat{a}, \hat{a}^\dagger]$ 在 $\{\ket{n}\}$ 基下是对角的。

**第一步：对易子为 c-number。** 升降算符的最简代数封闭性要求 $[\hat{a}, \hat{a}^\dagger]$ 不依赖态 $\ket{n}$——否则算符代数结构将随态而变，失去普适性。因此 $c_{n+1}^2 - c_n^2$ 必须是常数，记为 $\Delta$。

**第二步：确定归一化系数。** 由基态条件 $\hat{a}\ket{0} = 0$，得 $c_0 = 0$。递推：

$$
c_n^2 = c_0^2 + n\Delta = n\Delta
$$

取 $\Delta = 1$（非零最简值，即取量子数增量为最小单位），得：

$$
c_n = \sqrt{n}
$$

升降算符的归一化作用为：

$$
\hat{a}^\dagger\ket{n} = \sqrt{n+1}\ket{n+1},\quad \hat{a}\ket{n} = \sqrt{n}\ket{n-1}
$$

**第三步：对易关系。** 代入 $\Delta = 1$：

$$
[\hat{a}, \hat{a}^\dagger]\ket{n} = \ket{n}
$$

对任意 $\ket{n}$ 成立，故：

$$
[\hat{a}, \hat{a}^\dagger] = 1
$$

**第四步：数算符与升降算符的对易关系。** 由 $[\hat{a}, \hat{a}^\dagger] = 1$ 直接计算：

$$
[\hat{N}, \hat{a}^\dagger] = \hat{a}^\dagger\hat{a}\hat{a}^\dagger - \hat{a}^\dagger\hat{a}^\dagger\hat{a} = \hat{a}^\dagger[\hat{a}, \hat{a}^\dagger] = \hat{a}^\dagger
$$

$$
[\hat{N}, \hat{a}] = \hat{a}^\dagger\hat{a}\hat{a} - \hat{a}\hat{a}^\dagger\hat{a} = -[\hat{a}, \hat{a}^\dagger]\hat{a} = -\hat{a}
$$

这两条对易关系表明：$\hat{a}^\dagger$ 将量子数加一，$\hat{a}$ 将量子数减一，是升降算符名称的严格代数依据。


## 哈密顿形式

数算符的本征值谱 $n = 0, 1, 2, \dots$ 给出了分立、等间距的能级结构。要构造一个与 $\hat{N}$ 一致的哈密顿算符 $\hat{H}$，只需回答两个问题：$\hat{H}$ 与 $\hat{N}$ 是什么关系？比例常数是什么？

**第一步：$\hat{H}$ 与 $\hat{N}$ 的关系。** $\hat{H}$ 的本征值谱必须复现 $\hat{N}$ 的等间距结构。若 $\hat{H}$ 不仅是 $\hat{N}$ 的线性函数（即 $\hat{H} = \alpha\hat{N} + \beta$），则本征值间距将不再均匀，与谱结构矛盾。因此 $\hat{H}$ 只能取：

$$
\hat{H} = \alpha\hat{N} + \beta
$$

其中 $\alpha$、$\beta$ 为常数。这是谱结构对 $\hat{H}$ 形式的唯一约束。

**第二步：常数项与对称性。** 利用 $[\hat{a}, \hat{a}^\dagger] = 1$，构造对称序（Weyl 序）：

$$
\hat{a}^\dagger\hat{a} + \hat{a}\hat{a}^\dagger = 2\hat{N} + 1
$$

取 $\hat{H}$ 为对称序的半，使得产生与湮灭的贡献完全对等：

$$
\hat{H} = \frac{\alpha}{2}(\hat{a}^\dagger\hat{a} + \hat{a}\hat{a}^\dagger) = \alpha\left(\hat{N} + \frac{1}{2}\right)
$$

这一选择使得 $\hat{H}$ 在升降算符互换下不变，是最自然的对称构造。

**第三步：比例常数 $\alpha$。** 等间距谱 $n = 0, 1, 2, \dots$ 的最小非零间距为 $1$，对应最低能量量子。引入 $\omega$ 标记这一频率（即间距除以 $\hbar$），取 $\alpha = \hbar\omega$。这不是时间演化公设，而是对谱间距的命名：

$$
\hat{H} = \hbar\omega\left(\hat{N} + \frac{1}{2}\right)
$$

**能级结构。** 将 $\hat{N}\ket{n} = n\ket{n}$ 代入：

$$
\hat{H}\ket{n} = \hbar\omega\left(n + \frac{1}{2}\right)\ket{n}
$$

由此直接得出：

1. **分立能级**：$E_n = \hbar\omega(n + \tfrac{1}{2})$，$n = 0, 1, 2, \dots$——量子化是代数结构的必然结果，无需人为假设；
2. **零点能**：$E_0 = \tfrac{1}{2}\hbar\omega \neq 0$——即使基态也具有不可消除的真空能量，这正是对称序中 $\hat{a}\hat{a}^\dagger$ 贡献的 $\tfrac{1}{2}\hbar\omega$；
3. **等间距**：相邻能级差 $\Delta E = \hbar\omega$，与量子数 $n$ 无关，这是升降算符等步长结构的直接体现。

至此，纯代数公理已导出完整的量子能级——尚未引入位置、动量，能量量子化已然确立。


## 位置动量算符

上一节已从纯代数导出 $\hat{H}=\hbar\omega(\hat{N}+\frac{1}{2})$。现在反过来问：能否找到厄米算符 $\hat{x}$、$\hat{p}$，使得它们的二次型恰好还原此 $\hat{H}$？

设 $\hat{x}$、$\hat{p}$ 为 $\hat{a}^\dagger$、$\hat{a}$ 的线性组合，要求

$$
\frac{\hat{p}^2}{2m}+\frac{1}{2}m\omega^2\hat{x}^2 \stackrel{?}{=} \hbar\omega\!\left(\hat{N}+\frac{1}{2}\right)
$$

下面分三步唯一确定 $\hat{x}$、$\hat{p}$ 的形式与系数。

**第一步：实本征值定厄米形式。** $\hat{x}$、$\hat{p}$ 须还原 $\hat{H}$ 中的实本征值，因此自身的本征值必须为实——这是匹配条件的数学要求，而非外加的可观测量公设。本征值为实的算符即厄米算符。$\hat{a}$ 和 $\hat{a}^\dagger$ 各自非厄米，但 $\hat{a}^\dagger+\hat{a}$ 与 $i(\hat{a}^\dagger-\hat{a})$ 是仅有的两个独立厄米组合：

$$
(\hat{a}^\dagger+\hat{a})^\dagger = \hat{a}+\hat{a}^\dagger = \hat{a}^\dagger+\hat{a}
$$

$$
\left(i(\hat{a}^\dagger-\hat{a})\right)^\dagger = -i(\hat{a}-\hat{a}^\dagger) = i(\hat{a}^\dagger-\hat{a})
$$

因此 $\hat{x}$、$\hat{p}$ 只能取：

$$
\hat{x} = \alpha(\hat{a}^\dagger+\hat{a}),\quad \hat{p} = i\beta(\hat{a}^\dagger-\hat{a})
$$

其中 $\alpha$、$\beta$ 为正实系数，待定。

**第二步：解耦定系数比。** 将上式代入二次型，展开：

$$
\hat{x}^2 = \alpha^2(\hat{a}^{\dagger 2}+\hat{a}^2+2\hat{N}+1)
$$

$$
\hat{p}^2 = -\beta^2(\hat{a}^{\dagger 2}+\hat{a}^2-2\hat{N}-1)
$$

代入 $\frac{\hat{p}^2}{2m}+\frac{1}{2}m\omega^2\hat{x}^2$，合并同类项：

- $\hat{a}^{\dagger 2}+\hat{a}^2$ 项系数：$-\dfrac{\beta^2}{2m}+\dfrac{1}{2}m\omega^2\alpha^2$
- $\hat{N}$ 项系数：$\dfrac{\beta^2}{m}+m\omega^2\alpha^2$
- 常数项：$\dfrac{\beta^2}{2m}+\dfrac{1}{2}m\omega^2\alpha^2$

要使结果仅为 $\hat{N}$ 的线性函数，$\hat{a}^{\dagger 2}+\hat{a}^2$ 项必须消去：

$$
-\frac{\beta^2}{2m}+\frac{1}{2}m\omega^2\alpha^2 = 0 \implies \beta = m\omega\alpha
$$

唯有 $\hat{x}\propto\hat{a}^\dagger+\hat{a}$ 与 $\hat{p}\propto i(\hat{a}^\dagger-\hat{a})$ 的配对，才能令二次型中的高阶项完全抵消，回归粒子数算符的线性结构。

**第三步：匹配 $\hat{H}$ 定系数值。** 代入 $\beta = m\omega\alpha$，剩余项为：

$$
\frac{\hat{p}^2}{2m}+\frac{1}{2}m\omega^2\hat{x}^2 = m\omega^2\alpha^2(2\hat{N}+1)
$$

与 $\hat{H}=\hbar\omega(\hat{N}+\frac{1}{2})=\frac{\hbar\omega}{2}(2\hat{N}+1)$ 比较，得：

$$
m\omega^2\alpha^2 = \frac{\hbar\omega}{2} \implies \alpha = \sqrt{\frac{\hbar}{2m\omega}},\quad \beta = m\omega\alpha = \sqrt{\frac{\hbar m\omega}{2}}
$$

最终：

$$
\boxed{\hat{x} = \sqrt{\frac{\hbar}{2m\omega}}(\hat{a}^\dagger+\hat{a}),\quad \hat{p} = i\sqrt{\frac{\hbar m\omega}{2}}(\hat{a}^\dagger-\hat{a})}
$$

**验证：$[\hat{x},\hat{p}]=i\hbar$ 自动成立。** 计算：

$$
[\hat{x},\hat{p}] = i\alpha\beta\,[\hat{a}^\dagger+\hat{a},\,\hat{a}^\dagger-\hat{a}] = i\alpha\beta\cdot 2\,[\hat{a},\hat{a}^\dagger] = 2i\alpha\beta
$$

代入 $\alpha\beta = \sqrt{\dfrac{\hbar}{2m\omega}}\cdot\sqrt{\dfrac{\hbar m\omega}{2}} = \dfrac{\hbar}{2}$：

$$
[\hat{x},\hat{p}] = 2i\cdot\frac{\hbar}{2} = i\hbar \quad\checkmark
$$

正则对易关系并非独立约束，而是实本征值要求 + 解耦 + $\hat{H}$ 匹配三步之后的**自动推论**。这意味着：位置与动量不是独立于升降算符的物理量，它们是升降算符的特定厄米组合，其一切性质都源自 $[\hat{a},\hat{a}^\dagger]=1$ 这一条底层规则。

## 能级与量子态

### 激发态的封闭表达式

对易关系一节已得 $\hat{a}^\dagger\ket{n}=\sqrt{n+1}\ket{n+1}$，反复作用：

$$
\ket{1} = \hat{a}^\dagger\ket{0},\quad \ket{2} = \frac{\hat{a}^\dagger}{\sqrt{2}}\ket{1} = \frac{(\hat{a}^\dagger)^2}{\sqrt{2!}}\ket{0},\quad \dots
$$

由数学归纳法，任意激发态均可由基态生成：

$$
\boxed{\ket{n} = \frac{(\hat{a}^\dagger)^n}{\sqrt{n!}}\ket{0}}
$$

这意味着：全部量子态的信息都编码在基态 $\ket{0}$ 与产生算符 $\hat{a}^\dagger$ 之中——湮灭不创造新信息，只改变已有态的量子数。

### 不确定性关系

正则对易关系 $[\hat{x},\hat{p}]=i\hbar$ 的直接物理推论是不确定性关系。对任意态 $\ket{\psi}$，由 Robertson 不等式：

$$
\Delta x\,\Delta p \geq \frac{1}{2}|\langle[\hat{x},\hat{p}]\rangle| = \frac{\hbar}{2}
$$

对于基态 $\ket{0}$，可以验证等号成立。利用 $\hat{a}\ket{0}=0$：

$$
\langle 0|\hat{x}^2|0\rangle = \alpha^2\langle 0|(\hat{a}^{\dagger 2}+\hat{a}^2+2\hat{N}+1)|0\rangle = \alpha^2
$$

$$
\langle 0|\hat{p}^2|0\rangle = -\beta^2\langle 0|(\hat{a}^{\dagger 2}+\hat{a}^2-2\hat{N}-1)|0\rangle = \beta^2
$$

而 $\langle 0|\hat{x}|0\rangle = \langle 0|\hat{p}|0\rangle = 0$，故：

$$
\Delta x = \alpha = \sqrt{\frac{\hbar}{2m\omega}},\quad \Delta p = \beta = \sqrt{\frac{\hbar m\omega}{2}}
$$

$$
\Delta x\,\Delta p = \alpha\beta = \frac{\hbar}{2}
$$

基态恰为最小不确定态——这不是偶然，而是升降算符代数结构（$\hat{a}\ket{0}=0$ 即 $\hat{p}\ket{0} = im\omega\hat{x}\ket{0}$，位置与动量线性锁定）的必然结果。

### 基态波函数

最后，将代数结果引入位置表象。基态条件 $\hat{a}\ket{0}=0$ 在位置表象下写为：

$$
\left\langle x\middle|\hat{a}\middle|0\right\rangle = 0
$$

代入 $\hat{a} = \sqrt{\frac{m\omega}{2\hbar}}\hat{x} + \frac{i}{\sqrt{2\hbar m\omega}}\hat{p}$，并取 $\hat{p} = -i\hbar\frac{d}{dx}$：

$$
\left(\sqrt{\frac{m\omega}{2\hbar}}\,x + \sqrt{\frac{\hbar}{2m\omega}}\frac{d}{dx}\right)\psi_0(x) = 0
$$

这是一阶常微分方程，直接积分得：

$$
\psi_0(x) = \left(\frac{m\omega}{\pi\hbar}\right)^{1/4}\exp\!\left(-\frac{m\omega x^2}{2\hbar}\right)
$$

高斯型基态——无需解二阶薛定谔方程，仅由 $\hat{a}\ket{0}=0$ 一阶条件即得。激发态波函数则由 $\psi_n(x) = \frac{(\hat{a}^\dagger)^n}{\sqrt{n!}}\psi_0(x)$ 逐阶生成，自然给出 Hermite 多项式结构。

至此，一条公理的全部推论已展开。它们之间的逻辑依赖关系，将在下一节回溯梳理。

## 总结

回望整条推导链路：一条公理（$\hat{a}\ket{0}=0$，即湮灭算符湮灭基态）如同一颗种子，每一步都只有唯一去向——

$\hat{a}\ket{0}=0$ → 数算符 $\hat{N}=\hat{a}^\dagger\hat{a}$ 厄米、本征值非负、取整数 → $\{\ket{n}\}$ 正交归一是 $\hat{N}$ 厄米性的推论 → 对易关系 $[\hat{a},\hat{a}^\dagger]=1$ → 哈密顿 $\hat{H}=\hbar\omega(\hat{N}+\tfrac{1}{2})$ 由谱结构唯一确定 → 位置动量算符由实本征值要求与 $\hat{H}$ 匹配唯一确定 → 正则对易关系、不确定性关系、基态波函数均为自动推论。

传统量子力学的若干公理在此框架中的地位如下：

- **态矢量公理**（量子态是 Hilbert 空间中的矢量）：$\{\ket{n}\}$ 的正交归一性与完备性由 $\hat{N}$ 的厄米性 + 无简并谱推导，无需预设；
- **可观测量公理**（物理量对应厄米算符）：$\hat{x}$、$\hat{p}$ 的厄米性来自"实本征值"这一数学要求，而非外加的物理公设；
- **时间演化公理**（薛定谔方程 / Heisenberg 方程）：$\hat{H}$ 的形式由谱结构约束唯一确定，$\omega$ 是对谱间距的命名，无需引入时间演化公设；
- **Born 规则**与**复合系统公理**：不在本文推导范围内，是独立的物理层假设。

没有任何一步需要引入额外的物理假设，每一条结果都是前一步的代数必然。

### 结论

1. **量子化是代数的必然，而非经验的拟合。** 谐振子能级 $E_n=\hbar\omega(n+\tfrac{1}{2})$ 完全由 $[\hat{a},\hat{a}^\dagger]=1$ 的谱结构决定，无需人为附加量子化条件；
2. **位置与动量是派生量，而非本源量——至少在谐振子框架内如此。** $\hat{x}$、$\hat{p}$ 的形式由实本征值要求、解耦条件与 $\hat{H}$ 匹配三步唯一确定，正则对易关系 $[\hat{x},\hat{p}]=i\hbar$ 是自动推论而非独立公设。更一般的量子系统是否同样可由升降算符派生，则是值得追问的开放问题；
3. **升降算符的对易规则是可推广的公理化起点。** 类似的代数结构——角动量的 $\hat{J}_\pm$、量子场论中多模式 Fock 空间的产生湮灭算符——虽对易关系与谱结构各有差异，但「由算符代数确定物理」这一公理化路径是相通的。本文的谐振子推导是这一路径最简洁的范例。
