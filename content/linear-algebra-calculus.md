---
title: "线性代数视角下的微积分"
date: 2026-07-20
description: 可微性的本质不是"变化率"，而是非线性映射在局部被线性映射精确逼近。微分 df_p 不是数，是切空间之间的线性变换。
tags: ["数学", "微积分", "线性代数"]
draft: false
math: true
diagram: false
---

微积分通常从切线斜率或瞬时变化率开始讲，这些直觉都没问题。但换个角度看，可微性还有更深的一层：它在说一个非线性映射，在局部可以用线性映射来逼近，而且逼近到一阶是精确的。微分 $df_p$ 本身不是什么数——它是一个线性变换。

先从一个具体的例子开始。

---

## §1 从一个例子到线性变换

考虑函数 $f(x) = x^2$ 在 $x = 3$ 处。切线斜率为 $f'(3) = 6$。

传统写法是 $dy = 6\,dx$，但这个公式在说什么？关键不在"变化率"，而在 $f$ 在 $x=3$ 附近可以用线性映射来逼近：

$$f(3 + h) = (3+h)^2 = 9 + 6h + h^2 = f(3) + 6h + o(h)$$

去掉高阶无穷小后剩下的 $v \mapsto 6v$ 就是一个线性变换 $T: \mathbb{R} \to \mathbb{R}$，系数 $6$ 是它在基 $\{1\}$ 下的矩阵。这个由局部逼近定义的线性映射记作 $df_3$：

$$df_3(v) = 6 \cdot v$$

它在方向 $v$ 上的取值，恰好等于沿 $v$ 走一步的瞬时变化率，即方向导数：

$$df_p(v) = \lim_{t \to 0} \frac{f(p+tv) - f(p)}{t}$$

当 $f(x)=x^2,\ p=3$ 时代入得 $6v$。注意这里有一个层次：$df_p$ 由局部逼近定义，极限公式只是算它在某个方向上的值。推广到高维时，同一套逻辑照旧。

---

## §2 线性空间与线性变换

线性代数的研究对象是线性空间及其上的线性变换。设 $V, W$ 为实数域 $\mathbb{R}$ 上的向量空间，一个映射 $T: V \to W$ 若满足：

$$T(a\boldsymbol{u} + b\boldsymbol{v}) = aT(\boldsymbol{u}) + bT(\boldsymbol{v}), \quad \forall \boldsymbol{u},\boldsymbol{v} \in V,\ \forall a,b \in \mathbb{R}$$

则称 $T$ 为线性变换。它的特征是：有限维时，选定基就可以等价地写成矩阵乘法。线性变换没有高阶无穷小——它是一刀切的结构，在全局均匀成立。

到这里，线性变换还只是一个**静态的代数结构**，跟极限和邻域没有关系。下一步的问题是：非线性映射能不能和它扯上关系？

---

## §3 可微性的本质：局部线性化

非线性映射不能被线性变换全局表示。但如果它光滑，则在每一点 $p$ 的充分小邻域内，非线性偏差是高于一阶的微量——可以被一个线性变换**逼近到一阶**。这就是可微性的形式化定义。

先从最简单的情况说起。欧氏空间 $\mathbb{R}^n$ 本身就是一个光滑流形——它在每一点的切空间 $T_p\mathbb{R}^n$ 与 $\mathbb{R}^n$ 线性同构。在 $\mathbb{R}^n$ 上讨论微分时，切空间就等于 $\mathbb{R}^n$ 自己，方向向量 $v$ 就是一个普通向量。

设 $f: \mathbb{R}^m \to \mathbb{R}^n$ 在点 $p$ 处可微，其精确定义是：存在唯一的线性映射

$$df_p: \mathbb{R}^m \to \mathbb{R}^n$$

使得当 $\boldsymbol{h} \to \boldsymbol{0}$ 时，

$$\frac{\|\,f(p+\boldsymbol{h}) - f(p) - df_p(\boldsymbol{h})\,\|}{\|\boldsymbol{h}\|} \to 0$$

这样就得到了线性映射 $df_p$ 把 $f$ 在 $p$ 处逼近到一阶的式子，余项是高阶的。

由此可推出 $df_p$ 在任一方向上的取值恰为方向导数：在上式中令 $\boldsymbol{h}=tv$ 并让 $t\to 0$，即得

$$df_p(v) = \lim_{t \to 0} \frac{f(p+tv) - f(p)}{t}$$

这里有层关系需要注意：**可微性由逼近式定义，方向导数公式是它的推论**，反过来不行。各方向导数都存在、甚至关于 $v$ 线性，也不保证可微。举个例子：$f(x,y)=\dfrac{x^3 y}{x^4+y^2}$（原点补 $0$）在原点连续，沿任何方向的方向导数都是 $0$（零映射自然线性），但沿 $y=x^2,\ x\to 0^+$ 有 $\dfrac{f(x,x^2)}{\|(x,x^2)\|}\to \dfrac{1}{2}\neq 0$，不可微。差别在于：逼近式要求余项在 $\|\boldsymbol{h}\|\to 0$ 时**一致地**消失，这比"每个方向上的极限存在"更强。

关键在于：**$df_p$ 本身是一个线性变换**，满足：

$$df_p(a\boldsymbol{v}_1 + b\boldsymbol{v}_2) = a\,df_p(\boldsymbol{v}_1) + b\,df_p(\boldsymbol{v}_2)$$

不管 $f$ 有多非线性，它在一点处的微分总是一个线性映射。所有一阶微分算子（梯度、偏导数、方向导数）的线性性，都来自 $df_p$，不需要一个个去证。

> **注**：上述讨论限定在欧氏空间 $\mathbb{R}^n$ 上。更一般地，对于任意光滑流形 $M, N$，点 $p$ 处可以定义切空间 $T_pM$（一个与流形同样维数的线性空间，直观上是"所有经过 $p$ 点的光滑曲线的方向向量的全体"），而微分 $df_p: T_pM \to T_{f(p)}N$ 仍然是一个线性变换。$\mathbb{R}^n$ 是最简单的流形，它的一切性质都是这个一般框架的坐标特化。

---

## §4 一元微分：一维线性变换的特例

现在回看一元微积分，它只是上面一般理论在最简情况下的退化。

当 $f: \mathbb{R} \to \mathbb{R}$ 时，切空间 $T_p\mathbb{R}$ 与 $\mathbb{R}$ 自身同构——它是一个一维线性空间。一维线性空间到自身的线性变换只有一种形式：数乘。因此 $df_p$ 由唯一常数 $f'(p)$ 完全表征：

$$df_p(v) = f'(p) \cdot v, \quad \forall v \in \mathbb{R}$$

这就是开头 $x^2$ 在 $x=3$ 处 $df_3(v) = 6v$ 的一般形式。一元导数 $f'(p)$ 的本质不是"变化率"，而是**一维线性变换在基下的唯一表征系数**。传统写法 $dy = f'(x)dx$ 就是这个线性变换在一维情形的坐标展开。

可见，一元微分的全部结构都嵌在一般的线性映射理论里，不独立。

---

## §5 多元微分：高维线性映射的坐标矩阵

到了高维，事情不变——微分始终是线性变换，只是维数高了需要写矩阵。

设 $f: \mathbb{R}^m \to \mathbb{R}^n$ 在点 $p$ 处可微。$df_p: \mathbb{R}^m \to \mathbb{R}^n$ 是一个 $m \to n$ 维的线性变换。根据线性代数基本定理，选取 $\mathbb{R}^m$ 的标准基 $\{\boldsymbol{e}_1,\dots,\boldsymbol{e}_m\}$ 和 $\mathbb{R}^n$ 的标准基后，这个线性变换有一个 $n \times m$ 的矩阵表示。矩阵的每一列，就是基向量 $\boldsymbol{e}_j$ 在 $df_p$ 下的像：

$$df_p(\boldsymbol{e}_j) = \lim_{t \to 0} \frac{f(p + t\boldsymbol{e}_j) - f(p)}{t} = \begin{pmatrix}\frac{\partial f^1}{\partial x_j}(p) \\ \vdots \\ \frac{\partial f^n}{\partial x_j}(p)\end{pmatrix}$$

这个矩阵就是雅可比矩阵：

$$J_f(p) =
\begin{pmatrix}
\displaystyle\frac{\partial f^1}{\partial x_1} & \dots & \displaystyle\frac{\partial f^1}{\partial x_m} \\
\vdots & & \vdots \\
\displaystyle\frac{\partial f^n}{\partial x_1} & \dots & \displaystyle\frac{\partial f^n}{\partial x_m}
\end{pmatrix}$$

对任意方向向量 $\boldsymbol{v} = (v_1,\dots,v_m)$，由线性性质：

$$df_p(\boldsymbol{v}) = df_p\!\left(\sum_j v_j \boldsymbol{e}_j\right) = \sum_j v_j\,df_p(\boldsymbol{e}_j) = \sum_j v_j \begin{pmatrix}\frac{\partial f^1}{\partial x_j} \\ \vdots \\ \frac{\partial f^n}{\partial x_j}\end{pmatrix} = J_f(p)\,\boldsymbol{v}$$

或者写成大家熟悉的微分形式 $d\boldsymbol{y} = J_f(p)\,d\boldsymbol{x}$。

这个推导就是多元微分的代数结构：**全微分是高维空间中的线性变换。** 偏导数 $\frac{\partial f^i}{\partial x_j}$ 只是线性变换在坐标基下的分量——光看分量看不清 $f$ 在该点的局部线性结构，组合成 $J_f$ 才还原出一个完整的线性变换。

---

## §6 链法则：微分作为函子

$df_p$ 是一个线性映射。线性映射之间最自然的运算是复合——给两个可微映射，复合后的微分就是微分的复合。这不是需要背的公式，而是一句几乎免费的观察——线性映射复合之后仍然是线性映射，而微分就是线性映射。

设 $f:\mathbb{R}^m\to\mathbb{R}^n$ 在 $p$ 可微，$g:\mathbb{R}^n\to\mathbb{R}^k$ 在 $f(p)$ 可微。复合 $g\circ f$ 在 $p$ 处的微分就是：

$$d(g\circ f)_p = dg_{f(p)}\circ df_p.$$

代入逼近式验证：把 $f(p+\boldsymbol{h})=f(p)+df_p(\boldsymbol{h})+o(\|\boldsymbol{h}\|)$ 代入 $g$ 的逼近式，线性部分层层复合，高阶余项被吸收，剩下的线性映射就是 $dg_{f(p)}\circ df_p$。写成坐标语言，就是雅可比矩阵相乘：

$$J_{g\circ f}(p)=J_g(f(p))\,J_f(p).$$

所以链法则不是多出来的规则，是线性映射复合封闭性的直接后果。$f\mapsto df$ 把光滑映射送到线性映射，并且保持复合与恒等——从范畴论的角度看，满足 $F(\mathrm{id})=\mathrm{id}$ 和 $F(g\circ f)=F(g)\circ F(f)$ 的映射叫作**函子**，$d$ 正好符合。写成 $J_{g\circ f}=J_g\,J_f$ 或者 $d(g\circ f)=dg\circ df$，归根结底都是线性代数。

---

## §7 约束极值：拉格朗日乘数法作为线性代数推论

这个框架还有一个直观的应用：拉格朗日乘数法可以从线性代数直接推出来。

考虑在约束 $g(x)=c$ 下求 $f$ 的极值，$g:\mathbb{R}^n\to\mathbb{R}$，设 $p$ 是正则点（$dg_p\neq 0$，即 $\nabla g(p)\neq 0$）。约束面 $S=g^{-1}(c)$ 在 $p$ 附近是一张光滑曲面。对 $S$ 上任一过 $p$ 的曲线 $\gamma$，$g\circ\gamma\equiv c$，用链法则：

$$dg_p(\gamma'(0)) = (g\circ\gamma)'(0) = 0$$

所以 $T_pS\subseteq\ker dg_p$。由 $dg_p$ 是秩 $1$ 的线性泛函知 $\dim\ker dg_p=n-1=\dim S$，因此

$$T_pS = \ker dg_p.$$

约束面的切空间，就是约束微分的核。

极值条件：$p$ 是 $f|_S$ 的极值点，等价于 $d(f|_S)_p=0$。限制的微分就是微分的限制，所以

$$df_p\big|_{T_pS}=0 \iff T_pS\subseteq\ker df_p \iff \ker dg_p\subseteq\ker df_p.$$

用一句话：每个约束允许的方向，$f$ 沿它走到一阶都不动。

最后收尾的是一条线性代数引理：若线性泛函 $\alpha,\beta:V\to\mathbb{R}$ 满足 $\ker\alpha\subseteq\ker\beta$，则 $\beta=\lambda\alpha$（取 $u$ 使 $\alpha(u)\neq0$，对任意 $v$ 由 $v-\dfrac{\alpha(v)}{\alpha(u)}u\in\ker\alpha\subseteq\ker\beta$ 即解出）。代入 $\alpha=dg_p,\ \beta=df_p$：

$$df_p = \lambda\, dg_p \iff \nabla f(p)=\lambda\,\nabla g(p).$$

这就得到了拉格朗日乘数法——$\lambda$ 是核包含关系所保证的线性相关系数。多约束 $g=(g_1,\dots,g_k):\mathbb{R}^n\to\mathbb{R}^k$（$Dg_p$ 满秩 $k$）同理：$T_pS=\bigcap_i\ker dg_i^p=\ker Dg_p$，极值条件 $\ker Dg_p\subseteq\ker df_p$，由分解引理给出 $df_p=\sum_i\lambda_i\,dg_i^p$，即 $\nabla f=\sum_i\lambda_i\nabla g_i$。

> **几何读法**：在 $\mathbb{R}^n$ 里 $df_p(v)=\langle\nabla f,v\rangle$，$\ker dg_p=(\operatorname{span}\nabla g)^\perp$。条件 $\ker dg_p\subseteq\ker df_p$ 等价于 $\nabla f$ 与约束面切向量正交，即 $\nabla f$ 落在法空间 $\operatorname{span}\nabla g$ 里。这就是 $\nabla f=\lambda\nabla g$ 的几何解释。用核表述不依赖内积，在任意光滑流形上也成立。

举个最小的例子：求原点到椭圆 $\dfrac{x^2}{a^2}+\dfrac{y^2}{b^2}=1$ 的最远点，即极大化 $f=x^2+y^2$ 受约束 $g=\dfrac{x^2}{a^2}+\dfrac{y^2}{b^2}-1=0$。$\nabla f=(2x,2y)$，$\nabla g=\left(\dfrac{2x}{a^2},\dfrac{2y}{b^2}\right)$，条件 $\nabla f=\lambda\nabla g$ 给 $2x=\lambda\dfrac{2x}{a^2}$、$2y=\lambda\dfrac{2y}{b^2}$，非平凡解只在 $x=0$ 或 $y=0$ 处，对应 $(\pm a,0)$ 与 $(0,\pm b)$，最远点是长轴端点。

再看一个把线性代数和微积分串到一起的例子：在单位球面 $\|x\|=1$ 上极大化二次型 $f(x)=x^TAx$（$A$ 对称）。$\nabla f=2Ax$，$\nabla g=2x$（$g=x^Tx-1$），乘数条件给出 $Ax=\lambda x$——**约束极值的临界点恰好是 $A$ 的特征向量，乘数 $\lambda$ 是特征值**。极值处 $f=x^TAx=\lambda\,x^Tx=\lambda$，所以二次型在单位球上的最大、最小值就是 $A$ 的最大、最小特征值。用核的语言看：球面切空间 $T_pS=\ker dg_p=x^\perp$，极值要求 $df_p$ 在 $x^\perp$ 上为零，即 $Ax\in\operatorname{span}\{x\}$，正是特征方程。

---

## §8 结论

线性代数为微积分的"局部线性化"提供了语言和工具。微积分用这套语言描述非线性映射的局部行为：$df_p$ 是切空间之间的线性映射，在 $\mathbb{R}^n$ 上是雅可比矩阵左乘方向向量，在一维退化为数乘。一元和多元不是两种微积分——是同一个线性变换框架在高低维下的不同展开。

约束极值也一样：拉格朗日乘数法可以归结为 ${}T_pS=\ker dg_p$ 和 $\ker dg_p\subseteq\ker df_p$，加上一条核包含引理得到 ${}\nabla f=\lambda\nabla g$。梯度、偏导数、方向导数的线性性都来自 $df_p$。从 $x^2$ 在 $x=3$ 处 $df = 6\,dx$，到 $d(g\circ f)=dg\circ df$，到全微分矩阵，再到 $\nabla f=\lambda\nabla g$，是同一套逻辑。
