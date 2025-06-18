---
title: MA113 线性代数
date: 2024-08-25 00:11:24
tags:
  - Maths
categories: 2022 Fall
mathjax: true
description: >-
  仅作为其他课程复习用，不记录全部知识点。
---

# About

由于篇仅用于某些计算机相关课程的复习，所以某些“不太重要”的知识点要么默认已知，要么不会提及（如转置的概念和二级结论，行列式的概念等）。

由于教材不同或版本原因，对于一些符号可能有差异（如表示单位方阵有 $I/E$，以下统一使用本人学习的教材所用符号）



# 第一章 矩阵（入门）

## 矩阵的逆

定义：$AC=CA=I\Longleftrightarrow$ $C$ 是 $A$ 的逆，记作 $A^{-1}$。

> 不是所有方阵都有逆，不可逆<font color=red>方阵</font>称为<font color=red>奇异矩阵</font>。



## 矩阵的 LDU 分解

定义：$\color{red}PA=LDU$，其中 $L$ 是下三角矩阵，$D$ 是对角矩阵，$U$ 是主对角元为 1 的上三角矩阵。($P$ 仅用作行交换)



# 第二章 向量空间

## 基的相关定理

**一、坐标：** 令 $A=\{v_1,v_2,\cdots,v_n\}$ 是 $\mathbb{V}^n$ 的一组基，$\forall w\in\mathbb{V}^n$ ，有唯一数组 $\{a_1,a_2,\cdots,a_n\}$ 使 $w=a_1v_1+a_2v_2+\cdots +a_nv_n$ ，则该数组是 $\mathbb{A}$（向量空间的一组基）中 $w$ 的坐标，记为 $[w]_A$ 。



**二、满秩：** 对于一个 $m*n$ 矩阵 $A$，若 $rank(A)=m$，则称 $A$ 行满秩，若 $rank(A)=n$，则称 $A$ 列满秩。

- 若 $A_{m\times n}$ 行满秩（可推出 $m\le n$），且前 $m$ 列线性独立，令 $A=\left[A_0|X\right]$ ，则右逆 $C=\left[\begin{array}{c}A_0^{-1} \\ 0\end{array}\right]$ ；
- 若 $A_{m\times n}$ 列满秩（可推出 $m\ge n$），且前 $n$ 行线性独立，令 $A=\left[\begin{array}{c}A_0 \\ Y\end{array}\right]$ ，则左逆 $B=\left[A_0^{-1}|\ 0\right]$ 。



**三、线性变换：**

- 求解变换矩阵的方法：
  - 1）自然基变换求解
  - 2）非自然基变换求解：利用矩阵逆运算



# 第三章 正交性

**正交补**

对于子空间 $V \subseteq \mathbb{R}^n$，所有与 $V$ 正交的向量集合组成的空间称为正交补，写作 $W=V^{\bot}$。

**投影**

向量 $b$ 在向量 $a$ 上的投影表示为：$proj_a:b\mapsto p=\hat xa$

{%cq%}
$
\begin{array}{l}
&0=a^T(b-\hat{x}a)\Rightarrow \hat{x}=\frac{a^Tb}{a^T a} \\
\therefore &proj_a(b)=\frac{aa^T}{a^Ta}\cdot b
\end{array}
$
{%endcq%}

所以，投影矩阵为 $P=\frac{aa^T}{\|a\|^2}$



## 最小二乘

若系统 $Ax=b$ 无解，则其最小二乘解（残差最小）为 $\hat{x}=(A^TA)^{-1}A^T b$，其投影 $p=A\hat{x}=A(A^TA)^{-1}A^T b$。



## 正交基（重点）

正交矩阵 $Q$ 满足 $Q^TQ=I$，即 $Q$ 的每一列（向量）组成了 $\mathbb{R}^n$ 的标准正交基。

## Gram-Schmidt 正交化

- 基本思想：从每一个新向量中扣除其在已知方向上的投影分量
- 步骤：
  1. 求投影（正交化）
  2. 求标准基（单位化）

{%cq%}
$
\begin{array}{l}
&A_j=a_j-(q_1^T a_j)q_1-(q_2^T a_j)q_2--\cdots -(q_{j-1}^T a_j)q_{j-1} \\
&q_j=\frac{A_j}{\|A_j\|}
\end{array}
$
{%endcq%}

## QR 分解（重点）

$A=QR$

{%cq%}
$
\begin{array}{l}
A= \left[\begin{array}{c} a&b&c \end{array}\right]=\left[\begin{array}{c} q_1 & q_2 & q_3 \end{array}\right]\left[\begin{array}{c} q_1^Ta & q_1^Tb & q_1^Tc \\ & q_2^Tb & q_2^Tc \\ && q_3^Tc \end{array}\right]=QR
\end{array}
$
{%endcq%}

> $Q$ 可通过 Gram-Schmidt 过程求得，$R$ 通过 $R=Q^TA$ 求得。



# 第四章 行列式

略。



# 第五章 特征值与特征向量

## 介绍

定义：设 $A$ 为 $n$ 阶矩阵。若存在一个非零向量 $x$ 和标量 $\lambda$ 满足 $Ax=\lambda x$ ，则称 $\lambda$ 为 $A$ 的一个特征值，$x$ 为 $\lambda$ 对应的特征向量。

定义：设 $A=[a_{ij}]_{n\times n}$ 为 $n$ 阶方阵：

{%cq%}
$
\begin{array}{l}
f(\lambda)=|A-\lambda I|=\left|\begin{array}{c} a_{11}-\lambda & a_{12} & \cdots & a_{1n} \\ a_{21} & a_{22}-\lambda & \cdots & a_{2n} \\ \vdots & \vdots & \ddots & \vdots \\ a_{n1} & a_{n2} & \cdots & a_{nn}-\lambda \end{array}\right|
\end{array}
$
{%endcq%}

为 $A$ 的特征多项式。


- 求特征值和特征向量的方法：
  - 计算 A 的特征多项式
  - 求出全部根（特征值）
  - 带入特征方程求出特征向量
- 特征值的性质：以矩阵 $A$ 的所有特征值为例
  - 特征值之积：$\prod_{i}\lambda_i=|A|$
  - 特征值之和：$\sum_{i}\lambda_i=trace(A)$，称为 $A$ 的迹



## 矩阵对角化

定义：$\exists S,\ S^{-1}AS=\Lambda\Rightarrow$ 矩阵 $A$ 可对角化（$\Lambda$ 是对角矩阵，对角元为 $A$ 的特征值）


概念：

- 代数重数：特征值 $\lambda_i$ 的最大重复数
- 几何重数：$\lambda_i$ 对应的特征子空间（即 $A-\lambda_i I$ 的零空间）的维数

定理：

- 当且仅当对于 $A$ 的每个特征值 $\lambda_i$，几何重数等于（一般是小于等于）代数重数时，矩阵 $A$ 可对角化
- 定义中 $S$ 的第 $j$ 列是 $A$ 的第 $j$ 个特征值所对应的特征向量
- 设 $A$，$B$ 为同阶可对角化矩阵，当且仅当 $AB=BA$ 时，它们有相同的特征向量。



## 复矩阵

概念：

- 共轭：$z=a+ib \to \bar{z}=a-ib$
- 极坐标：$\begin{array}{l} &a+ib=r(\cos{\theta}+i\sin{\theta})=re^{i\theta} \\ &a-ib=r(\cos{\theta}-i\sin{\theta})=re^{-i\theta} \end{array}$

极坐标简化运算：$(a+bi)\times (c+di)=re^{i\theta}\times Re^{i\alpha} \to$ 模长为 $rR$，角度为 $\theta +\alpha$



**厄米特矩阵 (Hermitian Matrix)**

定义1：对于一复数矩阵 $A$，其转置为 $\bar{A}^T$，称为 $A$ 的厄米特矩阵，记为 $A^H$ 。

定义2：若矩阵 $A$ 满足 $A^H=A$，则称 $A$ 是厄米特矩阵。



## 三种对角化分解

- <font color=red>可对角化矩阵</font>可以用<font color=blue>可逆矩阵</font>进行对角化：$A=S\Lambda S^{-1}$
- <font color=red>实对称矩阵</font>可以用<font color=blue>正交矩阵</font>进行对角化：$A=Q\Lambda Q^T$
- <font color=red>厄米特矩阵</font>可以用<font color=blue>酉矩阵</font>进行对角化：$A=U\Lambda U^H$
  - 酉矩阵是满足 $U^H=U^{-1}$ 的矩阵



## 相似矩阵

定义：若矩阵 $A$，$B$ 满足 $B=MAM^{-1}$（其中，$M$ 是可逆矩阵），则称 $A$ 与 $B$ 相似，记为 $A\sim B$。

定理：

1. $A$ 和 $B$ 有相同的特征值。
2. 当且仅当 $M^{-1}v$ 是 $B$ 的一个特征向量时，$v$ 是 $A$ 的特征向量。
3. $A$ 和 $B$ 有相同的行列式、秩和迹。



**相似变换**

定理：同一个线性变换在两组基下的表示矩阵 $A$ 和 $B$ 是相似的

证明：令 $V=\mathbb{R}^2$ ，$T$ 为 $V$ 的一个线性变换；又设另一组基 $\{w_1,w_2\}$，证明如下

{%cq%}
$
\begin{array}{l}
&T(v_1)=a_{11}v_1+a_{12}v_2\ \ \ \ T(v_2)=a_{21}v_1+a_{22}v_2 \\
\therefore &\left[\begin{array}{c}Tv_1&Tv_2\end{array}\right]=\left[\begin{array}{c}v_1&v_2\end{array}\right]\left[\begin{array}{c}a_{11}&a_{21} \\ a_{12}&a_{22}\end{array}\right]=\left[\begin{array}{c}v_1&v_2\end{array}\right]A \\
\because &\left\{\begin{array}{l}&w_1=m_{11}v_1+m_{12}v_2 \\ &w_2=m_{21}v_1+m_{22}v_2 \end{array}\right. \\
\therefore &\left[\begin{array}{c}w_1&w_2\end{array}\right]=\left[\begin{array}{c}v_1&v_2\end{array}\right]M \\
&\left[\begin{array}{c}Tw_1&Tw_2\end{array}\right]=\left[\begin{array}{c}Tv_1&Tv_2\end{array}\right]M \\
\text{also }\because &\left[\begin{array}{c}Tw_1&Tw_2\end{array}\right]=\left[\begin{array}{c}w_1&w_2\end{array}\right]B \\
\therefore &MB=AM \Rightarrow B=M^{-1}AM\text{(相似)}
\end{array}
$
{%endcq%}



## 舒尔定理

对一个 $n$ 阶方阵 $A$，总有一个酉矩阵 $U$，令 $U^{-1}AU=T$，$T$ 为与 $A$ 相似的三角矩阵，且对角线上的值是 $A$ 的特征值

推论：当 $A$ 为厄米特矩阵时，$T^H=(U^{-1}AU)^H=U^{-1}A^H U=T$。$T$ 是对角矩阵（证明了谱定律：$A=A^H \Rightarrow A$ 是可对角化的）



# 第六章 正定矩阵

## 二次型

{%cq%}
$
f(x,y)=ax^2+2bxy+cy^2
$
{%endcq%}

- 当且仅当 $ac\gt b^2$ 且 $a\gt 0(a\lt 0)$ 时，$f(x,y)$ 为正定（负定），存在唯一最小（最大）值【定点】
- 若 $ac=b^2$，则根据 $a$ 的值称为半正定/半负定，存在不唯一最小（最大）值
- 当 $ac\lt b^2$，称二次型 $f$ 不定，此时二次型所过定点非最大也非最小，称为<font color=red>鞍点</font>



二次型的另一种表示：($A$ 是实对称矩阵)

{%cq%}
$
ax^2+2bxy+cy^2=\left[\begin{array}{c}x&y\end{array}\right]\left[\begin{array}{c}a&b \\ b&c\end{array}\right]\left[\begin{array}{c}x \\ y\end{array}\right]=x^TAx
$
{%endcq%}

> 正定二次型所对应矩阵 $A$ 为正定矩阵 



## 主轴定理（重点）

定义：

设 $A_{n\times n}$ 为实对称矩阵则有一个变换 $x=Qy$（ 为正交矩阵），将二次型 $x^T Ax$ 转变为 $y^T\Lambda y$（不含交叉乘积项），通过这种变换，可以得到一个标准二次型 $y^T\Lambda y=\lambda_1y_1^2+\lambda_2y_2^2+\cdots +\lambda_n y_n^2$。







例：设 $A=\left[\begin{array}{c}5&-2 \\ -2&5\end{array}\right]$ ，即 $x^T Ax=5x_1^2-4x_1x_2+5x_2^2$ 。可求得 $A$ 的特征值是 $3$ 和 $7$，对应的特征向量为 $u_1=\left[\begin{array}{c}1/\sqrt{2} \\ 1/\sqrt{2}\end{array}\right]$ 和 $u_2=\left[\begin{array}{c}-1/\sqrt{2} \\ 1/\sqrt{2}\end{array}\right]$。

令 $Q=\left[\begin{array}{c}u_1&u_2\end{array}\right]=\left[\begin{array}{c}1/\sqrt{2}&-1/\sqrt{2} \\ 1/\sqrt{2}&1/\sqrt{2}\end{array}\right]$，由 $x=Qy$ 得：$y^T\Lambda y=3y_1^2+7y_2^2$。

> 用法：将实对称矩阵 $A$ 正交对角化：$A=Q\Lambda Q^T$。再通过 $y=Q^T x$ 得到 $y$。



## 合同变换法

思路：任意 n 阶实对称矩阵 A，都存在可逆矩阵 C，使得 $C^TAC=diag(d_1,d_2,\cdots,d_n)$。通过对矩阵 $A$ 的初等行/列变换，可以得到合同变换矩阵 $C$ 和变换后的矩阵 $D=C^TAC$

{%cq%}
$
\left[\begin{array}{c}C^T&0 \\ 0&I\end{array}\right]\left[\begin{array}{c}A \\ I\end{array}\right]C=\left[\begin{array}{c}C^TAC \\ C\end{array}\right]=\left[\begin{array}{c}D \\ C\end{array}\right]
$
{%endcq%}

由此可见，用正交变换法得到的标准型并不唯一（参考主轴定理）。标准型不唯一，但是标准型的正负号个数不变。



## 奇异值分解（重点）

思路：当矩阵非方阵时，不可进行特征值分解（正交对角化）。此时可以进行奇异值分解。



定义：可对任意矩阵进行奇异值分解（SVD）：

{%cq%}
$
A=U\Sigma V^T
$
{%endcq%}

其中，$\Sigma$ 为 $m\times n$ 对角阵（非方阵），其对角线处非零元素记为 $\sigma_1,\sigma_2.\cdots,\sigma_r$ ，这些元素称为 $A$ 的奇异值，也是 $AA^T$ 的特征值的平方根。

- 正交矩阵 $U$：$m$ 行 $n$ 列，每个向量是 $AA^T$ 的特征向量；
- 正交矩阵 $V$：$n$ 行 $n$ 列，每个向量是 $A^TA$ 的特征向量。



