---
title: '离散数学片羽 | 离散 Fourier 变换'
publishDate: 2026-06-22 15:44:23
description: 'DFT 是有限 Abel 群上的 Fourier 变换，是计算机科学中常用的数学工具。'
tags:
  - 'DFT'
  - '数学'
heroImage: { src: './feibi1.png', color: '#B4C6DA' }
language: '中文'
---

离散数学与结构 (2025Fall) 的个人笔记，摘出来一些比较好玩的东西。

## 离散 Fourier 变换

离散 Fourier 变换是在一个有限 Abel 群上作 Fourier 变换．设 $G$ 是有限 Abel 群，$|G| = n$．根据有限生成 Abel 群的结构定理，我们知道存在 $n_1, \dots, n_k$，使得 $G \cong \mathbb{Z}_{n_1} \times \dots \times \mathbb{Z}_{n_k}$．类比一般的 Fourier 变换，我们想要将任意函数 $f: G \to \mathbb{C}$ 分解成另外一些“周期”函数的和．回顾已有的知识，我们知道函数空间 $\{f | f: G \to \mathbb{C}\}$ 是 $n$ 维 $\mathbb{C}$-向量空间，带有复内积 $\langle f, g\rangle = \sum_{x \in G} f(x)\bar{g}(x)$．一般的 Fourier 变换以 $\sin$ 和 $\cos$ 作为基底，而这里我们来关注这样的对象：

**定义**：沿用上面的记号．群同态 $\chi: G \to \mathbb{S}^1$ 称为 **character**，这里 $\mathbb{S}^1 = \{z \in \mathbb{C} | |z| = 1\}$．特别地，给定 $a \in G$，定义 $\chi_a(x) = \omega_{n_1}^{a_1 x_1} \dots \omega_{n_k}^{a_k x_k} = \prod_j {\mathrm{e}}^{\frac{2\pi \mathrm{i}}{n_j}a_j x_j}$，这里 $\omega_d = \mathrm{e}^{\frac{2\pi \mathrm{i}}{d}}$，$a_i$ 和 $x_i$ 是 $a$ 和 $x$ 在 $\mathbb{Z}_{n_1} \times \dots \times \mathbb{Z}_{n_k}$ 中的同构像．

所有的 $\chi_a$ 构成一个群 $\widehat{G} = \{\chi_a | a \in G\}$，群运算为 $\chi_a \cdot \chi_b = \chi_{a+b}$．此时群的单位元是 $\chi_0 = \mathrm{id}$ （“0” 指的是全零向量所对应的群元素），逆运算为 $\chi_a^{-1} = \chi_{-a}$．对任意 $a$ 和 $x$，由于 $\chi_a(x)$ 模长为 $1$，故 $\overline{\chi_a(x)} = (\chi_{a}(x))^{-1} = \chi_{-a}(x)$，从定义中还不难看出 $\chi_a(x) = \chi_x(a)$．

我们关心是否 $\chi_a$ 已经是所有的 character，这需要以下性质：

**引理**：对任意的 character $\chi$，有
$$
\sum_x \chi(x) = \begin{cases}
|G|, & \text{if } \chi = \chi_0 \\
0, & \text{otherwise}
\end{cases}
$$

**证明**：如果 $\chi = \chi_0$，那么恒有 $\chi(x) = 1$．假设 $\exists x^*, \chi(x^*) \neq 1$，记 $S = \sum_x \chi(x)$，那么
$$
\chi(x^*) S = \sum_x \chi(x^* x) = \sum_x \chi(x) = S \Rightarrow S = 0
$$

因此对任意 character $\chi$ 和 $\chi'$，它们的内积
$$
\langle \chi, \chi' \rangle = \sum_x \chi(x) {\chi'(x)}^{-1} = \sum_x (\chi \chi'^{-1})(x)
$$
要么为 $n$ 要么为 $0$，且当且仅当 $\chi = \chi'$ 时为 $0$，故所有 character 两两正交．由于 $\chi_a$ 已经有 $n$ 个，达到了向量空间的维数，因此 $\chi_a$ 就是全部的 character，它们构成了函数空间的一组正交基，所有的函数 $f: G \to \mathbb{C}$ 就可以使用这组基来分解：

**定义**：对于任意函数 $f: G \to \mathbb{C}$，$f = \sum_{a \in G} \hat{f}(a) \chi_a$，$\hat{f}(a)$ 称为 $f$ 的 **Fourier 系数**．

Fourier 系数可以被简单地计算：

**命题**：符号同上，
$$
\hat{f}(a) = \frac{1}{n}\langle f, \chi_a \rangle
$$

**证明**：$f = \sum_{b \in G} \hat{f}(b)\chi_b$，对 $\chi_a$ 作内积有
$$
\langle f, \chi_a \rangle = \sum_b \hat{f}(b) \langle \chi_b, \chi_a \rangle = n\hat{f}(a)
$$

（可能）不太平凡的是，通过内积也能反过来从 Fourier 系数确定原函数，这表明函数 $f$ 和它的 Fourier 变换 $\hat{f}$ 是地位等价的：

**命题**：符号同上，
$$
\langle \hat{f}, \chi_{-x} \rangle = f(x)
$$

**证明**：
$$
\begin{align*}
\langle \hat{f}, \chi_{-x} \rangle &= \sum_b \hat{f}(b) \chi_{-x}(b)^{-1} \\
&= \sum_b \frac{1}{n} \langle \hat{f}, \chi_{b} \rangle \chi_{-x}(b)^{-1} \\
&= \frac{1}{n} \sum_b \sum_y f(y) \chi_b(y)^{-1} \chi_{-x}(b)^{-1} \\
&= \frac{1}{n} \sum_b \sum_y f(y) \chi_b(y)^{-1} \chi_{b}(-x)^{-1} \\
&= \frac{1}{n} \sum_b \sum_y f(y) \chi_b(y - x)^{-1} \\
&= \frac{1}{n} \sum_y f(y) \sum_b \chi_{x-y}(b) \\
&= \frac{1}{n} nf(x) \\
&= f(x)
\end{align*}
$$

下面是 Fourier 变换的一些简单性质：

**命题**：
- $||f||_2^2 = n ||\hat{f}||_2^2$；
- $\widehat{f+g} = \hat{f} + \hat{g}$；
- $\widehat{cf} = c\hat{f}$；
- $\widehat{fg} = \hat{f} * \hat{g}$；
- $\widehat{f*g} = n \hat{f} \hat{g}$．

**证明**：先证模长．
$$
\begin{align*}
||f||^2 &= \langle f, f \rangle \\
&= \langle \sum_a \hat{f}(a)\chi_a, \sum_b \hat{f}(b) \chi_b \rangle \\
&= \sum_a \sum_b \hat{f}(a) \overline{\hat{f}(b)} \langle \chi_a, \chi_b \rangle \\
&= \sum_a \left| \hat{f}(a) \right|^2 n \\
&= n ||\hat{f}||^2
\end{align*}
$$

Fourier 变换显然是线性变换．下面只证明乘法和卷积：
$$
\begin{align*}
fg &= \sum_a \hat{f}(a)\chi_a \sum_b \hat{g}(b)\chi_b \\
&= \sum_a \sum_b \hat{f}(a) \hat{g}(b) \chi_{a+b} \\
&= \sum_c \sum_{a+b=c} \hat{f}(a) \hat{g}(b) \chi_c
\end{align*}
$$
$$
\begin{align*}
\widehat{f*g}(a) &= \frac{1}{n}\langle f*g, \chi_a \rangle \\
&= \frac{1}{n} \sum_x (f*g)(x) \overline{\chi_a(x)} \\
&= \frac{1}{n} \sum_x \sum_{y+z=x} f(y) g(z) \overline{\chi_a(x)} \\
&= \frac{1}{n} \sum_y \sum_z f(y)g(z) \overline{\chi_a(y+z)} \\
&= \frac{1}{n} \sum_y f(y) \overline{\chi_a(y)} \sum_z g(z) \overline{\chi_a(z)} \\
&= \frac{1}{n} \cdot n\hat{f}(a) \cdot n\hat{g}(a) \\
&= n\hat{f}(a)\hat{g}(a)
\end{align*}
$$

另外，Fourier 系数也可以以另外一种方式被定义：$\hat{f}(a) = \langle f, \chi_a \rangle$，省去那个系数 $\frac{1}{n}$．在这种定义下，
- $f = \frac{1}{n} \sum_a \hat{f}(a) \chi_a$；
- $||\hat{f}||^2 = n||f||^2$；
- $n\widehat{fg} = \hat{f} * \hat{g}$；
- $\widehat{f*g} = \hat{f}\hat{g}$．

Fourier 变换简单的应用就是把算卷积转化为算乘法，通常来说乘法比卷积容易计算．考虑一个简单的例子，设随机变量 $X \sim P_X, Y \sim P_Y$，$X$ 和 $Y$ 相互独立，那么
$$
P_{X+Y}(z) = \sum_{x+y=z} P_X(x) P_Y(y) = (P_X * P_Y)(z)
$$
进而，设 $X_1, \dots, X_t \sim P_X$ 独立同分布，那么
$$
P_{X_1 + \dots + X_t} = P_X * \dots * P_X
$$
因而
$$
\widehat{P_{X_1 + \dots + X_t}} = n^{t-1} (\widehat{P_X})^t
$$

## 应用：布尔函数分析

考虑 $\{0, 1\}^n \to \{0, 1\}$ 的布尔函数，我们有两种方式去表示：一种是 $f: \mathbb{Z}_2^n \to \{0, 1\}$，以 $0$ 表示 TRUE，$1$ 表示 FALSE；有时也常进一步把 $\{0, 1\}$ 映射到 $\{(-1)^0, (-1)^1\}$，也就是 $f': \mathbb{Z}_2^n \to \{-1, 1\}$．它们之间的基本关系是 $f' = (-1)^f = 1 - 2f$，因此 Fourier 系数满足
$$
\widehat{f'}(a) = \begin{cases}
-2\hat{f}(a), & a \neq 0 \\
1 - 2\hat{f}(a), & a = 0
\end{cases}
$$
此时 $\chi_a(x) = \prod_j (-1)^{a_j x_j} = (-1)^{\langle a, x \rangle}$．

布尔函数分析 (Boolean Function Analysis) 中涉及这样的事情：考虑集合 $A \subseteq [n]$，函数 $\alpha \to \{0, 1\}$，对于每个 01 向量 $x$，把 $A$ 所指定的那些位替换成 $\alpha$ 映射的值，然后再输入到 $f$ 中：

**定义**：设 $A \subseteq [n]$，$\alpha \to \{0, 1\}$，定义
$$
x_{A \leftarrow \alpha}[i] = \begin{cases}
x[i], & i \notin A \\
\alpha(i), & i \in A
\end{cases}
$$
$$
f_{A \leftarrow \alpha}(x) = f(x_{A \leftarrow \alpha})
$$

这样做的背景是 $\mathrm{AC}_0$ 电路．电路由若干逻辑门（与门，或门和非门）相互连接，有 $n$ 个输入和一个最终的输出．$\mathrm{AC}_0$ 是一个电路的集合，“A” 的意思是允许与门和或门接受任意多的信号输入，“0” 则意味着电路的深度为 $O(1)$．（一般地，下标 $t$ 则表示电路的深度为 $O(\log^t n)$；“A” 可以替换成反义词 “N”，表示与门和或门只能接受两个信号输入．）研究 $\mathrm{AC}_0$ 电路时经常采用随机选取 $A$ 和 $\alpha$ 的方式降低电路的复杂度，具体不作展开．

先考虑简单情形：固定 $A = \{1\}$，等概率地随机选取 $\alpha \in \{0, 1\}$，此时
$$
f_{A \leftarrow \alpha}(x) = f(\alpha x_2 \dots x_n) = f(\alpha x_\mathrm{tail})
$$
在这种记号下，
$$
\begin{align*}
\widehat{f_{A \leftarrow \alpha}}(a) &= \frac{1}{2^n} \langle f_{A \leftarrow \alpha}, \chi_a \rangle \\
&= \frac{1}{2^n} \sum_x f_{A \leftarrow \alpha}(x) \overline{\chi_a(x)} \\
&= \frac{1}{2^n} \sum_{x_1} \sum_{x_\mathrm{tail}} f(\alpha x_\mathrm{tail}) \overline{\chi_{a_1}(x_1)} \overline{\chi_{a_\mathrm{tail}}(x_\mathrm{tail})}
\end{align*}
$$
如果 $a_1 = 1$，那么由于 $\sum_{x_1} \overline{\chi_{a_1}(x_1)} = 0$，上式恒为 $0$，因此下面只考虑 $a_1 = 0$，此时 $\sum_{x_1} \overline{\chi_{a_1}(x_1)} = 2$．等式两侧对 $\alpha$ 取期望，有
$$
\begin{align*}
\mathbb{E}_\alpha\left[\widehat{f_{A \leftarrow \alpha}}(a)\right] &= \frac{1}{2^n} \cdot 2 \cdot \sum_{x_\mathrm{tail}} \mathbb{E}_\alpha\left[f(\alpha x_\mathrm{tail})\right] \overline{\chi_{a_\mathrm{tail}}(x_\mathrm{tail})} \\
&= \frac{1}{2^n} \cdot 2 \cdot \sum_{x_\mathrm{tail}} \frac{1}{2} \sum_\alpha f(\alpha x_\mathrm{tail}) \overline{\chi_{a_\mathrm{tail}}(x_\mathrm{tail})} \\
&= \frac{1}{2^n} \cdot \sum_\alpha \sum_{x_\mathrm{tail}} f(\alpha x_\mathrm{tail}) \overline{\chi_{a_1}(\alpha)} \overline{\chi_{a_\mathrm{tail}}(x_\mathrm{tail})} \\
&= \frac{1}{2^n} \sum_x f(x) \overline{\chi_a(x)} \\
&= \hat{f}(a)
\end{align*}
$$
总而言之，
$$
\mathbb{E}_\alpha\left[\widehat{f_{A \leftarrow \alpha}}(a)\right] = \begin{cases}
0, & a_1 = 1 \\
\hat{f}(a), & a_1 = 0
\end{cases}
$$
推而广之，如果 $A$ 不是单点集，那么
$$
\mathbb{E}_\alpha\left[\widehat{f_{A \leftarrow \alpha}}(a)\right] = \begin{cases}
0, & \exists i \in A, a_i = 1 \\
\hat{f}(a), & \text{otherwise}
\end{cases}
$$
最后，回到一开始说的，随机选取 $A$，也随机选取 $\alpha$，那么

**定理**：沿用上面的记号，记 $\Pr[i \in A] = 1 - p$，那么
$$
\mathbb{E}_{\alpha, A}\left[\widehat{f_{A \leftarrow \alpha}}(a)\right] = p^{||a||_1} \hat{f}(a)
$$
这里 $||a||_1$ 是 $a$ 的 L1 范数，也就是其中 $1$ 的数量．