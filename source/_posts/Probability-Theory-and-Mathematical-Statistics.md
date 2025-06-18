---
title: MA212 概率论与数理统计
date: 2024-08-19 19:46:18
tags: 
  - CSE Learning
  - Maths
categories:
  - 2023 Fall
mathjax: true
description: >-
  仅作为其他课程复习用，不记录全部知识点。
---

# 第一章 概率

## 记数方法

- 古典概型
    - $\Omega$ 只含有限个样本点
    - 每个样本点出现是等可能的

{%cq%}
$
P(A)=\frac{A \text{的有利场合数}}{\text{样本点总数}}=\frac{k}{n}
$
{%endcq%}


- 几何概型
    - 对比古典概型有无限个样本点



## 全概率定律

**Def.** 设 $\Omega$ 为样本空间，若事件 $B_1,B_2,\cdots ,B_n$ 满足：
1. $B_1,B_2,\cdots ,B_n$ 两两不相容
2. $B_1 \cup B_2 \cup \cdots \cup B_n = \Omega$

则称 $B_1,B_2,\cdots ,B_n$ 为样本空间的一个<font color=red>划分</font>。

由此推出全概率公式：

{%cq%}
$
P(A)=\sum_{i=1}^{n} P(A|B_i) \cdot P(B_i)
$
{%endcq%}



由全概率公式和条件概率的乘法公式推导出 <font color=red>Bayes 公式</font>：

{%cq%}
$
P(B_i|A)=\frac{P(A|B_i)P(B_i)}{\sum_{j=1}^{n} P(A|B_j)P(B_j)}
$
{%endcq%}

Bayes 公式的实际意义：

假定 $B_1,B_2,\cdots ,B_n$ 为导致实验结果的“原因”，称 $P(B_i) (i=1,2,\cdots ,n)$ 为<font color=red>先验概率</font>。

若试验产生事件 A ，则要探讨事件发生的“原因”：称 $P(B_i|A)$ 为<font color=red>后验概率</font>，称 $P(A|B_i)$ 为<font color=red>原因概率</font>



# 第二章 随机变量

## 离散随机变量

定义：

{%cq%}
$
P\{ X=x_k \}= p(x_k),\ \ (k=1,2,3,\cdots)
$
{%endcq%}

为离散型 r.v. X 的概率质量函数 (PMF)



{%cq%}
$
F(x)=P\{ X \le x \}, -\infty \lt x \lt \infty
$
{%endcq%}

为离散型 r.v. X 的累计分布函数 (CDF)



**泊松定理**

设 $\lambda \gt 0$，$n$ 为正整数，$\lim_{n \to \infty} np_n=\lambda$，则有

{%cq%}
$
\lim_{n\to \infty} C^k_n p^k_n(1-p_n)^{n-k}=\frac{\lambda^{k}e^{-\lambda}}{k!}
$
{%endcq%}



## 连续随机变量

定义：

{%cq%}
$
F(x)=\int_{-\infty}^{x} f(t)\text{d}t,\ -\infty \lt x \lt \infty
$
{%endcq%}

其中 $f(t)$ 为连续型 r.v. X 的概率密度函数 (PDF)



**标准正态分布**

{%cq%}
$
\begin{array}{l}
&\Phi(x)=\int_{-\infty}^{x} \psi(x)\text{d}x \\
&\psi(x)=\frac{1}{\sqrt{2\pi}\sigma} e^{-\frac{(x-\mu)^2}{2\sigma^2}} \\
&s.t.\ \mu=0,\ \sigma^2=1
\end{array}
$
{%endcq%}



## 随机变量的函数

例：设随机变量 $X$，$Y$，满足 $Y=aX+b$，如何通过 $X$ 的概率密度分布求出 $Y$ 的 PDF？

解：令 $x=g(y)=\frac{y-b}{a}$，可得 $F_Y(y)=P\{ Y\le y \}=P\{ X\le\frac{y-b}{a} \}=F_X(\frac{y-b}{a})$。

化简得：$f_Y(y)=F_{X}'(g(y))=(g(y))'f_X(g(y))$


如正态分布 $X\sim N(\mu,\sigma^2)$ 的线性函数 $aX+b \sim N(a\mu +b,(a\sigma)^2)$ 也是正态分布



# 第三章 联合分布

## 联合随机变量

定义：

{%cq%}
$
F(X,Y)\triangleq P\{ X\le x,Y\le y \}\ \ s.t.\ \{x,y\}\in \mathbb{R}
$
{%endcq%}

为 $X$ 与 $Y$ 的联合累积分布函数。

{%cq%}
$
F_X(x)= P\{ X\le x,Y\le \infty \}\ \ s.t.\ x\in \mathbb{R}
$
{%endcq%}

称为 $X$ 的边际分布，$Y$ 同理。



**概率密度函数**

{%cq%}
$
F(x,y)=\int_{-\infty}^{x} \int_{-\infty}^{y} f(u,v)\text{d}u\text{d}v,\ s.t.\ \{ x,y\}\in\mathbb{R}
$
{%endcq%}

则 $f(x,y)$ 为 $X$，$Y$ 的概率密度函数(joint PDF)


**边际密度**

{%cq%}
$
f_X(u)=\int_{-\infty}^{\infty} f(u,y)\text{d}y
$
{%endcq%}

称为 $X$ 的边际密度，$Y$ 同理。



## 独立随机变量

{%cq%}
$
f(x,y)=f_X(x)\cdot f_Y(y)
$
{%endcq%}

当上式成立时，$X$，$Y$ 相互独立，即相关系数 $\rho=0$。



## 条件分布

（只看连续，离散情况容易推导）

{%cq%}
$
\begin{array}{l}
P(X\le x|y\le Y\le y+\epsilon) &=\frac{P\{X\le x,y\le Y\le y+\epsilon\}}{P\{ y\le Y\le y+\epsilon\ \}} \\
&=\frac{\int_{-\infty}^{x}\int_{y}^{y+\epsilon} f(u,v)\text{d}u\text{d}v}{\int_{y}^{y+\epsilon} f_Y(v)\text{d}v} \\
&=\frac{\epsilon\int_{-\infty}^{x}f(u,y_\epsilon)\text{d}u}{\epsilon f_Y(\tilde{y}_{\epsilon})} \\
&= \int_{-\infty}^{x}\frac{f(u,y)}{f_Y(y)} \text{d}u\ \ (\epsilon\to 0)
\end{array}
$
{%endcq%}

定义 $\frac{f(u,y)}{f_Y(y)} \triangleq f_{X|Y}(x|y)$ 为 $Y=y$ 下 $X$ 的<font color=red>条件密度</font>



## 联合分布随机变量的函数

1 . $Z=X+Y$

利用卷积公式 (可写作 $f_X * f_Y$)：

{%cq%}
$
\begin{array}{l}
&f_Z(z)=\int_{-\infty}^{\infty}f_X(z-y)f_Y(y)\text{d}y \\
\text{or } &f_Z(z)=\int_{-\infty}^{\infty}f_X(x)f_Y(z-x)\text{d}x
\end{array}
$
{%endcq%}

<font color=red>前提：</font> 

$X$，$Y$ 相互独立（如不独立，可利用联合分布、条件分布求得，或变换成独立变量再求解）。



2 . $Z=\frac{X}{Y}$

{%cq%}
$
\begin{array}{l}
&\text{由于 }F_Z(z)=P\{ X/Y\le z \}=\underset{\frac{x}{y}\le z}{\int\int} f(x,y)\text{d}x \text{d}y \text{ 积分区域可能不是矩形} \\
&\text{为简化积分计算，使用 }\textbf{J}=\frac{\partial{(x,y)}}{\partial{(u, v)}}= \Large{\left| \begin{array}{c} \frac{\partial{x}}{\partial{u}} & \frac{\partial{x}}{\partial{v}} \\ \frac{\partial{y}}{\partial{u}} & \frac{\partial{y}}{\partial{v}} \end{array} \right|} \\
&\text{得 }F_Z(z)=\underset{\Omega}{\int\int} f[x(u,v),y(u,v)] |\textbf{J}|\text{d}u \text{d}v
\end{array}
$
{%endcq%}



## 顺序统计量

设 $X_i\sim f(x)$ 是独立同分布的连续型 r.v.，则对于顺序统计量 $X_{(1)}(\min),\cdots ,X_{(n)}(\max)$ ，如何求 $X_{(k)}$ 的密度？

解：对于充分小的空间 $[x,x+\text{d}x]$，有

{%cq%}
$
\begin{array}{l}
&P\{ x\lt X_{(k)} \lt x+\text{d}x \}=\left(\begin{array}{c}n \\ k-1 \end{array} \right) F(x)^{k-1} \left(\begin{array}{c}n-k+1 \\ 1 \end{array} \right) [F(x+\text{d}x)-F(x)] \left(\begin{array}{c}n-k \\ n-k  \end{array} \right) [1-F(x+\text{d}x)^{n-k}] \\
\therefore\ \ &f_k(x)=\frac{\text{d}P\{ x\lt X_{(k)} \lt x+\text{d}x \}}{\text{d}x}=\frac{n!}{(k-1)!(n-k)!}F(x)^{k-1} f(x)[1-F(x)]^{n-k}
\end{array}
$
{%endcq%}

称此为 Veta 分布，记为 $X\sim Beta(k, n-k+1)$

Beta 密度用于刻画 [0, 1] 上的随机变量：

{%cq%}
$
\begin{array}{l}
&f(u)=\frac{\Gamma(a+b)}{\Gamma(a)\Gamma(b)}u^{a-1}(1-u)^{b-1} \\
s.t.\ &\Gamma(x)=(x-1)! ,\ 0\le u\le 1
\end{array}
$
{%endcq%}



# 第四章 随机变量的数字特征

## 协方差

定义：

{%cq%}
$
\begin{array}{l}
&Cov(X,Y)\triangleq E[(X-E(X))\cdot (Y-E(Y))]
\end{array}
$
{%endcq%}

称为 X，Y 的协方差，其相关系数表示为：

{%cq%}
$
\begin{array}{l}
&\rho_{XY}\triangleq Cov(X^{\*},Y^{\*})=\frac{Cov(X,Y)}{\sqrt{D(X)}\sqrt{E(Y)}}
\end{array}
$
{%endcq%}



利用这个属性进行 X，Y 线性拟合的计算：

记均方误差为 $e=E[(Y-\hat{Y})^2]=E[(Y-(aX+b))^2]$

令 $\left\{ \begin{array}{l} &\frac{\partial{e}}{\partial{b}}=2b+2aE(X)-2E(Y)=0 \\ &\frac{\partial{e}}{\partial{a}}=2aE(X^2)-2E(XY)+2bE(X)=0 \end{array} \right.$

解得 $\left\{ \begin{array}{l} &b_0=\frac{Cov(X,Y)}{D(X)} \\ &a_0=E(Y)-E(X)\cdot b_0 \end{array} \right.$

进一步得 $\underset{a,b}{\min e}=D(Y)(1-\frac{Cov^2(X,Y)}{D(X)D(Y)})=D(Y)(1-\rho^2_{XY})$

<font color=red>注意：</font>$\rho_{XY}=0$ 并不意味着 X，Y 相互独立！（但正态分布能证明 不相关 = 独立）



定义：

对 r.v. X，Y，

$E(X^k)\ \ \ (k=1,2,\cdots)$ 为 <font color=red>k 阶原点矩</font>

$E[(X-E(X))^k]\ \ \ (k=1,2,\cdots)$ 为 <font color=red>k 阶中心矩</font>

$E[(X-E(X))^k(Y-E(Y))^l]\ \ \ (k,l=1,2,\cdots)$ 为 <font color=red>k+l 阶混合中心矩</font>

> 因此，r.v. 的期望是一阶原点矩，方差是2阶中心矩，协方差是2阶混合中心矩。



## 条件期望

定义：

{%cq%}
$
\left\{\begin{array}{rl}
&E(h(Y)|X=x)=\sum_{y}h(y)p_{Y|X}(y|x) &\text{（离散）} \\
&E(h(Y)|X=x)=\int_{y}h(y)f_{Y|X}(y|x)\text{d}y &\text{（连续）}
\end{array}\right.
$
{%endcq%}

> 特殊情况下，$h(y)=y$

<font color=green>回顾泊松分布：</font> $X\sim P(\lambda t)$

$P(X=k)=\frac{(\lambda t)^k}{k!}e^{-\lambda t}\ \ \ k\in\mathbb{N}$  称为泊松强度。

例：考虑 [0, 1] 区间上均值为 $\lambda$ 的泊松流，令 N 是 [0, 1] 上点的个数。对于 $p\lt 1$，令 X 是 [0, p] 上点的个数。计算给定 N = n 的情况下，X 的条件分布和条件期望。

解：联合分布

$P\{X=x,N=n \}=\frac{(p\lambda)^xe^{-p\lambda}}{x!}\cdot \frac{((1-p)\lambda)^{(n-x)}e^{-(1-p)\lambda}}{(n-x)!}$

而 $N\sim P(\lambda)$

因此 $P\{X=x|N=n \}=\frac{n!}{x!(n-x)!}p^x(1-p)^{(n-x)}\sim b(n,p)$

从而 $X$ 的条件期望为 $np$。



# 第五章 数理统计（入门）

## 大数定律

（伯努利版）设 $P(A)=p$，则对任意 $\epsilon\gt 0$，有 $\color{red} \underset{n\to \infty}{\lim}=P\{|\frac{n_A}{n}-p|\ge \epsilon \}=0$

（切比雪夫版）$\{X_n\}$ 为独立随机变量列，且期望方差相同，则对任意 $\epsilon\gt 0$，有 $\color{red} \underset{n\to \infty}{\lim}=P\{|\frac{1}{n}\sum_{i=1}^{n}X_i-\mu|\ge \epsilon \}=0$

（这意味着样本量足够大时，期望可被样本的算术均值替代）



## 中心极限定理

若 $X_n$ 的分布 $F_n(x)$ 对任意 $x$ 满足

{%cq%}
$
\color{red}
\begin{array}{l}
&\underset{n\to \infty}{\lim} F_n(x)=\underset{n\to \infty}{\lim}P\{ \frac{\sum_{i=1}^{n}(X_i-\mu_i)}{\sqrt{\sum_{i=1}^{n}\sigma_i^2}}\le x \}=\psi(x)
\end{array}
$
{%endcq%}

则称 $\{X_n \}$ 服从中心极限定理（$\psi(x)$ 为标准正态）

特别当 $X_n$ 独立同分布，则有 $\underset{n\to \infty}{\lim}P\{ \frac{\sum_{i=1}^{n}X_i-n\mu_i}{\sqrt{n}\sigma_i}\le x \}=\psi(x)$

> 德莫夫-拉普拉斯中心极限定理：对 $\eta_n\sim b(n,p)$
> 
> $\frac{\eta_n-np}{\sqrt{np(1-p)}}\sim N(0,1)$
>
> 参考高尔顿钉板



# 第六章 数理统计（基础）

## 抽样分布

1 . $\chi^2$ - 分布

设 $X_1-X_n$ 是来自总体 $X\sim N(0,1)$ 的样本，令

{%cq%}
$
\begin{array}{l}
\chi^2=X_1^2+X_2^2+\cdots +X_n^2
\end{array}
$
{%endcq%}

称 $\chi^2$ 服从自由度为 $n$ 的 $\chi^2$ - 分布（也称卡方分布），记为 $\chi^2(n)$。

> <font color=blue>自由度：</font>自由度是二次型 $\chi^2=X_1^2+X_2^2+\cdots +X_n^2$ 的秩，即可独立变化的变量个数。

数字特征：

- $E(\chi^2)=n$
- $D(\chi^2)=2n$



2 . t - 分布

设 $X\sim N(0,1)$，$Y\sim \chi^2(n)$，且 $X$，$Y$ 独立，令

{%cq%}
$
\begin{array}{l}
t=X/\sqrt{Y/n}
\end{array}
$
{%endcq%}

称 $t$ 为服从自由度为 n 的 t - 分布，记为 $t(n)$。

性质：

- 数字特征
  - $E(t)=0$
  - $D(t)=\frac{n}{n+2}$
- 当 n 充分大时，T 近似服从 N(0, 1)，即趋近标准正态分布
  - 可证明 $\underset{n\to\infty}{\lim}f(x)=(2\pi)^{-\frac{1}{2}}e^{-\frac{x^2}{2}}$



3 . F - 分布

设 $U\sim \chi^2(n_1)$，$V\sim \chi^2(n_2)$，且 $U$，$V$ 独立，令

{%cq%}
$
\begin{array}{l}
F=\Large\frac{U/n_1}{V/n_2}
\end{array}
$
{%endcq%}

称 F 为服从自由度为 $(n_1,n_2)$ 的 F - 分布，记为 $F(n_1,n_2)$。



二级结论：

{%cq%}
$
\begin{array}{l}
&T\sim t(n) \Rightarrow T^2\sim F(1,n) \\
\text{证：} &T=\frac{X}{\sqrt{Y/n}} \Rightarrow T^2=\frac{X^2/1}{Y/n}, \\
\text{ 且 }&X^2,Y\text{ 仍相互独立}
\end{array}
$
{%endcq%}



## 抽样分布定理

1 . 设 $X_1\sim X_n$ 是来自总体 $X\sim N(\mu,\sigma^2)$ 的样本，则

{%cq%}
$
\begin{array}{l}
\bar{X}\sim N(\mu,\frac{\sigma^2}{n})
\end{array}
$
{%endcq%}

因为 $\bar{X}=(X_1+\cdots +X_n)/n$ ，而线性组合仍服从正态分布。

因此，$E(\bar{X})=\mu$，$D(\bar{X})=\frac{\sigma^2}{n}$



2 . 设 $X_1\sim X_n$ 是来自总体 $X\sim N(\mu,\sigma^2)$ 的样本，$\bar{X}$ 、$S^2$ 分别是样本均值和样本方差，则有

{%cq%}
$
\begin{array}{lr}
&\frac{(n-1)S^2}{\sigma^2}\sim \chi^2(n-1) &(1) \\
&\bar{X},S^2\text{ 相互独立} &(2)
\end{array}
$
{%endcq%}



3 . waiting...

4 . 

5 . 



# 第七章 参数估计

## 点估计

定义：设总体分布函数 $F(x,\theta)$ ， $X_1\sim X_n$ 为样本，构造一个统计量 $\theta=\theta(X_1,\cdots ,X_n)$ 来估计参数 $\theta$ ，则称为参数 $\theta$ 的**估计量**。

将观测值 $x_1,\cdots ,x_n$ 带入 $\theta(X_1,\cdots ,X_n)$ ，得到的 $\theta(x_1,\cdots ,x_n)$ 称为参数 $\theta$ 的**估计值**。


常用点估计法：

- 矩估计：设总体 $X\sim F(x;\theta)$ ，$\theta_1\sim \theta_m$ 未知，设对 n 个样本，总体矩都存在（即 $\alpha_k \triangleq E(X^k),(k=1,2,\cdots,m)$ ），由辛钦大数定律得

{%cq%}
$
\begin{array}{l}
&A_k=\frac{1}{n}\sum_{i=1}^{n}X^k_i \overset{P}{\longrightarrow}E(X^k)=\alpha_k\ \ (n\to\infty,k=1,2,\cdots,m) \\
\text{可认为 }&A_k\approx E(X^k)=\int{x^k}\text{d}F \triangleq \alpha_k(\theta_1,\cdots, \theta_m) & \\
\therefore & \left\{\begin{array}{l} 
&\alpha_1(\theta_1,\cdots, \theta_m)=E(X) \approx A_1 \\
&\alpha_2(\theta_1,\cdots, \theta_m)=E(X^2) \approx A_2 \\
&\vdots \\
&\alpha_m(\theta_1,\cdots, \theta_m)=E(X^m) \approx A_m
\end{array}\right.
\end{array}
$
{%endcq%}

解上述方程组得：

{%cq%}
$
\left\{
\begin{array}{l}
&\hat{\theta}_1=\hat{\theta}_1(A_1,A_2,\cdots,A_m) \\
&\vdots \\
&\hat{\theta}_m=\hat{\theta}_m(A_1,A_2,\cdots,A_m) \\
\end{array}
\right.
$
{%endcq%}

- 最大似然估计：构造似然函数 $L(\theta)$ ，通过求极大值点得到参数值

{%cq%}
$
L(\theta)=
\left\{
\begin{array}{lr}
&p(x_1,x_2,\cdots,x_n;\theta)=\prod_{i=1}^{n}p(x_i;\theta) &(\text{离散}) \\
&f(x_1,x_2,\cdots,x_n;\theta)=\prod_{i=1}^{n}f(x_i;\theta) &(\text{连续}) \\
\end{array}
\right.
$
{%endcq%}

取对数便于求偏导（对每个参数 $\theta_i$ 求偏导）：$\large\frac{\partial{\ln{L}}}{\partial{\theta_i}}=0$



**参数评价标准**

- 无偏性：$E(\hat\theta)=\theta$
- 有效性：$E(\hat\theta_1)=E(\hat\theta_2)=\theta$ 且 $D(\hat\theta_1)\le D(\hat\theta_2)$，则称 $\hat\theta_1$ 较 $\hat\theta_2$ 有效。
- 相合性（一致性）：设 $\hat\theta_n=\hat\theta(X_1,X_2,\cdots,X_n)$ 是 $\theta$ 的点估计，若 $\forall \theta\in\Theta$ 满足对 $\forall\epsilon\gt 0$ 有 $\color{red}\underset{n\to\infty}{\lim}P\{|\hat\theta_n -\theta|\ge \epsilon \}=0$ ，则称 $\hat\theta_n$ 是 $\theta$ 的<font color=red>相合估计</font>，记作 $\hat\theta_n\overset{P}{\longrightarrow}\theta(n\to\infty)$ 。



## 区间估计

> 区别：点估计构造一个参数统计量，而区间估计构造两个并将 $(\theta_1,\theta_2)$ 以一定的置信度作为 $\theta$ 的估算区间。

定义：设总体 $X\sim F(x;\theta)$ ，若存在 2 个统计量

{%cq%}
$
\begin{array}{lr}
&\underline{\theta}=\underline{\theta}(X_1,\cdots,X_n),\ \ \ \overline{\theta}=\overline{\theta}(X_1,\cdots,X_n) &(\underline{\theta}\lt\overline{\theta})
\end{array}
$
{%endcq%}

使得 $\forall\theta\in\Theta$ 有 $P\{\underline{\theta}\le\theta\le\overline{\theta}\}\ge 1-\alpha$ ，则称随机区间 $(\underline{\theta},\overline{\theta})$ 为 $\theta$ 的<font color=red>置信水平</font>为 $1-\alpha$ 的<font color=red>置信区间</font>，$\underline{\theta}$ 和 $\overline{\theta}$ 分别称为置信下限和置信上限。

- 区间估计一般方法
  - 枢轴法（对应 t-分布的应用）
  - 波动理论（对应卡方分布的应用）



**二级结论总结**

1. $\sigma^2$ 已知，对 $\mu$ 估计：$\color{red}(\bar{X}-u_{1-\frac{\alpha}{2}}\frac{\sigma}{\sqrt{n}},\bar{X}+u_{1-\frac{\alpha}{2}}\frac{\sigma}{\sqrt{n}})$
2. $\sigma^2$ 未知，对 $\mu$ 估计：$\color{red}(\bar{X}-\frac{S}{\sqrt{n}}t_{1-\frac{\alpha}{2}}(n-1),\bar{X}+\frac{S}{\sqrt{n}}t_{1-\frac{\alpha}{2}}(n-1))$
3. $\mu$ 未知，对 $\sigma^2$ 估计：$\large\color{red}(\frac{(n-1)S^2}{\chi^2_{1-\frac{\alpha}{2}}(n-1)}, \frac{(n-1)S^2}{\chi^2_{\frac{\alpha}{2}}(n-1)})$



*后两条的推导式：*

{%cq%}
$
\begin{array}{c}
&P\left\{ \frac{|\bar{X}-\mu|}{S/\sqrt{n}}\lt t_{1-\frac{\alpha}{2}}(n-1) \right\}=1-\alpha \\
&P\left\{\chi^2_{\frac{\alpha}{2}}(n-1)\lt \frac{(n-1)S^2}{\sigma^2}\lt\chi^2_{1-\frac{\alpha}{2}}(n-1) \right\}=1-\alpha
\end{array}
$
{%endcq%}



# 第八章 假设检验

## 一、建立对立的假设：

原假设（零假设）$H_0$ 和备择假设（对立假设）$H_1$

1. 保护原假设：原假设错误的“代价”必须小于备择假设（如原假设新药物有副作用）
2. 原假设趋于维持现状
3. 原假设取简单假设

## 二、给出检验统计量，确定拒绝域形式

拒绝域是拒绝原假设的样本值范围，其补集为接受域。

设置合理的 $C$ 值（待定常数），使得 $\bar{X}$ 大于/小于该常数时，拒绝原假设。



**I 类错误与 II 类错误**

I 类错误指拒绝原假设但是原假设为真的情况，用 $\alpha$ 表示犯错概率，一般控制在 $(0.01,0.1)$ 范围内。(也称<font color=red>显著水平</font>)

II 类错误指接受原假设但原假设为假的情况，用 $\beta$ 表示。



## 三、根据显著水平和统计量的分布确定临界值

根据 NP 原则，先保证犯 I 类错误的概率不超过 $\alpha$，再令犯 II 类错误的概率尽可能小。

例：取 $\alpha=0.05$ ，当 $H_0:\mu=0$ 成立时，$\frac{\bar{X}}{0.6/\sqrt{9}}\sim N(0,1)$。（统计量分布）则可进行如下计算：

{%cq%}
$
\begin{array}{l}
P\{\bar{X}\ge C|\mu=0 \}&=P\left\{\frac{\bar{X}}{\sigma/\sqrt{n}}\ge\frac{C}{\sigma/\sqrt{n}}|\mu=0 \right\} \\
&=1-\psi\left(\frac{C}{\sigma/\sqrt{n}}\right)\le\alpha=0.05\ \ (0.05=\psi(-\textbf{z}_{0.05})) \\
&\Rightarrow \frac{C}{0.6/\sqrt{9}}\ge \textbf{z}_{0.05}=1.645 \Rightarrow C\ge0.329
\end{array}
$
{%endcq%}

因此取 $C=0.329$ 以减小 II 类错误！



## 四、根据样本数据判断是否要拒绝假设

> 例：如 $\bar{x}=0.522\gt 0.329$，则拒绝原假设。



## 正态总体参数的假设检验样例

In development...

