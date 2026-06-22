---
title: '离散数学片羽 | 逻辑 II: 谓词逻辑、ZFC 和 Gödel 不完备定理'
publishDate: 2026-06-21 12:37:40
description: '谓词逻辑也称一阶逻辑，ZFC 建立在一阶逻辑基础之上，而 Gödel 的定理指出了一阶逻辑的缺陷。'
tags:
  - '逻辑'
  - '数学'
heroImage: { src: './feibi4.png', color: '#B4C6DA' }
language: '中文'
---

离散数学与结构 (2025Fall) 逻辑部分的笔记 (第二部分)。

## 谓词逻辑，谓词逻辑的自然演绎

**谓词逻辑(Predicate Logic)** 也被称为**一阶逻辑**，是对命题逻辑的延伸．我们先引入相关的概念：

**定义：** 一阶逻辑所使用的字符集 **(Alphabet)** 包含以下内容:  
- **变量(Variable)** $x$ $y$ $z$ $x_1$ $y_1$ $z_1$  
- **函数(Function)** $f$ $g$ $h$ $f_1$ $f_2$ $f_3$  
- **谓词(Predicate)** $=$ $\bot$ $P$ $Q$ $P_1$  
- **连接词(Connective)** $\land$ $\vee$ $\rightarrow$ $\lnot$ $\leftrightarrow$  
- **量词(Quantifier)** $\forall$ $\exists$  
- **常量(Constant)** $c$ $c_1$ $c_2$  
- **辅助记号(Auxiliary)** ( ) ,

下面就可以定义 Term 和 Formula：

**定义：** **TERM** 是使下列性质成立的最小集合:  
1. 任意常数 $c\in \text{TERM}$，任意变量 $x\in \text{TERM}$；  
2. 若 $t_1$, $t_2\in\text{TERM}$，对任意函数 $f$，$f(t_1,t_2)\in\text{TERM}$．

**定义：** **FOR** 是使下列性质成立的最小集合：  
1. 对任意谓词 $P$，任意 $t_1$,$t_2\in\text{TERM}$，有 $P(t_1,t_2)\in\text{FOR}$；  
2. 对任意 $\psi$，$\phi\in\text{FOR}$，$\lnot\phi$，$\phi\rightarrow\psi$，$\phi\leftrightarrow\psi$，$\phi\vee\psi$，$\phi\land\psi\in\text{FOR}$；  
3. 对任意 $\phi\in\text{FOR}$，任意变量 $x$，有 $\forall x\phi\in\text{FOR}$，$\exists x\phi\in\text{FOR}$．

接着来定义适用于一阶逻辑的语义：

**定义：** 一个 **Structure** 包括下面的内容：  
- 宇宙 **(universe)** $\Omega$；  
- 函数 $f:\Omega^n\rightarrow\Omega$；  
- 谓词 $P: \Omega^n\rightarrow\{0,1\}$；  
- 常数 $c_1, \dots, c_n \in \Omega$．

宇宙可以暂且按照集合理解．我们需要规定 structure 中的内容与字符集的对应，例如 $f$ 对应到 $\bar{f}$，$c$ 对应到 $\bar{c}$，$P$ 对应到 $\bar{P}$ 等．通常用德文尖角体字母表示 structure，例如 $\mathfrak{A}$．$(\mathbb{R}; x; inv(); =, <; \{0, 1\})$ 是一个非常简单的 structure，将它的谓词、函数的变元数等写成一个元组 $<2, 1, 2, 2, 2>$， 称为它的 **Type**．类似于赋值 valuation，structure 也有相应的“求值”操作：

$$
\begin{aligned}
    [\![\bar{c}]\!]_\mathfrak{A} &= c \\
    [\![x]\!]_\mathfrak{A} &= x^\mathfrak{A} \\
    [\![\bar{f}(t_1,t_2)]\!]_\mathfrak{A} &= f([\![t_1]\!]_\mathfrak{A},[\![t_2]\!]_\mathfrak{A}) \\
    [\![\bar{P}(t_1,t_2)]\!]_\mathfrak{A} &= P([\![t_1]\!]_\mathfrak{A},[\![t_2]\!]_\mathfrak{A}) \\
    [\![\phi\rightarrow\psi]\!]_\mathfrak{A} &= \max(1-[\![\phi]\!]_\mathfrak{A},[\![\psi]\!]_\mathfrak{A}) \\
    [\![\forall x\phi(x)]\!]_\mathfrak{A} &= \min_{c\in\Omega}[\![\phi(x)]\!]_\mathfrak{A}|_{x=c} = \min_{c\in\Omega}[\![\phi(\bar{c})]\!]_\mathfrak{A} \quad (*)
\end{aligned}
$$
注：上面的定义中只给出了连接词 $\rightarrow$ 的相关定义，其他连接词同理自行给出；(*)处给出了 $\forall$ 语义的两种定义，需注意：后面的定义方式需要为所有 $c\in\Omega$ 指定对应的字符 $\bar{c}$，但是原本的字符集未必如此．以 Peano 算术为例，它的公理中只需要使用 $0$ 和 $1$ 这两个常数符号，但是在计算 $\min_{c\in\Omega}[\![\phi(\bar{c})]\!]_\mathfrak{A}$ 时可能需要使用更多的自然数符号，所以这是在扩充的字符集中进行的操作，稍后还会提到这一点．另外，这里用到了一种操作：将原字符串中的 $x$ 替换为 $\bar{c}$，但是这种操作需要一定的安全性保证，简单的直接替换未必是我们想要的效果，例如：  
$$
\begin{aligned}
“\forall x \exists(x = y)” &\rightarrow “\forall x^2 \exists y(x^2=y)” \\
&\rightarrow “\forall x \exists 0(x=0)”
\end{aligned}
$$
直观地说，我们只想让一个命题中的“自由变量”被替换，替换后的命题应当与原命题“相同”．由此出发，我们先对“自由变量”下定义：

**定义：** 给定 $t\in\text{TERM}$，以 $V(t)$ 表示其中的所有变量．对 $\phi\in\text{FOR}$，定义自由变量 **(Free Variable)** 集合 $FV(\phi)$ 如下：  
1. 如果 $\phi=P(t_1,t_2)$，则 $FV(\phi)=V(t_1)\cup V(t_2)$；  
2. $FV(\phi\square\psi)=FV(\phi)\cup FV(\psi)$，这里 $\square$ 表示连接词；  
3. $FV(Qx\phi)=FV(\phi)-\{x\}$，这里 $Q\in\{\forall,\exists\}$．

现在来规定字符串的替换（以 $\phi[t/x]$ 表示将 $x$ 替换为 $t$）:

**定义：** 按照以下规则进行字符串的替换：  
1. 若 $\phi=P(t_1,t_2)$，则 $\phi[t/x]=P(t_1[t/x], t_2[t/x])$；  
2. $(\phi\square\psi)[t/x]=\phi[t/x]\square\psi[t/x]$；  
3. $(Qx\phi)[t/x]=Qx\phi$；  
4. $(Qy\phi)[t/x]=Qy\phi[t/x]$；  
5. $\phi[t/x]$ 只有当 “t is free for x in $\phi$” 时才是安全的．这包括下面的情况：  
5.1 如果 $\phi=P(\dots)$，那么 “t is free for x in $\phi$”；  
5.2 “t is free for x in $\phi\square\psi$” 当且仅当 “t is free for x in $\phi$” 且 “t is free for x in $\psi$”；  
5.3 “t is free for x in $Qx\phi$” 永远成立；  
5.4 “t is free for x in $Qy\phi$” 当且仅当 $x\notin FV(\phi)$ 或者 $y\notin FV(t)$ 且 “t is free for x in $\phi$”．  
6. 0元谓词（也就是所谓的“布尔常值”）也可以被替换，这里仍需要一些安全规则，略去．

第5条的若干规则主要针对于把 “$\exists x, x=y+1$” 变成 “$\exists x, x=x+1$” 这样的不合期望的替换．另外，“$\forall x, \exists x, (x^2=x+1) \land (\exists x, x=0)$” 这样的命题是合法的，最内层的 $x$ 和中间层的 $x$ 是两个不同的东西，可以类比程序中函数内的变量允许和函数外的变量重名；最外层的 $\forall x$ 没有任何作用．

现在来重新回到语义的层面．

**定义：** 以下是若干和记号 $\vDash$ 相关的定义：  
- 若 $FV(\phi)=\varnothing$，称 $\mathfrak{A}\vDash\phi$ 如果 $[\![\phi]\!]_\mathfrak{A}=1$．  
- 若 $FV(\phi)\neq\varnothing$，比如 $FV(\phi)=\{x_1,x_2,x_3\}$， $\mathfrak{A}\vDash\phi$ 则等价于 $\forall x_1 \forall x_2 \forall x_3 \phi$．  
进一步地，$\mathfrak{A}\vDash\Gamma$ 表示对所有 $\phi\in\Gamma$ 都有 $\mathfrak{A}\vDash\phi$，此时我们称 $\mathfrak{A}$ 是 $\Gamma$ 的一个 **Model**．  
$\vDash\phi$ 表示 $\mathfrak{A}\vDash\phi$ 对所有“具有相同 type”的 structure $\mathfrak{A}$ 成立．（例如，“$\forall x, x=x$”是满足 $\vDash\phi$ 的，这里隐含要求了所有 structure 都具有相同含义的等号．）  
- 若 $\mathfrak{A}\vDash\Gamma$ 能推得 $\mathfrak{A}\vDash\phi$，则记 $\Gamma\vDash\phi$．

等号常常附带以下的公理：  
1. $\forall x, x=x$；  
2. $\forall x, \forall y, x=y\rightarrow y=x$；  
3. $\forall x\forall y\forall z, (x=y)\land(y=z)\rightarrow(x=z)$；  
4. $\forall x_1\dots\forall x_k\forall y_1\dots\forall y_k, (x_1=y_1)\land\dots\land(x_k=y_k)\rightarrow (t(x_1,\dots,x_k)=t(y_1,\dots,y_k))$；  
$\forall x_1\dots\forall x_k\forall y_1\dots\forall y_k, (x_1=y_1)\land\dots\land(x_k=y_k)\rightarrow(\phi(x_1,\dots,x_k)\leftrightarrow\phi(y_1,\dots,y_k))$．

这些公理也可以作为演绎规则（参见下面介绍自然演绎的部分），把它们当成推理规则和当成公理是等价的．

下面来介绍扩充的自然演绎规则．在原先的规则中，我们需要加上以下的规则：

**命题：** （自然演绎规则）  
- $\dfrac{D \vdash \phi(x)}{\forall x\phi (x)}$ ($\forall I$)  
（注：这条规则的语言表述是，如果 $\Gamma\vdash\phi(x)$对任何$x\notin FV(\psi)$和任意$\psi\in\Gamma$成立，那么就有 $\Gamma\vdash\forall x\phi(x)$．换言之，推导 $\phi(x)$的过程用到了许多条件，这些条件不能对 $x$ 有限制．）  
- $\dfrac{\forall x\phi(x)}{\phi(t)}$ ($\forall E$)  
定义 $\exists=\lnot\forall\lnot$．换言之 $\exists$ 仅作为简写，在这里不纳入字符集．  
下面的规则则对应于等号的四条公理：  
- $\dfrac{}{x=x}$ ($\text{IR}_1$)  
- $\dfrac{x=y}{y=x}$ ($\text{IR}_2$)  
- $\dfrac{x=y \quad y=z}{x=z}$ ($\text{IR}_3$)  
- $\dfrac{x_1=y_1 \quad \dots}{t(x_1,\dots,x_k)=t(y_1,\dots,y_k)}$ ($\text{IR}_4$)  
- $\dfrac{x_1=y_1 \quad \dots}{\phi(x_1,\dots,x_k)\leftrightarrow\phi(y_1,\dots,y_k)}$ ($\text{IR}_4$)  

下面是一个例子，推导 $\forall x\phi(x) \vdash \exists x\phi(x)$．  
$$
\dfrac{\dfrac{\forall x\phi(x)}{\phi(x)} \forall E \quad \dfrac{[\forall x\lnot \phi(x)]}{\lnot\phi(x)} \forall E}{\dfrac{\bot}{\exists x\phi(x)} \rightarrow I} \rightarrow E
$$

下面以 Peano 公理作为例子．Peano 公理系统所使用的字符集包括函数 $+,\cdot,\mathsf{s}$，其中 s 表示后继元运算，谓词只需要 $=$，常数符号仅需要 $0, 1$．我们用简写 PA 指代以下公理：  
- $\forall x (\lnot \mathsf{s}(x)=0)$  
- $\forall x\forall y(\mathsf{s}(x)=\mathsf{s}(y)\rightarrow x=y)$  
- $\forall x(x+0=x)$  
- $\forall x\forall y(x+\mathsf{s}(y)=\mathsf{s}(x+y))$  
- $\forall x(x\cdot 1=x)$  
- $\forall x(x\cdot \mathsf{s}(y) = xy+x)$  
- $\phi(0) \land \forall x(\phi(x)\rightarrow\phi(\mathsf{s}(x)))\rightarrow \forall x\phi(x)$

以推导 $\forall x(0+x=x)$ 为例，这里把等号的性质作为公理而非推导规则．为简便，记 $\phi(x)$ 为 $0+x=x$（由于证明树太长，将其拆分成几个部分）：  
$$
\dfrac{\forall x(x+0=x)}{0+0=0\text{，即 } \phi(0)} \forall E
$$
$$
\dfrac{\dfrac{\dfrac{\forall xy(x=y\rightarrow\mathsf{s}(x)=\mathsf{s}(y))}{0+x=x \rightarrow \mathsf{s}(0+x)=\mathsf{s}(x)} \forall E \quad [0+x=x]}{\mathsf{s}(0+x)=\mathsf{s}(x)} \rightarrow E \quad \dfrac{\forall xy(x+\mathsf{s}(y)=\mathsf{s}(x+y))}{0+\mathsf{s}(x)=\mathsf{s}(0+x)} \forall E}{(0+\mathsf{s}(x)=\mathsf{s}(0+x))\land(\mathsf{s}(0+x)=\mathsf{s}(x))\text{，简记为 } \psi(x)} \land I
$$
$$
\dfrac{\dfrac{\dfrac{\forall xyz((x=y)\land(y=z)\rightarrow x=z)}{\psi(x)\rightarrow 0+\mathsf{s}(x)=\mathsf{s}(x)} \forall E \quad \psi(x)}{0+\mathsf{s}(x)=\mathsf{s}(x)} \rightarrow E}{\dfrac{\dfrac{0+x=x\rightarrow 0+\mathsf{s}(x)=\mathsf{s}(x)}{\forall x(\phi(x)\rightarrow\phi(\mathsf{s}(x)))}\forall I \quad \phi(0)}{\dfrac{\phi(0)\land\forall x(\phi(x)\rightarrow\phi(\mathsf{s}(x))) \quad \text{PA}_7}{\forall x\phi(x)} \rightarrow E} \land I} \rightarrow I
$$

## 谓词逻辑的完备性

**定义：** 如果 $\Gamma \nvdash \bot$，就说 $\Gamma$ 是一致的 **(Consistent)**．

本节的目标是下面的定理：

**定理：** 如果 $\Gamma$ 是一致的，那么存在模型 $\mathfrak{A}$ 使 $\mathfrak{A}\vDash\Gamma$．

先准备若干定义．

**定义：** 若集合 $T$ 满足若 $T\vdash \phi$ 则 $\phi\in T$，则称 $T$ 是一个 **Theorem**．等价的说法是，$T=\{\text{closed sentence } \phi \text{ s.t. } \Gamma\vdash\phi\}$，这里的 $\Gamma$ 为公理集．

**定义：** 若 theorem $T$ 满足：对于每个 sentence $\exists x\phi(x) \in T$，存在常数符号 $c$ 使得 $\phi(c)\in T$，就称 $T$ 是一个 **Henkin Theorem**． 

回顾先前提到的扩张字符集的问题．用 $\Sigma$ 表示符号表，将 $T$ 中使用的全部符号构成的集合 $L \subseteq \Sigma$ 称为一个 **Language**．当我们引进新的字符之后也会产生新的定理，这就引出了下面的概念：

**定义：** 设 $T' \supseteq T$ 是扩张后的 $T$，如果 $T' \cap L = T$，就称 $T'$ 是 $T$ 的**保守(conservative)扩张**．  
这个定义相当于说新推出的定理一定使用了新的符号，不添加新符号就只能推出原本的东西．

下面，对于 $T$ 中的每一个 sentence $\exists x\phi(x)$ 引入新常数符号 $c_\phi$ 和新的公理 $\exists x\phi(x)\rightarrow \phi(c_\phi)$，扩张后的结果记作 $T^*$ 和 $L^*$．此时，我们断言：

**引理：** $T^*$ 是 $T$ 的保守扩张．等价地说，如果 $T, \exists x\phi(x)\rightarrow\phi(c_\phi)\vdash\psi$，$\psi\in L$，那么 $\psi\in T$．

**证明：** 证明过程是一系列推导，这里只列出主要步骤，省略具体的推导过程．  
- $T\vdash (\exists x\phi(x) \rightarrow \phi(c_\phi))\rightarrow\psi$
- $T\vdash (\exists x\phi(x) \rightarrow \phi(y))\rightarrow\psi$
- $T\vdash \forall y((\exists x\phi(x) \rightarrow \phi(y))\rightarrow\psi)$
- $T \vdash \exists y(\exists x\phi(x)\rightarrow\phi(y))\rightarrow\psi$
- $T \vdash (\exists x\phi(x) \rightarrow \exists y\phi(y))\rightarrow \psi$
- $T \vdash \psi$

注意，此时的 $T^*$ 未必是 Henkin Theorem，因为虽然 $T$ 中原有的命题都符合了 Henkin Theorem 的要求，但新产生的命题未必．所以，我们需要不断地进行扩张，即令 $T_0 = T$，$T_1 = T_0^*$，$\dots$，$T_{n+1}=T_n^*$，$\dots$，最后令 $T_\epsilon = \bigcup_{i=0}^\infty T_i$，我们断言：

**引理：** $T_\epsilon$ 是 Henkin Theorem，并且也是 $T$ 的保守扩张．

**证明：** 对于每个 $\exists x\phi(x) \in T_\epsilon$，$\exists i$ 使得 $\exists x\phi(x) \in T_i$，进而 $\exists c_\phi \in T_{i+1}$ 使得 $\phi(c_\phi)$，也即 $\exists c_\phi\in T_\epsilon$ 使得 $\phi(c_\phi)$．这样就说明了 $T_\epsilon$ 是 Henkin 的．同时，$T_\epsilon \cap L = (\bigcup_{i=0}^\infty T_i) \cap L = \bigcup_{i=0}^\infty (T_i\cap L) = T$，这样就说明了扩张是保守的．

基于以上的内容，我们就可以来定义所需的模型 $\mathfrak{A}$．我们取：  
- $\Omega = \{\text{closed term } t \in L_\epsilon\} \text{ i.e. 不含变量的 term}$  
- $[\![f(t_1, \dots, f_n)]\!] = f([\![t_1]\!], \dots, [\![t_n]\!]) = f(t_1, \dots, t_n)$  
- $[\![P(t_1, t_2)]\!] = \begin{cases} 1, & \text{如果 } P(t_1, t_2) \in T_\epsilon \\ 0, & \text{otherwise} \end{cases}$

但是这里可能存在的小问题是：假如 $P(t_1, t_2) \notin T_\epsilon$ 并且 $\lnot P(t_1, t_2) \notin T_\epsilon$，那么 $[\![P(t_1, t_2)]\!]$ 可能均为 $0$．（全为 $1$ 是不可能的，由于 $T \nvdash \bot$，扩张是保守特，故 $T_\epsilon \nvdash \bot$）为了解决这个问题，再作扩张 $T_\epsilon \rightarrow T_\epsilon^*$，扩张的方式是：如果 $\phi, \lnot \phi \notin T_\epsilon$，就将其中之一作为新公理．（注：如果 $T_\epsilon$ 不可数，这里需要 Zorn 引理；暂且略去）之后，对于非 $=$ 的谓词，我们就可以定义 $[\![\phi]\!]$ 为：  
$$ [\![\phi]\!] = \begin{cases} 1, & \text{如果 } \phi \in T_\epsilon^* \\ 0, & \text{如果 } \phi\notin T_\epsilon^* \end{cases} $$

对于谓词 $=$，我们需要一些特殊处理： 

$$ [\![t_1 = c]\!] = \begin{cases} 1, & \text{如果 } t_1=c\in T_\epsilon^* \\ 0, & \text{otherwise} \end{cases} $$

为了方便，可以定义 $\Omega$ 上的等价关系 $t_1 \sim t_2$ 为 $t_1=t_2\in T_\epsilon^*$，然后将原本的 $\Omega$ 修改为商集 $\Omega / \sim$，将 $[\![t]\!]$ 定义为 $t$ 所在的等价类，这样就可以保证 $=$ 的行为符合预期．

至此，我们就证明了本节的目标．事实上，原定理的逆定理也是正确的，不过那是一个平凡的命题．

另外，这里构造的 $\Omega$ 和 $L$ 是“差不多大”的，例如容易看出如果 $L$ 可数，也有 $\Omega$ 可数．实际上，借助更多知识（Löwenheim-Skolem 定理？）可知，我们可以对实数集这样的不可数集也给出可数的模型；也可以通过向自然数的符号集中加入更多符号来获得一个不可数的模型（？）前面的区域以后再来探索吧．

## ZFC 集合论

集合论的字符集没有太多需要说明的，谓词主要是 $=$ 和 $\in$．下面来以此介绍集合论的公理．

**命题：外延公理（Axiom of Extensionality）** 

$$\forall xy((\forall z(z \in x \leftrightarrow z \in y)) \rightarrow x=y)$$ 

这个公理相当于说两个集合相等当且仅当它们具有相同的元素．

**命题：分离公理模式（Axiom Schema of Specification）** 

$$\forall x \exists y \forall z(z \in y \leftrightarrow (z \in x) \land \phi(z))$$  

多变量的形式：

$$\forall x w_1 \dots w_t \exists y \forall z(z \in y \leftrightarrow (z \in x) \land \phi(z, w_1, \dots, w_t))$$  

这等同于说如果 $x$ 是集合，那么 $\{y \in x \mid \phi(y)\}$ 是集合．

**推论：空集存在且唯一．**  

**证明：** 在分离公理模式中取 $\phi$ 为 $\bot$，由此知 $\forall x \exists y \forall z(z \in y \leftrightarrow \bot)$，利用 $\forall E$ 可知 $\exists y \forall z(z \in y \leftrightarrow \bot)$，这个 $y$ 即为“空集”．由于任何“空集”包含相同的元素（都不包含任何元素），由外延公理可知所有“空集”是同一个集合．

**推论：交集存在．**  

**证明：** 利用分离公理模式，$\forall x \forall y \exists z \forall w(w \in z \leftrightarrow (w \in x \land w \in y))$．    

**命题：配对公理（Axiom of Pairing）** 

$$\forall x \forall y \exists z(x \in z \land y \in z)$$  

如果 $x$，$y$ 是集合，那么存在集合 $z$ 含有 $x$ 和 $y$．在此基础上利用分离公理去掉其他的元素，等价于说如果 $x$，$y$ 是集合，那么 $\{x, y\}$ 是集合．

**命题：并集公理（Axiom of Union）** 

$$\forall x \exists y \forall zw(z \in x \land w \in z \rightarrow w \in y)$$  

等同于说如果 $x$ 是集合，那么广义并 $\cup x$ 是集合．

**推论：并集存在．**  

**证明：** 在上文得到的 $\{x, y\}$ 基础上取并即可．

**命题：幂集公理（Axiom of Powerset）** 

$$\forall x \exists y \forall z(z \subseteq x \rightarrow z \in y)$$  

这里引入了子集符号 $\subseteq$，其含义为 $z \subseteq x \leftrightarrow \forall y(y \in z \rightarrow y \in x)$．这个公理等价于说如果 $x$ 是集合，那么 $2^x$ 是集合．

**命题：无穷公理（Axiom of Infinity）** 

$$\exists N(\varnothing \in N \land (\forall y(y \in N \rightarrow y \cup \{y\} \in N)))$$  

无穷公理中所出现的集合 $N$ 一般被称为**归纳集**．无穷公理可推出自然数集 $\mathbb{N}$ 存在，这是因为我们在集合论中以下面的方式定义自然数：  
$0 = \varnothing$  
$1 = \{\varnothing\}$  
$\dots$  
$n + 1 = \{0, 1, \dots, n\} = n \cup \{n\}$

**命题：替换公理模式（Axiom Schema of Replacement）** 

$$\forall A w_1 \dots w_t(\forall x(x \in A \rightarrow \exists !y\phi(y, x, w_1, \dots, w_t))) \rightarrow (\exists B \forall x (x \in A \rightarrow \forall y (\phi(y, x, w_1, \dots, w_t) \rightarrow y \in B)))$$  

这等同于说如果在集合 $A$ 上给出一个映射 $f_w$，那么存在集合 $B=f_w(A)$．

**命题：正则公理（Axiom of Regularity）** 

$$\forall x(x \neq \varnothing \rightarrow \exists y(y \in x \land y \cap x = \varnothing))$$  

这个公理的作用是防止一些“不正常的”集合出现．比如说考虑集合 $S=\{a,b,c,T\}$，$T=\{a,b,c,S\}$，那么假如 $S=T$，那么 $S$ 和 $T$ 具有相同的元素，所以 $S=T$；假如 $S\neq T$，那么 $S$ 和 $T$ 具有不同的元素，所以 $S\neq T$；$S$，$T$ 可以相等也可以不相等．再例如自己属于自己的集合 $S=\{S\}$．正则公理要求集合之间需要存在一种正确的层级关系，避免这样的“病态”集合．

**命题：选择公理（Axiom of Choice）** 

$$\forall S(\forall x(x \in S \rightarrow x \neq \varnothing \land \forall x \forall y (x \in S \land y \in S \rightarrow x \cap y = \varnothing)) \rightarrow (\exists T \forall x (x \in S \rightarrow \exists !y (y \in x \cap T))))$$  

选择公理相当于说，如果一个集合 $S$ 中含有若干互不相交的非空集合，那么就可以从每个非空集合中取出一个元素来构成新的集合 $T$．这个公理在历史上存在非常大的争议，这里不作详述．

至此 ZFC 的九条公理已经叙述完毕．在此基础上可以定义有序对，笛卡尔积，关系，函数等等，从自然数出发可以定义整数，有理数等等，在此也不再详述．

## Gödel 不完备定理

Gödel 不完备定理指出，如果一个形式系统是基于“足够强”的逻辑建立起来的，它就存在不可证的真命题．这种矛盾产生自含义为 “$\phi$ 不可以被证明” 的命题 $\phi$，无论它正确或是不正确都会产生矛盾，所以只能“不去证明”它．命题逻辑是一种表达能力很弱的逻辑系统，弱到它无法描述这样的 $\phi$，从而完备性没有被破坏．但是对于足够描述自然数和集合论的一阶逻辑来说，情况就有所不同．

我们的主要想法是，对每个命题进行“编码”，得到从命题集合到自然数集的单射．例如我们规定：

| 编码 | 字符 | 编码 | 字符 |
| :--- | :--- | :--- | :--- |
| 1 | $\land$ | 10 | $\varnothing$ |
| 2 | $\rightarrow$ | 11 | 1 |
| 3 | $\lnot$ | 12 | $x_0$ |
| 4 | $\bot$ | 12 00 | $x_1$ |
| 5 | $=$ | 12 00 00 | $x_2$ |
| 6 | $($ | $\dots$ | |
| 7 | $)$ | | |
| 8 | $\exists$ | | |
| 9 | $\forall$ | | |

并且采用 100 进制，用 “$\quad$” 表示命题的编码，就可以有：  
“$\forall x_0 \quad x_0 = x_1$” = 91212051200

下面可以定义集合 TERM 和 FOR，表示所有的 term 和 formula 的编码所构成的集合．例如，TERM 可以被定义为满足以下性质的最小集合：  
- $10 \in \text{TERM} \land 11 \in \text{TERM}$  
- $\forall k \in \mathbb{N}, 12 \cdot 100 ^ k \in \text{TERM}$  
- $\forall t_1, t_2, t_1 \in \text{TERM} \land t_2 \in \text{TERM} \rightarrow “f(t_1, t_2)” \in \text{TERM}$  

FOR 可以类似地定义．（注：我们这里不一定要把它们定义成集合；换言之，我们的证明过程可以不需要使用集合论，只需要自然数；反过来只用集合论不用自然数也是可以的，只是我们这里采取的相对简化的处理方式，同时使用集合论和自然数，这样处理起来比较方便．）特别地，我们可以定义集合 PROV 为所有可被证明的命题的编码构成的集合 $\{ “\phi” \mid \text{Axiom} \vdash \phi \}$（PROV 可以根据自然演绎的推导规则去定义），有了这些我们就可以尝试写出开头提到的命题 $\phi$．这里有一点小问题在于，$\phi$ 的编码可能比 $\phi$ 要长得多，我们需要特别的方式绕开这一点．一种可行的构造是：  

$$
\phi = (\forall \text{PROV} (\text{some conditions} \rightarrow \forall x (x = \square \rightarrow \forall y(y \text{ 是 } x \text{ 中把第一个 } “z” \text{ 替换成 } x \text{ 后的数} \rightarrow y \notin \text{PROV})))
$$  

其中 
$$
\square = “(\forall \text{PROV} (\text{some conditions} \rightarrow \forall x (x = z \rightarrow \forall y(y \text{ 是 } x \text{ 中把第一个 } “z” \text{ 替换成 } x \text{ 后的数} \rightarrow y \notin \text{PROV})))”
$$  

some conditions 指的是集合 PROV 满足的某一些条件．注意到，把 $\square$ 这个编码序列中的首个 “$z$” 替换成 $x$ 之后得到的正是 “$\phi$”，所以 $\phi$ 所表达的正是 “$\phi$” $\notin \text{PROV}$，即“$\phi$ 无法被证明”，这样就得到了不完备性定理． 

**定理：** **Gödel（第一）不完备定理** 任意一个包含一阶逻辑的（归纳可枚举的）形式系统都存在不可被证明的真命题．

下面来阐述另一种证明方式，这种证明需要借助**图灵机**来进行．考虑一种机器 TM Verifier(“$\phi$”, proof)，如果 proof 可以证明 $\phi$ 就输出 1，如果不能就输出 0．再考虑机器 TM Test，定义如下：

```text
TM Test(“phi”)
    try all proof
        if Verifier(“phi”, proof) = 1, return 1
        if Verifier(“not phi”, proof) = 1, return 0
```

如果完备性成立，那么 TM Test 总能输出结果．但是，一阶逻辑足以将图灵停机问题也进行编码，这是因为对任何图灵机 $T$ 和输入 $x$，我们能定义集合 $S$ 使得初始状态 $(x, 0, \text{state(init)}) \in S$ 且 $\forall \text{state} \in S, \text{next(state)} \in S$，从而将图灵机的运行过程用集合来表达，进而可以编码．把编码后的图灵停机问题输入给 TM Test 即可解决图灵停机问题，但是“众所周知”后者是不可判定的，从而产生矛盾．

下面来证明图灵停机问题不可判定．用 $|\cdot|$ 表示 $\cdot$ 的编码的长度．按如下方式定义 $f(n)$：对于所有满足 $|T, x| \le n$ 的 $T, x$，若 $T(x)$ 能停机，则停机的步数不超过 $f(n)$．假如图灵停机问题是可判定的，那么 $f(n)$ 就可以通过下面的方式来计算：

```text
if T(x) halts
    count its steps
output max(steps)
```

但是，若 $f(n)$ 是可计算的，并设 $T_f(n) = f(n)$，我们可以构造图灵机 $T'$：

```text
t = Tf(|T'|)
wait t steps
halt
```

（由于数字 $n$ 的二进制编码位数为 $O(\log n)$，因此将 $|T'|$ 出现在 $T'$ 自己的内部是没有问题的．）此时，$T'$ 的编码长度为 $|T'|$，它没有输入，故根据 $f(n)$ 的定义和 $f(n)$ 可计算的前提，$T'$ 必须在 $f(|T'|) = t$ 步内停机，然而实际上根据程序可知 $T'$ 必然在 $t$ 步之后停机，矛盾！这样就说明了图灵停机问题不可判定．