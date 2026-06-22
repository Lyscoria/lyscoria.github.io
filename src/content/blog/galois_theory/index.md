---
title: '离散数学片羽 | Galois 理论'
publishDate: 2026-06-21 14:20:03
description: '古典代数学的一大顶峰。'
tags:
  - '抽象代数'
  - '数学'
heroImage: { src: './feibi3.png', color: '#B4C6DA' }
language: '中文'
---

离散数学与结构 (2025Fall) 抽象代数部分的笔记；群环域的部分被高代包含了，所以没有整理。

## 正规扩张与可分扩张

**定义** 设 $F$ 是域，$K$ 是 $F$ 的扩域．若对任意不可约的 $f \in F[X]$，$f$ 在 $K$ 中有根 $\Rightarrow$ $f$ 在 $K$ 中分裂，则称 $K$ 是 $F$ 的**正规扩张 (Normal Extension)**．

比如说，$\mathbb{Q}[2^{1/3}]$ 并不是 $\mathbb{Q}$ 的正规扩张，因为多项式 $X^3 - 2$ 在其中并不分裂．回忆分裂域的概念：对某个多项式 $f \in F[X]$，如果 $K = F(\alpha_1, \dots, \alpha_n)$ 并且 $f = (x - \alpha_1) \dots (x - \alpha_n)$，则称 $K$ 是 $f$ 的分裂域．我们可以证明，如果 $K$ 是 $F$ 的有限扩张，那么这两个概念是等价的．在此之前先介绍极小多项式的概念：

**定义** 设 $F$ 是域，$K$ 是其扩域．对代数数 $\alpha \in K$，所有满足 $f(\alpha) = 0$ 的多项式 $f \in F[X]$ 中次数最低的首一多项式称为 $\alpha$ 的极小多项式．

将 $\alpha$ 的极小多项式记作 $m$．我们知道 $F[X]$ 是一个 PID，容易验证所有满足 $f(\alpha) = 0$ 的多项式 $f \in F[X]$ 构成一个理想，这个理想实际上就是 $(m)$．由此不难得出，极小多项式是不可约多项式，且整除所有以 $\alpha$ 为根的多项式．特别地，$[F(\alpha) : F] = \deg m$，不难检验 $1, \alpha, \dots, \alpha^{\deg m - 1}$ 是 $F(\alpha)$ 作为 $F$-向量空间的一组基．

**定理** 设 $F$ 是域，$K$ 是其扩域，扩张次数 $[K : F] < +\infty$．则 $K$ 是 $F$ 的正规扩张等价于 $K$ 是某个 $f \in F[X]$ 的分裂域．

**证明** 假设 $K = F(u_1, \dots, u_r)$ 是 $F$ 的有限次正规扩张，取 $u_1, \dots, u_r$ 各自的极小多项式 $f_1, \dots, f_r \in K[X]$，令 $f = f_1f_2 \dots f_r$．根据正规性，$K$ 包含了 $f$ 的全部根，进而包含了 $f$ 的分裂域；同时，由于 $u_1$ 均为 $f$ 的根，$K$ 包含于 $f$ 的分裂域中：由此即知 $K$ 是 $f$ 的分裂域．

另一方面，如果 $K = F(u_1, \dots, u_r)$ 是多项式 $f = (x - u_1) \dots (x - u_r) \in F[X]$ 的分裂域，我们只需要证明 $\forall \alpha \in K, \exists g \in F[X]$ 使得 $g(\alpha) = 0$ 且 $g$ 在 $K$ 上分裂．（为什么？我们需要证明的是对任意不可约多项式 $f \in F[X]$，如果 $\exists \alpha \in K$ 使得 $f(\alpha) = 0$，那么 $f$ 在 $K$ 上分裂．假如说我们构造出了前面的多项式 $g$，那么 $\deg \gcd(f, g) > 1$，依 $f$ 的不可约性，只能有 $\gcd(f, g) = f \Rightarrow f | g \Rightarrow f$ 在 $K$ 上也分裂．）为此，设
$$
\alpha = \sum a_{i_1 \dots i_r} u_1^{i_1} \dots u_r^{i_r}
$$
$\forall \pi \in S_r$，定义
$$
\alpha_\pi = \sum a_{\pi(i_1) \dots \pi(i_r)} u_1^{i_1} \dots u_r^{i_r}
$$
相当于把原系数打乱（打乱指数也是相同的效果），令
$$
g(x) = \prod_{\pi \in S_r} (X - \alpha_\pi)
$$
那么 $\alpha$ 是 $g$ 的根，并且 $g$ 的系数是关于 $u_1, \dots, u_r$ 的对称多项式，它们总能被初等对称多项式，也就是 $f$ 的系数表出，故 $g \in F[X]$，证毕．

相比于正规扩张，可分扩张的概念自然得多：

**定义** 设 $F$ 是域，$K$ 是其扩域．如果 $\forall u \in K$，存在无重根的多项式 $f \in F[X]$ 使 $f(u) = 0$，则称 $K$ 是 $F$ 的**可分扩张 (Separable Extension)**．

想要举出一个不可分的域扩张不太容易，这是因为：

**定理** 设 $F$ 是域，$K$ 是其代数扩张．如果 $F$ 特征为 $0$ 或 $F$ 为有限域，那么 $K$ 是可分扩张．

**证明** 取 $\alpha \in K$，将其极小多项式记作 $f$，假设 $f$ 有重根，则 $\gcd(f, f')$ 非常数．由于 $f$ 不可约，只能有 $f = \gcd(f, f') \Rightarrow f | f'$，由此推出 $f' = 0$．

如果 $F$ 是特征 $0$ 的域，那么 $f' = 0$ 意味着 $f$ 是常数多项式，矛盾．

如果 $F$ 是有限域，设它的大小为 $p^n$，$p$ 是素数，那么 $f' = 0$ 意味着 $f = \sum_k a_k x^{pk}$．由于 $x \mapsto x^p$ 是 $F$ 的自同构，存在 $b_k$ 使得 $b_k^p = a_k$，令 $g(x) = \sum_k b_k x^k$，就有 $g^p = f$ 成立，与 $f$ 不可约矛盾．

从而 $\alpha$ 的极小多项式没有重根，这就说明了 $K$ 是 $F$ 的可分扩张．

关于可分扩张有一个重要的定理：

**定理** 若 $K$ 是 $F$ 的有限次可分扩张，那么 $K$ 是 $F$ 的单扩张．换言之，存在 $\alpha \in K$ 使得 $K = F(\alpha)$．

**证明** 我们只需要说明，对于任何 $\alpha, \beta$，$\exists c$ 使得 $F(\alpha, \beta) = F(\alpha + c\beta)$．

设 $\alpha$ 和 $\beta$ 的极小多项式分别为 $P(x) = (x - \alpha_1) \dots (x - \alpha_s)$，$Q(x) = (x - \beta_1) \dots (x - \beta_t)$，其中 $\alpha_1 = \alpha$，$\beta_1 = \beta$．容易说明极小多项式是无重根的：根据可分性，存在一个无重根多项式以 $\alpha$ 为根，而这个多项式被极小多项式整除．令 $\gamma = \alpha + c\beta$，$c$ 待定，考虑多项式 $P(\gamma - cx)$．如果 $\beta_i$ 是 $P(\gamma - cx)$ 的根，那么 $\gamma - c\beta_i = \alpha_j$，即 $\alpha + c(\beta - \beta_i) = \alpha_j$．因此，只要取 $c \neq (\alpha_j - \alpha)/(\beta - \beta_i), \forall i, j$，那么除了 $\beta$ 之外，所有 $\beta_i$ 均不是 $P(\gamma - cx)$ 的根，从而 $\gcd(Q(x), P(\gamma - cx)) = x - \beta$，也就说明了 $\beta \in F(\gamma)$，进而 $\alpha = \gamma - c\beta \in F(\gamma)$．

## 有限 Galois 基本定理

**定义** 若 $K$ 是 $F$ 的有限次可分正规扩张，则称 $K$ 是 $F$ 的 **Galois 扩张**．此时定义 $\mathrm{Gal}(K/F)$ 为所有保持 $F$ 不变的 $K$-自同构，称为 **Galois 群**．

**定理** 条件同上，则 $|\mathrm{Gal}(K/F)| = [K : F]$．

**证明** 我们已经证明 $K$ 是 $F$ 的单扩张 $F(\alpha)$，设其极小多项式 $m$ 的根为 $\alpha_1, \dots, \alpha_r$，$\alpha_i$ 互不相同．任取 $\phi \in \mathrm{Gal}(K/F)$，$\phi$ 保持 $F$ 不变，进而保持极小多项式系数不变，从而保持集合 $\{\alpha_1, \dots, \alpha_r\}$ 不变．由此看出，Galois 群中的自同构与 $\alpha \mapsto \alpha_i$ 一一对应，故 $|\mathrm{Gal}(K/F)| = r = \deg m = [F(\alpha) : F] = [K : F]$．

**定理** 条件同上．对于中间域 $F \subseteq E \subseteq K$，$K$ 也是 $E$ 的 Galois 扩张．

**证明** 可分性是明显的，而正规性与分裂域等价，从而也是明显的．

**定理** 条件同上，简记 $G = \mathrm{Gal}(K/F)$．那么有一一对应
$$
\begin{aligned}
    \{H | H \leq G\} &\xleftrightarrow{1 : 1} \{\textup{Field }E | F \subseteq E \subseteq K\} \\
    H &\mapsto K^H := \{x \in K | \forall h \in H, h(x) = x\} \\
    \mathrm{Gal}(K/E) &\leftarrow E
\end{aligned}
$$
同时，$H$ 是 $G$ 的正规子群等价于 $E$ 是 $F$ 的正规扩张，且
$$
\mathrm{Gal}(E/F) \cong \mathrm{Gal}(K/F) / \mathrm{Gal}(K/E)
$$

**证明** 先验证左$\rightarrow$右$\rightarrow$左的合成，这等价于说明 $\mathrm{Gal}(K/K^H) = H$．按照定义，显然 $H \leq \mathrm{Gal}(K/K^H)$（即“$H$ 中的 $K$-自同构保持 $K$ 在 $H$ 作用下的不动点不动”）．记 $E = K^H$，那么 $\exists \alpha \in K$ 使得 $K = E(\alpha)$．令 $f = \prod_{h \in H} (x - h(\alpha))$，那么 $H$ 保持 $f$ 的系数不变，因而 $f \in E[X]$，故 $|H| = \deg f \ge \deg m_\alpha = [K : E] = |\mathrm{Gal}(K/K^H)|$．故 $H = \mathrm{Gal}(K/K^H)$．
    
再验证右$\rightarrow$左$\rightarrow$右的合成，这等价于证明 $K^{\mathrm{Gal}(K/E)} = E$．按照定义，显然 $E \leq K^{\mathrm{Gal}(K/E)}$ （即“保持 $E$ 不变的 $K$-自同构保持 $E$ 不变”）．而 $[K:E] = |\mathrm{Gal}(K/E)| = |\mathrm{Gal}(K/K^{\mathrm{Gal}(K/E)})|$ (这一步是利用已经证明的左$\rightarrow$右$\rightarrow$左的合成为恒等) $= [K : K^{\mathrm{Gal}(K/E)}]$，于是 $[E : K^{\mathrm{Gal}(K/E)}] = 1$，$E = K^{\mathrm{Gal}(K/E)}$．

此时，$H \trianglelefteq G \Leftrightarrow \forall g \in G = \mathrm{Gal}(K/F), \forall h \in H = \mathrm{Gal}(K/E), g^{-1}hg \in H$．也就是说，$\forall g \in \mathrm{Gal}(K/F), \forall h \in \mathrm{Gal}(K/E), \forall \alpha \in E, g^{-1}hg(\alpha) = \alpha \Leftrightarrow hg(\alpha) = g(\alpha) \Leftrightarrow g(\alpha) \in E$．问题归结于以下的命题：$E$ 是 $F$ 的正规扩张等价于 $\forall g \in G$，$g(E) = E$．

假设 $E$ 是 $F$ 的正规扩张，取 $\alpha \in E$，极小多项式记作 $m$，那么 $m$ 在 $E$ 中分裂．$m(g(\alpha)) = g(m(\alpha)) = 0$，$g(\alpha)$ 也是 $m$ 的根，于是 $g(\alpha) \in E$，这样就有 $g(E) \subseteq E$，以 $g^{-1}$ 代 $g$ 即知反向的包含关系，于是等式成立．

假设 $\forall g \in G, g(E) = E$．取不可约多项式 $f \in F[X]$，假设它在 $E$ 中有根 $\alpha$．对 $K$ 中 $f$ 的任意根 $\beta$，取 $K$-自同构 $g(\alpha) = \beta$（$\alpha$ 与 $\beta$ 极小多项式相同，因此 $\alpha \mapsto \beta$ 构成 $F(\alpha) \rightarrow F(\beta)$ 的同构，将其延拓到整个 $K$ 即可．），由 $g(E) = E$ 可知 $\beta \in E$，说明 $f$ 所有根属于 $E$，故 $E$ 是 $F$ 的正规扩张．

最后，令 $\phi: \mathrm{Gal}(K/F) \rightarrow \mathrm{Gal}(E/F)$，映 $g$ 为 $g|_E$，不难看出映射是满同态，且 $\ker \phi = \mathrm{Gal}(K/E)$．

## 多项式方程的根式解

以下假设 $F$ 特征为 $0$ 或为有限域．对一个多项式方程给出根式解的过程，可以看作一步步通过开方来对域进行扩张的过程：
$$
F = F_0 \subseteq F_1 \subseteq \dots \subseteq F_m = K 
$$
其中 $F_{i+1} = F_i(\alpha_i)$，$\alpha_i^{n_i} \in F_i$．如果最终 $K$ 包含了多项式的分裂域，那么说明方程存在根式解．然而这里每一步未必是 Galois 扩张，所以无法直接应用先前的结论．但是，令 $n = \mathrm{lcm}(n_1, \dots, n_m)$，设 $\zeta$ 为 $n$ 次单位根，事先将 $\zeta$ 添加进 $F$，再按照上面的方式进行扩张，我们发现由于 $F_{i+1}$ 包含了 $\alpha_i$ 和所有的 $n_i$ 次单位根，$F_{i+1}$ 成为了 $x^{n_i} - \alpha_i^{n_i}$ 的分裂域，从而每一步都是 Galois 扩张．因此，这一列域就通过 Galois 基本定理对应到一列 Galois 群 
$$
G_0 \trianglerighteq G_1 \trianglerighteq \dots \trianglerighteq G_m = \{e\}
$$
特别地，$\mathrm{Gal}(F_{i+1}/F_{i}) \cong \mathbb{Z}_{n_i}$，所以这里 $G_i / G_{i+1} \cong \mathbb{Z}_{n_i}$．

**定义** $G$ 是一个群．如果存在正规子群列 $G = G_0 \triangleright G_1 \triangleright \dots \triangleright G_n = \{e\}$，使得相邻商群 $G_i/G_{i+1}$ 是 Abel 群，就称 $G$ 是**可解群 (Solvable Group)**．

至此，根式求解方程的问题就转化为了判断多项式的 Galois 群是否是可解群．

考虑 $n$ 次多项式 $f \in F[X]$，其 Galois 群 $G$ 保持 $F$ 不变，进而保持根集 $A = \{\alpha_1, \dots, \alpha_n\}$ 不变． 令 $G$ 作用于 $A$，也就使 $G$ 嵌入为置换群 $S_n$ 的子群．

特别地，考虑一个有三个不等实根和一对共轭虚根的五次不可约多项式 $f \in \mathbb{Q}[X]$，比如说 $f(x) = 2x^5 - 10x + 5$．对任意两个根 $c_i$ 和 $c_j$，将 $c_i$ 映为 $c_j$ 总能给出一个保持 $\mathbb{Q}$ 不变的 $\mathbb{C}$-自同构（上一节已经解释过，这是 $\mathbb{Q}[c_i] \cong \mathbb{Q}[c_j]$ 的延拓），用群作用的语言来说，$G$ 在 $A$ 上的作用是传递的，因此轨道的大小为 $5$，而 $|\text{orbit}(x)| = [G : \text{Stab}(x)]$ 说明 $5 | |G|$，进而 $G$ 有五阶元，这个五阶元只能是某个 $5$-循环．此外，自同构 $\mathbb{C} \rightarrow \mathbb{C}: z \mapsto \bar{z}$ 说明 $G$ 中还存在一个单对换．我们知道，它们两个已经足以生成 $S_5$，故 $G = S_5$．我们知道 $S_5 \triangleright A_5$，而交错群 $A_n$ 在 $n \ge 5$ 时均为单群，这就说明了五次方程不存在一般的根式解．（上面的证明实际上适用于所有大于等于 $5$ 的素数．）