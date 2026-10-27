---
title: 'TCS 入门 | 可计算性理论 III: 递归定理，图灵归约'
publishDate: 2026-10-27 17:09:49
description: 'TCS 个人笔记摘录'
tags:
  - 'TCS'
  - '数学'
heroImage: { src: '../tcs-note4/yuuki2.jpg', color: '#B4C6DA' }
language: '中文'
---

理论计算机科学基础（2026Fall）个人笔记。

## 前言

我们继续研究不可判定性的证明方法，之前已经学习了对角化和 $m$-归约。本次首先介绍**递归定理**及其等价描述；然后介绍**图灵归约**，也就是 $T$-归约；最后讨论一类和随机串有关的新的计算问题。

## 递归定理

我们有四种方式去表述：
1. **自我复制**。存在特殊的图灵机，名为 $\text{SELF}$，无论接受什么输入，永远输出自身的编码 $\langle \text{SELF} \rangle$。
2. **递归定理**。对于任何可计算函数 $t: \Sigma^* \times \Sigma^* \to \Sigma^*$，都存在图灵机 $M$，使得 $M$ 计算一元函数 $t(x, \langle M \rangle)$（如果 $t$ 无定义，那么 $M$ 不停机）。
3. **自我引用**。图灵机的定义中可以引用自身的编码。
4. **不动点定理**。对于任何全可计算函数 $f$，都存在图灵机 $M$ 使得 $\langle M\rangle$ 和 $f(\langle M \rangle)$ 等价，即这两个编码对应的图灵机计算相同的语言/函数。

先来看自我复制。用 $P_w$ 表示对于任何输入 $x$ 永远输出 $w$ 的图灵机，这样的图灵机总是存在的：
$$
\begin{align*}
\text{TM } P_w = &\text{ “对于输入 } x, \\
&\ \ \text{ 1. 在纸带上删除 }x; \\
&\ \ \text{ 2. 在纸带上写下 }w; \\
&\ \ \text{ 3. 停机。”} \\
\end{align*}
$$
定义 $q: \Sigma^* \to \Sigma^*$ 使得 $q(w) = \langle P_w\rangle$，这个函数是可计算的：$P_w$ 的编码模板是固定的，只需要让图灵机 $Q$ 拿到输入 $w$ 之后填入 $P_w$ 的模板中，写在纸带上，这样 $Q$ 就计算 $q$。我们断言 $\text{SELF}$ 可以构造为两个图灵机的合成 $AB$（先执行 $A$，再在 $A$ 的输出上执行 $B$），其中
$$
\begin{align*}
A &= P_{\langle B\rangle} \\
B &= \text{“对于输入 } \langle M \rangle \text{，输出 } \langle P_{\langle M\rangle}M \rangle \text{”}
\end{align*}
$$
首先 $B$ 是良定义的，已经说明过 $q: \langle M\rangle \mapsto \langle P_{\langle M\rangle}\rangle$ 是可计算的，$B$ 算出 $P_{\langle M\rangle}$ 的代码来和 $M$ 自己的代码拼起来即可。这样，$\text{SELF}$ 无论输入如何，先运行 $A$ 得到 $\langle B\rangle$，再运行 $B$ 得到 $\langle P_{\langle B\rangle}B \rangle$，后者正是 $\langle AB \rangle$。

基于自我复制，可以用类似的手段证明递归定理。所需的图灵机 $M$ 可以构造为 $ABT$，其中 $T$ 是计算 $t$ 的图灵机（图灵机 $T$ 接受两个输入，一个是 $M$ 的输入，另一个是 $AB$ 的计算结果），$B$ 和前文相同，$A$ 为 $P_{\langle BT\rangle}$。对于任何输入 $x$，$M$ 先运行 $A$ 得到 $\langle BT \rangle$（保留输入 $x$），然后运行 $B$ 得到 $\langle P_{\langle BT\rangle} BT\rangle = \langle ABT \rangle = \langle M \rangle$，最后运行 $T$ 得到 $t(x, \langle M\rangle)$，正是我们想要的。

自我引用是递归定理的推论。假设我们想要定义图灵机 $M$，但是它的定义中引用了 $\langle M\rangle$。我们先把定义中引用 $\langle M \rangle$ 的地方换成 $y$ 得到 $T$，这样 $T$ 有两个输入，计算二元函数 $T(x,y)$。根据递归定理，存在图灵机 $M$ 使得 $M$ 计算 $T(x,\langle M\rangle)$，这正是我们一开始想要定义的。

自我引用很容易推导回自我复制，这是显然的。

图灵机自引用可以解决一部分计算问题的不可判定性：得到自己的编码，由此导出某些矛盾。例如，**图灵机的极小性问题**
$$
\text{MIN}_{\text{TM}} = \{\langle M\rangle \mid M \text{ 是极小图灵机}\}
$$
其中极小图灵机指的是，对于任意图灵机 $M'$，如果 $M'$ 的编码长度 $|\langle M'\rangle| < |\langle M\rangle|$，那么 $M'$ 与 $M$ 功能不等价（识别不同的语言/计算不同的函数）。我们断言它不是图灵可识别的。假定它可识别，那么它可枚举，设 $E$ 是它的枚举器。构造图灵机 $C$：对于输入 $x$，先运行 $E$ 直到得到满足 $|\langle D\rangle| > |\langle C\rangle|$ 的图灵机 $D$（极小图灵机无穷多，这样的 $D$ 总是存在），然后模拟 $D$ 在 $x$ 上的运行。由于 $D$ 是 $E$ 枚举出来的，故 $D$ 是极小图灵机，但是 $D$ 和 $C$ 显然功能等价，而 $|\langle D\rangle| > |\langle C\rangle|$ 说明 $D$ 并非极小，矛盾。

回到主线，来证明递归定理和不动点定理的等价性。

<div class="list-spaced">

- 不动点定理 $\to$ 递归定理。给定可计算函数 $t: \Sigma^* \times \Sigma^* \to \Sigma^*$，设图灵机 $T$ 计算 $t$，进一步设图灵机 $T_y$ 计算 $t(\cdot, y)$，定义函数 $f(y) = \langle T_y\rangle$。依不动点定理，存在图灵机 $M$ 使得 $M$ 和 $f(\langle M\rangle) = \langle T_{\langle M\rangle}\rangle$ 对应的图灵机，即 $T_{\langle M\rangle}$ 等价。注意 $T_{\langle M\rangle}$ 计算 $t(\cdot, \langle M\rangle)$，这说明 $M$ 正是递归定理所要求的图灵机。
- 递归定理 $\to$ 不动点定理。给定任意可计算函数 $f$，定义函数 $t(x, y) = (f(y))(x)$，即 $t$ 先计算 $f(y)$，然后把 $f(y)$ 解释成图灵机，输出该图灵机在 $x$ 上的运行结果，因而 $t$ 是可计算的。依递归定理，存在图灵机 $M$ 使得 $M$ 计算 $t(\cdot, \langle M\rangle) = f(\langle M\rangle)$，这正是不动点定理所要求的。

</div>

## 图灵归约

回想之前学过的映射归约，映射归约有很多好处（例如具有传递性，且对可判定性、可识别性等常见性质封闭），但是有一种缺陷：一个语言有时无法规约到自己的补语言，而 $A$ 和 $\overline{A}$ 理应很容易互相联系（对答案取反）。为此，下面介绍另一种归约。

首先，我们用 $M^B$ 表示一种特殊的图灵机，它带有一个外部信息源（称为谕示或神谕），能够回答 $x \in B$ 这样的问题；这样的图灵机称为**谕示图灵机**或**神谕图灵机**（**OTM**，**O**racle **T**uring **M**achine）。形式化地，它相当于在普通图灵机 $M$ 的基础上增加三个特殊状态 $q_\text{query}$，$q_\text{yes}$ 和 $q_\text{no}$，并增加一条名为查询带的纸带。开始查询带为空，工作过程中，图灵机可以向查询带上写入字符串，写完之后进入查询状态 $q_\text{query}$；一步之后，图灵机自动进入 $q_\text{yes}$ 或 $q_\text{no}$ 之一，表示查询带上的字符串 $\in B$ 或 $\notin B$。

对于两个语言 $A$，$B$，如果存在处处停机的谕示图灵机 $M^B$ 使得 $A = L\left(M^B\right)$，就称 $A$ 可以**图灵归约**到 $B$，记作 $A \le_T B$。加谕示也被称为相对化，因而图灵归约也可以称作 **$A$ 相对于 $B$ 可判定**。

图灵归约也是传递的。如果 $A \le_T B$，$B \le_T C$，那么 $A \le_T C$。

图灵归约也对可判定性封闭。如果 $A \le_T B$，$B$ 可判定，那么 $A$ 也可判定。既然 $B$ 已经可判定，我们可以抛弃 $M^B$ 中的谕示，直接判定 $x \in B$ 或 $x \notin B$ 即可，这就将谕示图灵机改造成了普通图灵机。然而图灵归约的封闭性不如映射归约好，它对可识别性/补图灵可识别性并不封闭。

一个语言很容易图灵归约到自己的补，即 $A \le_T \overline{A}$，只需要反转谕示。

图灵归约可以更好地刻画 Kleene 谱系。在 Kleene 谱系中，增加 $A_\text{TM}$ 作为谕示会使得层级刚好扩大一层：
$$
\begin{align*}
&\Sigma_1^{A_\text{TM}} = \Sigma_2 \\
&\Pi_1^{A_\text{TM}} = \Pi_2 \\
&\Delta_1^{A_\text{TM}} = \Delta_2
\end{align*}
$$
（这里，上标 $A_\text{TM}$ 表示将原语言类定义中的图灵机 $M$ 换成谕示图灵机 $M^{A_\text{TM}}$，三者分别称作“相对于 $A_\text{TM}$ 可识别/补图灵可识别/可判定”）。下面以第一个的证明为例：

<div class="list-spaced">

- 先说明 $\Sigma_2 \subseteq \Sigma_1^{A_\text{TM}}$。依定义，$\forall L \in \Sigma_2$，存在可计算的谓词 $R$ 使得 $L = \{x \mid \exists y_1, \forall y_2, R(x, y_1, y_2)\}$。定义 $L_1 = \{(x, y_1) \mid \forall y_2, R(x, y_1, y_2)\}$，则 $L_1 \in \Pi_1$，进而 $\overline{L_1} \in \Sigma_1$，$\overline{L_1} \le_m A_\text{TM}$。设函数 $f$ 将 $\overline{L_1}$ 归约到 $A_\text{TM}$，构造图灵机
    $$
    \begin{align*}
    M^{A_\text{TM}} = &\text{ “对于输入 }x\text{，枚举 }y_1\text{，} \\
    &\ \ \text{ 查询 }f(x, y_1)\text{ 是否属于 }A_\text{TM}\text{，} \\
    &\ \ \text{ 如果答案是否，则接受。”}
    \end{align*}
    $$
    如果 $x \in L$，那么存在 $y_1$ 使得 $(x, y_1) \in L_1$，等价地 $(x, y_1) \notin \overline{L_1} \Leftrightarrow f(x, y_1) \notin A_\text{TM}$，$M^{A_\text{TM}}$ 停机接受；反之不停机。因此 $L = L\left(M^{A_\text{TM}}\right)$，$L \in \Sigma_1^{A_\text{TM}}$。
- 再说明 $\Sigma_1^{A_\text{TM}} \subseteq \Sigma_2$。$\forall L \in \Sigma_1^{A_\text{TM}}$，存在谕示图灵机 $M^{A_\text{TM}}$ 使得 $L = L\left(M^{A_\text{TM}}\right)$。给定输入 $x$，$M$ 在运行中会向谕示查询若干个字符串，包括回答 yes 的串 $y_1, \dots, y_p \in A_\text{TM}$ 和回答 no 的串 $z_1, \dots, z_q \notin A_\text{TM}$。注意 $A_\text{TM} \in \Sigma_1$，因而 $A_\text{TM}$ 可以写作 $\{x \mid \exists y, R_0(x, y)\}$，所以 $y_i \in A_\text{TM}$ 就可以写作 $\exists u_i, R_0(y_i, u_i)$，$z_i \notin A_\text{TM}$ 就可以写作 $\forall v_i, \lnot R_0(z_i, v_i)$。最终，（可能写的不是特别严谨）
  $$
  \begin{align*}
  L = \Bigl\{x \mid \ &\exists \text{ 计算历史 } c \text{ 和 } (u_1, \dots, u_p), \\
  &\forall (v_1, \dots, v_q), \\
  &c \text{ 是 } M \text{ 在 } x \text{ 上的接受计算历史，}\\
  &y_1, \dots, y_p \text{ 是 } c \text{ 中全部回答 yes 的查询，}\\
  &z_1, \dots, z_q \text{ 是 } c \text{ 中全部回答 no 的查询，并且} \\
  &\bigwedge_{i=1}^{p} R_0(y_i,u_i)\land\bigwedge_{j=1}^{q} \neg R_0(z_j,v_j)\Bigr\}
  \end{align*}
  $$
  故 $L \in \Sigma_2$。

</div>

留意到 $\Sigma_1$ 中的问题的能力不超过 $A_\text{TM}$，我们可以写 $\Sigma_2 = \Sigma_1^{\Sigma_1}$，类似地 $\Sigma_3 = \Sigma_1^{\Sigma_2}$，以此类推。因此，Kleene 谱系也称为**算术谱系**。算术谱系可以与之前介绍的乔姆斯基谱系衔接，乔姆斯基谱系最顶层的 $0$ 型语言是算术谱系的开始。在算术谱系之上，还有解析谱系等更高层的内容，留待进一步学习。

## 随机串

最后来讨论一类新的计算问题。说到“随机串”，首先的问题是什么是“随机”，串 00000000 和 01001101 虽然都可以由抛硬币的方式生成，但是前者看起来不如后者随机，概率视角下的随机无法解释这个问题。讨论这个问题的一种视角是**压缩**：长度为 $n$ 的串总共有 $2^n$ 个，而长度小于 $n$ 的所有串加起来只有 $2^n - 1$ 个，这就意味着必然有串无法压缩，不可压缩的串就是随机串。

严格地，如果存在图灵机 $M$ 和输入串 $y$ 使得 $M$ 在 $y$ 上停机输出 $x$（简记作 $M(y) = x$），那么就说 $\langle M, y\rangle$ 描述 $x$。定义一个串的描述的极小长度为该串的 **Kolmogorov 复杂度**或**描述复杂度**，即
$$
K(x) = \min_{M(y) = x} |\langle M, y\rangle|
$$

如果 $K(x) \ge |x|$，就说串 $x$ **不可压缩**，或者说 $x$ 是**随机串**。所有随机串构成集合 $\text{RANDOM} = \{x \mid K(x) \ge |x|\}$。这里直接将随机性等同于不可压缩性，实际上可以严格证明，从不可压缩性出发可以推导出很多直观上随机串应该具有的数学性质。基于抽屉原理，随机串总是存在的。注意描述复杂度具有所谓的相对性原理，由于图灵机可以互相模拟，定义中使用什么图灵机实际上不太重要。

$K(x)$ 是相对于 $A_\text{TM}$ 可计算的，只需要枚举 $M$ 和 $y$ 即可；但是如果没有 $A_\text{TM}$ 的谕示，图灵机不知道 $M(y)$ 是否停机。下面将证明 $K(x)$ 是不可计算的；为了证明这一点，只需要证明 RANDOM 是不可判定的。实际上，这个集合不仅不可判定，还不可识别；不仅不可识别，还不含有无穷的可识别子集。这等价于说，任何图灵机都不能枚举出无穷多个随机串。假定图灵机 $E$ 能枚举出无穷多随机串 $x_1, x_2, \dots$，定义 $y_n$ 为“ $E$ 枚举出的第一个长度超过 $n$ ”的串，那么
$$
K(y_n) \le |\langle E\rangle| +C+O(\log_2 n)
$$
其中 $C$ 是上面的那句话的长度，$log_2 n$ 是 $n$ 的编码长度。由于 $y_n$ 不可压缩，$K(y_n) \ge |y_n| > n$，故 $n < O(\log n) + C'$。令 $n \to \infty$ 得到矛盾。