---
title: "线性代数视角下的微积分"
date: 2026-07-20
description: 可微性的本质不是"变化率"，而是非线性映射在局部被线性映射精确逼近。微分 df_p 不是数，是切空间之间的线性变换。
tags: ["数学", "微积分", "线性代数"]
draft: false
math: true
diagram: false
---

微积分教材通常告诉你：导数是切线的斜率，或是瞬时变化率。这些直觉没有错，但没说到根上。从现代数学的视角看，微积分的可微性理论并非自己独立成立——它是线性变换理论在无穷小邻域下的自然延伸。可微性的本质是：非线性映射在局部可以被线性映射精确逼近。微分 $df_p$ 不是一个数，而是定义在切空间上的一个**线性变换**。

下面从一个简单的例子出发，逐步推导一元微分与多元微分如何统一于线性代数。

---

## 一、从一个例子到线性变换

考虑函数 $f(x) = x^2$ 在 $x = 3$ 处。切线斜率为 $f'(3) = 6$。

传统写法是 $dy = 6\,dx$，但这个公式在说什么？关键不在于"变化率"，而在于：$f$ 在 $x=3$ 附近可以用一个线性映射来逼近。事实上，

$$f(3 + h) = (3+h)^2 = 9 + 6h + h^2 = f(3) + 6h + o(h)$$

去掉高阶无穷小 $o(h)$ 后剩下的部分 $v \mapsto 6v$，就是一个**线性变换** $T: \mathbb{R} \to \mathbb{R}$，满足 $T(av_1 + bv_2) = aT(v_1) + bT(v_2)$。系数 $6$ 是它在标准基 $\{1\}$ 下的矩阵表示。我们把这个由"局部逼近"定义出来的线性映射记作 $df_3$：

$$df_3(v) = 6 \cdot v$$

它在方向 $v$ 上的取值，恰好等于沿 $v$ 走一步的瞬时变化率，即方向导数：

$$df_p(v) = \lim_{t \to 0} \frac{f(p+tv) - f(p)}{t}$$

当 $f(x)=x^2,\ p=3$ 时，代入上式就得到 $6v$。注意这里的层次：$df_p$ 的身份由"局部逼近"赋予，极限公式只是计算它在某个方向上取值的手段。当 $p, v$ 推广为高维向量时，同一套逻辑依然成立——而这正是线性代数进入微积分的入口。

---

## 二、线性空间与线性变换

线性代数的研究对象是线性空间及其上的线性变换。设 $V, W$ 为实数域 $\mathbb{R}$ 上的向量空间，一个映射 $T: V \to W$ 若满足：

$$T(a\boldsymbol{u} + b\boldsymbol{v}) = aT(\boldsymbol{u}) + bT(\boldsymbol{v}), \quad \forall \boldsymbol{u},\boldsymbol{v} \in V,\ \forall a,b \in \mathbb{R}$$

则称 $T$ 为线性变换。它的核心特征是：在有限维空间中，任意线性变换可以通过选取基等价表示为矩阵乘法。线性变换没有高阶无穷小误差——它是全局均匀的结构映射。

到这里，线性变换还只是一个**静态的代数结构**，不涉及极限和邻域。下一步的关键问题是：非线性映射能不能和线性变换扯上关系？

---

## 三、可微性的本质：局部线性化

一个非线性映射无法被线性变换全局表示，但如果它是光滑的，则有一个关键性质：在任意一点 $p$ 的充分小邻域内，非线性偏差是高阶无穷小，可以被一个线性变换**精确逼近**到一阶。这正是可微性的形式化定义。

先从最简单的空间说起。欧氏空间 $\mathbb{R}^n$ 本身就是最基础的光滑流形——它在每一点的"切空间" $T_p\mathbb{R}^n$ 与 $\mathbb{R}^n$ 自身线性同构。换句话说，在 $\mathbb{R}^n$ 上讨论微分，切空间就是 $\mathbb{R}^n$ 本身，方向向量 $v$ 就是 $\mathbb{R}^n$ 中的一个普通向量。

设 $f: \mathbb{R}^m \to \mathbb{R}^n$ 在点 $p$ 处可微，其精确定义是：存在唯一的线性映射

$$df_p: \mathbb{R}^m \to \mathbb{R}^n$$

使得当 $\boldsymbol{h} \to \boldsymbol{0}$ 时，

$$\frac{\|\,f(p+\boldsymbol{h}) - f(p) - df_p(\boldsymbol{h})\,\|}{\|\boldsymbol{h}\|} \to 0$$

即 $f(p+\boldsymbol{h}) = f(p) + df_p(\boldsymbol{h}) + o(\|\boldsymbol{h}\|)$——线性映射 $df_p$ 把 $f$ 在 $p$ 处的局部行为逼近到一阶，余项是高阶无穷小。这才是"局部线性化"的本义。

由此可推出 $df_p$ 在任一方向上的取值恰为方向导数：在上式中令 $\boldsymbol{h}=tv$ 并让 $t\to 0$，即得

$$df_p(v) = \lim_{t \to 0} \frac{f(p+tv) - f(p)}{t}$$

这里有一个常被忽略的层次关系：**可微性由逼近式定义，方向导数公式只是其推论**，反之并不成立。仅凭各方向导数存在、甚至关于 $v$ 线性，并不能推出可微性——例如 $f(x,y)=\dfrac{x^3 y}{x^4+y^2}$（原点处补 $0$）在原点连续，且沿任何方向的方向导数都等于 $0$（零映射自然线性），但沿路径 $y=x^2,\ x\to 0^+$ 有 $\dfrac{f(x,x^2)}{\|(x,x^2)\|}\to \dfrac{1}{2}\neq 0$，故不可微。差别在于：逼近式要求余项在 $\|\boldsymbol{h}\|\to 0$ 时**一致地**消失，比"每个方向上的极限存在"更强。

关键在于：**$df_p$ 本身是一个线性变换**，满足：

$$df_p(a\boldsymbol{v}_1 + b\boldsymbol{v}_2) = a\,df_p(\boldsymbol{v}_1) + b\,df_p(\boldsymbol{v}_2)$$

不管 $f$ 本身有多非线性，它在一点处的微分总是一个完美的线性映射。所有一阶微分算子（梯度、偏导数、方向导数）的线性性，都继承于此，不需要单独证明。

> **注**：上述讨论限定在欧氏空间 $\mathbb{R}^n$ 上。更一般地，对于任意光滑流形 $M, N$，点 $p$ 处可以定义切空间 $T_pM$（一个与流形同样维数的线性空间，直观上是"所有经过 $p$ 点的光滑曲线的方向向量的全体"），而微分 $df_p: T_pM \to T_{f(p)}N$ 仍然是一个线性变换。$\mathbb{R}^n$ 是最简单的流形，它的一切性质都是这个一般框架的坐标特化。

---

## 四、一元微分：一维线性变换的特例

现在回看一元微积分，它只是上面一般理论在最简情况下的退化。

当 $f: \mathbb{R} \to \mathbb{R}$ 时，切空间 $T_p\mathbb{R}$ 与 $\mathbb{R}$ 自身同构——它是一个一维线性空间。一维线性空间到自身的线性变换只有一种形式：数乘。因此 $df_p$ 由唯一常数 $f'(p)$ 完全表征：

$$df_p(v) = f'(p) \cdot v, \quad \forall v \in \mathbb{R}$$

这就是开头 $x^2$ 在 $x=3$ 处 $df_3(v) = 6v$ 的一般形式。一元导数 $f'(p)$ 的本质不是"变化率"，而是**一维线性变换在基下的唯一表征系数**。传统写法 $dy = f'(x)dx$ 就是这个线性变换在一维情形的坐标展开。

可见，一元微分的全部结构都包含在一般线性映射理论之中，它没有任何独立的特殊性。

---

## 五、多元微分：高维线性映射的坐标矩阵

到了高维，事情依然不变——微分始终是线性变换，只是维数高了需要矩阵来写。

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

这个推导直接写出了多元微分的代数本质：**全微分就是高维空间中的线性变换。** 偏导数 $\frac{\partial f^i}{\partial x_j}$ 只是这个线性变换在坐标基下的分量——单独看它们并不能刻画 $f$ 在该点的局部线性结构，只有组合成 $J_f$ 才还原出完整的线性变换。

---

## 六、链法则：微分作为函子

$df_p$ 是一个线性映射。线性映射之间最自然的运算就是复合——两个线性映射首尾相接，得到一个新的线性映射。这几乎注定了链法则的出现：给两个可微映射，它们的复合的微分，自然就是它们微分的复合。不再是需要记忆的计算规则，而是一条几乎免费的观察：微分的复合就是复合的微分。

设 $f:\mathbb{R}^m\to\mathbb{R}^n$ 在 $p$ 可微，$g:\mathbb{R}^n\to\mathbb{R}^k$ 在 $f(p)$ 可微。复合映射 $g\circ f$ 在 $p$ 处的微分就是两个线性微分的复合：

$$d(g\circ f)_p = dg_{f(p)}\circ df_p.$$

从逼近式直接读出：把 $f(p+\boldsymbol{h})=f(p)+df_p(\boldsymbol{h})+o(\|\boldsymbol{h}\|)$ 代入 $g$ 的逼近式，线性部分层层复合、高阶余项仍被吸收，剩下的线性映射恰好是 $dg_{f(p)}\circ df_p$。换成坐标语言，它就是雅可比矩阵的乘法：

$$J_{g\circ f}(p)=J_g(f(p))\,J_f(p).$$

所以"复合函数求导"不是一条独立公理，而是线性映射在复合下封闭——准确说，$f\mapsto df$ 是一个从光滑映射到线性映射、保持复合与恒等的结构保持映射（范畴论中满足 $F(\mathrm{id})=\mathrm{id}$、$F(g\circ f)=F(g)\circ F(f)$ 的映射称为**函子**，$d$ 恰好满足这两个条件）。这解释了链法则为何处处成立、无需逐案证明：它就是线性代数的结构自身。

---

## 七、约束极值：拉格朗日乘数法作为线性代数推论

上面建立的框架有一个直接而漂亮的应用：它把拉格朗日乘数法从"一个要记的技巧"变成一句线性代数。

考虑在约束 $g(x)=c$ 下求 $f$ 的极值，$g:\mathbb{R}^n\to\mathbb{R}$，设 $p$ 是约束正则点（$dg_p\neq 0$，即 $\nabla g(p)\neq 0$）。约束曲面 $S=g^{-1}(c)$ 在 $p$ 附近是一张光滑曲面。它的切空间是什么？对 $S$ 上任一过 $p$ 的曲线 $\gamma$，$g\circ\gamma\equiv c$ 恒为常数，用链法则（无非是两个线性微分的复合）：

$$dg_p(\gamma'(0)) = (g\circ\gamma)'(0) = 0$$

故 $T_pS\subseteq\ker dg_p$；再由 $dg_p$ 是秩 $1$ 的线性泛函知 $\dim\ker dg_p=n-1=\dim S$，于是

$$T_pS = \ker dg_p.$$

这是上节视角的直接产物：**约束曲面的切空间，就是约束微分的核**——"被约束允许的方向"与"被 $dg_p$ 杀掉的方向"是同一回事。

接着看极值条件。$p$ 是 $f|_S$ 的极值点，等价于 $p$ 是 $f|_S$ 的临界点，即 $d(f|_S)_p=0$。而限制的微分就是微分的限制（$f|_S=f\circ\iota$，$d(f|_S)_p=df_p\big|_{T_pS}$），所以

$$df_p\big|_{T_pS}=0 \iff T_pS\subseteq\ker df_p \iff \ker dg_p\subseteq\ker df_p.$$

用话说：每个被约束允许的方向，都是 $f$ 到一阶不变的方向——你沿约束面走，$f$ 动不了。

最后是一条线性代数引理收尾：若线性泛函 $\alpha,\beta:V\to\mathbb{R}$ 满足 $\ker\alpha\subseteq\ker\beta$，则 $\beta=\lambda\alpha$（取 $u$ 使 $\alpha(u)\neq0$，对任意 $v$ 由 $v-\dfrac{\alpha(v)}{\alpha(u)}u\in\ker\alpha\subseteq\ker\beta$ 即解出）。代入 $\alpha=dg_p,\ \beta=df_p$：

$$df_p = \lambda\, dg_p \iff \nabla f(p)=\lambda\,\nabla g(p).$$

拉格朗日乘数法就这样被推出来——$\lambda$ 不是被请来的常数，而是"两个核相含"逼出来的线性相关系数。多约束 $g=(g_1,\dots,g_k):\mathbb{R}^n\to\mathbb{R}^k$（正则即 $Dg_p$ 满秩 $k$）完全同理：$T_pS=\bigcap_i\ker dg_i^p=\ker Dg_p$，极值条件 $\ker Dg_p\subseteq\ker df_p$，由分解引理给出 $df_p=\sum_i\lambda_i\,dg_i^p$，即 $\nabla f=\sum_i\lambda_i\nabla g_i$。

> **几何读法**：在 $\mathbb{R}^n$ 里 $df_p(v)=\langle\nabla f,v\rangle$，$\ker dg_p=(\operatorname{span}\nabla g)^\perp$。条件 $\ker dg_p\subseteq\ker df_p$ 等价于 $\nabla f$ 与约束面的每个切向量正交，即 $\nabla f$ 落在法空间 $\operatorname{span}\nabla g$ 中——"梯度平行于约束法向"的图景只是核这条代数在欧氏结构下的坐标投影，而核版本不依赖内积，在任意光滑流形上照样成立。

举个最小的例子：求原点到椭圆 $\dfrac{x^2}{a^2}+\dfrac{y^2}{b^2}=1$ 的最远点，即极大化 $f=x^2+y^2$ 受约束 $g=\dfrac{x^2}{a^2}+\dfrac{y^2}{b^2}-1=0$。$\nabla f=(2x,2y)$，$\nabla g=\left(\dfrac{2x}{a^2},\dfrac{2y}{b^2}\right)$，条件 $\nabla f=\lambda\nabla g$ 给 $2x=\lambda\dfrac{2x}{a^2}$、$2y=\lambda\dfrac{2y}{b^2}$，非平凡解只在 $x=0$ 或 $y=0$ 处出现，对应 $(\pm a,0)$ 与 $(0,\pm b)$，最远点即长轴端点。几何上：在最远处椭圆的法向（$\nabla g$）正好指向径向（$\nabla f$），两个核重合。

再来一个把两个主角合拢的例子：在单位球面 $\|x\|=1$ 上极大化二次型 $f(x)=x^TAx$（$A$ 对称）。$\nabla f=2Ax$，$\nabla g=2x$（$g=x^Tx-1$），乘数条件给出 $Ax=\lambda x$——**约束极值的临界点恰是 $A$ 的特征向量，乘数 $\lambda$ 恰是特征值**。而极值处 $f=x^TAx=\lambda\,x^Tx=\lambda$，故二次型在单位球上的最大/最小值就是 $A$ 的最大/最小特征值。用核的语言重读：球面切空间 $T_pS=\ker dg_p=x^\perp$，极值要求 $df_p$ 在 $x^\perp$ 上为零，即 $Ax\in\operatorname{span}\{x\}$，正是特征方程。线性代数的特征值问题，在这里作为微积分的约束极值自然浮出——两个主角在结尾合龙。

---

## 八、结论

从数学结构的层级关系看，线性代数是微积分的代数基础：它为"局部线性化"提供了语言和工具。微积分是线性结构对非线性映射的局部逼近理论：可微性就是局部线性可逼近，微分 $df_p$ 就是切空间之间的线性映射——在 $\mathbb{R}^n$ 上，它就是雅可比矩阵左乘方向向量；在一维情形，它退化为数乘。

一元微积分退化为一维线性变换的特例，多元微积分展开为高维线性变换的坐标矩阵实现。约束极值也不例外：拉格朗日乘数法不过是"约束切空间等于约束微分的核、极值要求 $df_p$ 在其上为零"加上一条核包含引理的必然结果，$\nabla f=\lambda\nabla g$ 是被推出来的而非凑出来的。整个理论完全自洽，没有需要额外解释的新概念——梯度、偏导数、方向导数的线性性都是 $df_p$ 的固有属性。从 $x^2$ 在 $x=3$ 处 $df = 6\,dx$，到复合映射的 $d(g\circ f)=dg\circ df$，到 $\mathbb{R}^m \to \mathbb{R}^n$ 的全微分矩阵，再到约束极值处的 $\nabla f=\lambda\nabla g$，同一套逻辑贯穿始终。
