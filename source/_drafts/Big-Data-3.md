---
title: MA234 大数据导论与实践（三）
date: 2024-08-29 16:50:10
tags:
  - Python
  - CSE Learning
categories:
  - 2024 Spring
mathjax: true
description: >-
  大数据导论与实践（二）的续集
---

# Big Data (III)

## VI. Ensemble Methods (集成方法)

- Two commonly used ensemble methods
	- **Bagging**
		- Random sampling : generating independent models, and averaging for regressions (making majority vote for classifications) [随机采样进行建模]
		- <font color=red>Reducing variances(方差)</font>
		- E.g. *Random Forest*
	- **Boosting**
		- Sequential training : training the subsequent models based on the errors of previous models <font color=grey>[复盘“错误”]</font>
		- <font color=red>Reducing bias(误差)</font>
		- E.g. AdaBoost and GBDT



### Bagging

**Algorithm**

- Input : training set $D = \{(x_1, y_1), ...,(x_N, y_N)\}$
- Output : additive model $\hat{f}_\text{bag} (x)$

1. For $m = 1$ to $M$ :
	1. Sample from $D$ with replacement to obtain $D_m$
	2. <font color=red>Train a model</font> $\hat f_m(x)$ from the dataset $D_m$ : for <font color=#4a4aff>classification</font>, $\hat f_m(x)$ returns a $K$-class 0-1 vector $e_k$ ; for <font color=#4a4aff>regression</font>, it is just a value
2. Compute **bagging estimate** $\hat{f}_\text{bag} (x)=\frac{1}{M} \sum_{m=1}^{M} \hat f_m(x) $
	1. for <font color=#4a4aff>classification</font>, make majority vote $\hat{G}_\text{bag}(x)=\arg\max_k \hat f_k(x)$
	2. for <font color=#4a4aff>regression</font>,  just return <font color=red>the average value</font>

**Variance Reduction**

- In bagging, we use the same model to train different sample set in each iteration ; assume the models $\{\hat f_m(x)\}_{m=1}^{M}$ have the <font color=red>same variance</font> $\sigma^2 (x)$, while the <font color=red>correlation</font> of each pair is $\rho(x)$ 
- Then the variance of the final model is :


$$
\begin{array}{l}
\text{Var}(\hat{f}_{bag}(x)) &= \frac{1}{M^2}\left(\sum_{m=1}^{M}\text{Var}(\hat{f}_m(x)) + \sum_{t\neq m}\text{Cov}(\hat{f}_t(x)\hat{f}_m(x))\right) \\ &= \rho(x)\sigma^2(x) + \frac{1-\rho(x)}{M}\sigma^2(x) 
\end{array}
$$


- As $M\to\infty$ , $\text{Var}(\hat f_{bag}(x))\to \rho(x)\sigma^2(x)$ . This usually <font color=red>reduces the variance</font>.
- If $\rho(x)=0$ , the variance approach zero.
- The <font color=red>random sampling</font> in bagging is to reduce the correlation $ρ(x)$, i.e., make the sub-predictors as independent as possible



<h4>Random Forest</h4>

- More randomness on **Decision Tree** : avoid local optimal
	- Sampling on the <font color=blue>training data</font> with replacement
	- Select <font color=blue>features</font> at random
- Example : RF consisting of $3$ independent trees, each with an error rate of $40\%$. Then the probability that more than one tree misclassify the samples is $0.4^3 + 3 * 0.4^2 * (1 − 0.4) = 0.352$
- **Algorithm**
    - Input : training set $D =\{(x_1,y_1),(x_2,y_2),...,(x_N,y_N)\}$
    - Output : additive model $\hat{f}_{\text{rf}}(x)$
    1. For $m = 1$ to $M$ :
		1. Sample from $D$ with replacement to obtain $D_m$
		2. Grow a random-forest tree $T_m$ to the dataset $D_m$ : by recursively repeating the following steps for each terminal node of the tree, until the minimum node size $n_{min}$ is reached
			- Select $q$ features at <font color=red>random</font> from the $p$ features
			- Pick the best feature/split-point among the $q$
			- Split the node into two daughter nodes
	2. Output the ensemble of trees $\{T_m\}_{m=1}^M$ : for <font color=blue>regression</font>, $\hat{f}_rf(x) = \frac{1}{M} \sum_{m=1}^M T_m(x)$; for <font color=blue>classification</font>, make majority vote
- <font color=red>Small value</font> of $q$ increases the <font color=red>independency</font> of trees;  empirically, $q = \log_2 p + 1$




<h4>Model Evaluation</h4>

- **Out-of-bag (OOB)** errors : The observation is called out-of-bag sample to some trees if it is <font color=red>not sampled</font> for those trees. Denote the training set in the m-th sampling by $D_m$. *OOB error* is computed as :
	1. For each observation $(x_i, y_i)$, find the trees which treat it as OOB sample : <br>$\{\hat T_m(\textbf{x}) : (\textbf{x}_i, y_i) \notin D_m \}$
	1. Use those trees to classify this observation and make majority vote as the label of this observation :<br>$\hat{f}_\text{oob}(\textbf{x}_i)=\arg\underset{y\in\mathcal Y}\max \sum_{m=1}^{M} I(\hat{f}_{m}(\textbf{x}_{i})=y)I(\textbf{x}_{i} \notin D_{m})$
	1. Compute the number of misclassified samples, and take the ratio of this number to the total number of samples as OOB error : <br>$Err_{oob}=\frac{1}{N} \sum_{m=1}^{M} I(\hat{f}_{oob}(\textbf{x}_i) \not = y_i)$

> Q: OOB 数据是否指所有生成的树都没有选择到的数据？
>
> Q: 第二步为什么不是 $I(\hat f_m(\textbf{x}_i)=y \wedge \textbf{x}_i\notin D_m)$ ?

- **Pros**
    - Bagging or random forest (RF) work for models with high variance but low bias (<font color=red>deal with overfitting</font>)
    - Better for <font color=red>nonlinear</font> estimators
    - RF works for very <font color=red>high-dimensional data</font>, and no need to do feature selection as RF gives the feature importance
    - Easy to do <font color=red>parallel computing</font>
- **Cons**
    - Overfitting when the samples are large-sized with <font color=blue>great noise</font>, or when the dimension of data is low
    - Slow computing performance comparing to single tree
    - <font color=red>Hard to interpret</font>

---



### Boosting & AdaBoost

> Principle : Combines the outputs of many <font color=blue>“weak” classifiers</font> to produce a powerful “committee”
>
> <font color=blue>Weak classifiers</font> : error rate $< 0.5$ (random guessing)

<table>
    <tr><td><img align="left" src="bd36.png" style="zoom:100%"></td>
        <td><font color=red>Sequentially</font> apply the weak classifiers to the repeatedly modified data, emphasizing the misclassified samples<br/><br/>
        Combine weak classifiers through a weighted majority vote or averaging to produce the final prediction</td></tr>
</table>



<font size=4>Boosting fits an additive model</font>

- Additive Model : $f(x)=\sum_{m=1}^{M}\beta_m b(x;\gamma_m)$  where $\gamma$ is the parameter of basic function, $\beta$ is the coefficient.
- Possible choices for <font color=blue>basis function</font> $b(x;\gamma_m)$
    - Neural Network : $\sigma(\gamma_0+\gamma_1^T x)$ , where $\sigma(t)=1/(1+e^{-t})$
    - Wavelets
    - Cubic Spline Basis
    - Trees
    - Eigenfunctions in reproducing kernel Hilbert space (RKHS)
- **Parameter fitting** : $\underset{ \{ \beta_{m},\gamma_{m}\} } \min \sum_{i=1}^{N} L(y_i,\sum_{m=1}^{M}\beta_{m} b(x_i;\gamma_{m}))$
- Loss function : <font color=red>squared error</font> $\color{red}L(y, f (x)) = (y − f (x))^2$ or likelihood-based loss



<b><font size=4>Forward <font color=red>Stagewise</font> Additive Model</font></b>



> Difference between “Forward Stepwise” and “Forward Stagewise”
>
> - Stepwise regression initialize model with all predictors(forward) or no predictors(backward), and then iteratively <font color=red>adds or removes</font> variables based on a defined criterion(e.g. AIC and BIC)
> - **Stagewise** regression initialize model with all predictors, and then in each iteration, it <font color=red>adjusts the coefficients</font> of the predictors by a small amount in the direction that improves the model's performance.
>
> **Stagewise** regression is designed to be more robust to multicollinearity and can produce more stable and interpretable models compared to stepwise regression. 
> Useful when there are many potential predictors, and the goal is to identify the most important ones while maintaining model stability.

**Algorithm**

- Input : training set $D =\{(x_1,y_1),(x_2,y_2),...,(x_N,y_N)\}$
- Output : additive model $f_M(x)$

1. Initialize $f_0(x)=0$
2. For $m=1$ to $M$ :
	2.1. Compute $(\beta_m,\gamma_m)=\underset{\beta ,\gamma}{\arg\min} \sum_{i=1}^{N} L(y_i,f_{m-1}(x_i)+\beta b(x_i;\gamma))$
	2.2. Update $f_m(x)=f_{m-1}(x)+\beta_m b(x_i;\gamma_m)$

Squared error loss in step 2.1:


$$
L(y_i,f_{m-1}(x_i)+\beta b(x_i;\gamma))=\underbrace{(y_i-f_{m-1}(x_i))}_{\text{residual}}-\beta b(x_i;\gamma)^2
$$


> What if we use Exponential loss in step 2.1 ?

- Exponential loss : $L(y,f(x))=\exp(-yf(x))$
- <font color=blue>Classifier</font> as basis function : $b(x; \gamma) = G(x) \in \{−1, 1\}$
- Let $w_i^{(m)}=\exp(-y_i f_{m-1}(x_i))$ , then step 2.1 turn to be :


$$
\begin{align}{l}
(\beta_m, G_m) &= \arg \min_{\beta, G} \sum_{i=1}^{n} w_i^{(m)} \exp(-\beta y_i G(x_i))\\
&=\arg \min_{\beta, G} \left[ \sum_{y_i \neq G(x_i)} w_i^{(m)} (e^{\beta} - e^{-\beta}) + e^{-\beta} \sum_{i=1}^{n} w_i^{(m)} \right]
\end {align}
$$


- We get $\beta_m$ and $G_m$ separately :


$$
\begin{align}{l}
G_m &= \arg \min_G \sum_{i=1}^{n} w_i^{(m)} I(y_i \neq G(x_i)) \\
\beta_m &= \arg \min_{\beta} \left[ \epsilon_m (e^{\beta} - e^{-\beta}) + e^{-\beta} \right] = \frac{1}{2} \log \frac{1-\epsilon_m}{\epsilon_m} \\
\epsilon_m &= \left(\left(\sum_{i=1}^{n} w_i^{(m)} I(y_i \neq G(x_i))\right) \left/\right. \sum_{i=1}^{n} w_i^{(m)}\right)
\end{align}
$$


where $\epsilon_m$ is weighted error rate.



<h4>AdaBoost Algorithm</h4>

<table><tr>
<td><img align="center" src="bd37.png" style="zoom:50%"></td>
<td><img align="center" src="bd38.png" style="zoom:90%"></td>
</tr></table>

<h5>Loss Functions</h5>

- For classification, exponential loss and binomial negative log-likelihood (deviance) loss $\log(1 + \exp(−2yf))$ share the same population minimizer ; thus it is equivalent to MLE rule
- For classification, squared error loss is not good (not monotonically decreasing) ; the exponential loss is good and binomial deviance is better (less penalty for large $−yf$)

---



### Gradient Boosting Decision Tree (GBDT)

<center><h4>Boosting Tree</h4></center>

- Using classification trees or regression trees as <font color=blue>base learners</font>
- $f_M(x) = \sum_{m=1}^{M} T(x; \Theta_m)$ where $T(x; \Theta) = \sum_{j=1}^{J} \gamma_j I(x \in R_j)$ <font color=grey>[树的表示方法：代表将输入空间划分为$J$个互不相交的区域$R_1,\cdots,R_J$，并在每个区域上确定输出的常量$\gamma_j$。所以$J$代表树的复杂度即叶节点个数 ]</font>
- Parameter set $\Theta = \{R_j, \gamma_j\}_{j=1}^{J}$ 
- Parameter finding : minimizing the empirical risk 


$$
\begin{array}{rl}
&\hat{\Theta} = \arg \min_{\Theta} \sum_{j=1}^{J} \sum_{x_i \in R_j} L(y_i, \gamma_j) \qquad &\text{Combinatorial optimization}
\end{array}
$$


- Approximate suboptimal solutions : 
	1. Finding $\gamma_j$ given $R_j$ : $\gamma_j = \bar{y}_j = \frac{1}{|R_j|} \sum_{y_i \in R_j} y_i$   for $L^2$ loss ; and  $\gamma_j =$ modal class in $R_j$   for misclassification loss 
    2. Finding $R_j$ given $\gamma_j$ : Difficult, need to estimate $\gamma_j$ as well ;<br>greedy, top-down recursive partitioning algorithm

<center><h4>Boosting Tree as Forward Stagewise Algorithm</h4></center>

- $\hat{\Theta}_m = \arg \min_{\Theta_m} \sum_{i=1}^{N} L(y_i, f_{m-1}(x_i) + T(x_i; \Theta_m))$
	1. $\hat{\gamma}_{jm} = \arg \min_{\gamma_{jm}} \sum_{x_i \in R_{jm}} L(y_i, f_{m-1}(x_i) + \gamma_{jm})$
    2. Finding $R_{jm}$ is more difficult than for a single tree in general.
- Squared-error loss : fit a tree to the residual<br> $L(y_i, f_{m-1}(x_i) + T(x_i; \Theta_m)) = (y_i - f_{m-1}(x_i) - T(x_i; \Theta_m))^2$
- Two-class classification and exponential loss : AdaBoost for trees, 
    - $\hat{\Theta}_m = \arg \min_{\Theta_m} \sum_{i=1}^{N} w_i^{(m)} \exp[-y_i T(x_i; \Theta_m)]$


$$
\hat{\gamma}_{jm} = \log \Large\frac{\sum_{x_i \in R_{jm}} w_i^{(m)} l(y_i = 1)}{\sum_{x_i \in R_{jm}} w_i^{(m)} l(y_i = -1)}
$$


- Absolute error or the Huber loss : robust but slow

<center><h4>Gradient Descent for General Loss</h4></center>

- Supervised learning is equivalent to the optimization problem


$$
\min_{f}L(f)=\min_{f}\sum_{i=1}^{N}L(y_i,f(x_i))
$$


- Numberical optimization : $\hat{\textbf{f}}=\arg\min_{\textbf{f}}L(\textbf{f})$  where  $\textbf{f}=\{f(x_1),f(x_2),\cdots,f(x_N)\}$ 
- Appriximate $\hat{\textbf{f}}$ by $\textbf{f}_M=\sum_{m=0}^{M} \textbf{h}_m$ , where $\textbf{f}_0=\textbf{h}_0$ is <font color=red>the initial guess</font>.
- Gradient Descent method : $\textbf{f}_m=\textbf{f}_{m-1}-\rho_m \textbf{g}_m$  , where $g_{im}=\left[\frac{\partial L(y_i,f(x_i))}{\partial f(x_i)} \right]_{f(x_i)=f_{m-1}(x_i)}$  , and $\textbf{h}_m=-\rho_m\textbf{g}_m$ .
- Here $\color{red}\rho_m$ is the learning rate, and $\color{red}\textbf{g}_m$ is the <font color=red>gradient of the target function</font> $\color{red}L(f)$ at the point $f(x_i)$ . So $\rho_m$ decides the <font color=blue>step length</font> of the gradient.

> Usage of Gradient Descent on **Decision Tree**
>
> - Find a Tree $T(x;\Theta_m)$ by minimization problem :


$$
\tilde{\Theta}_m=\arg\min_{\Theta_m}\sum_{i=1}^{N}(-g_{im}-T(x_i;\Theta_m))^2
$$


> In general, $\tilde{R}_{jm}\not=R_{jm}$ 

|    Setting     |              Loss Function              |          $-\partial L(y_i,f(x_i))/\partial f(x_i)$           |
| :------------: | :-------------------------------------: | :----------------------------------------------------------: |
|   Regression   | $\frac{1}{2}\left[y_i-f(x_i) \right]^2$ |                         $y_i-f(x_i)$                         |
|   Regression   |           $\lvert y_i-f(x_i)\rvert$            |            $\text{sign}\left[y_i-f(x_i) \right]$             |
|   Regression   |                  Huber                  | $y_i-f(x_i)$  for $\lvert y_i-f(x_i)\rvert \le \delta_m$<br>$\delta_m\text{sign}\left[y_i-f(x_i) \right]$  for $\lvert y_i-f(x_i)\rvert \gt \delta_m$ <br>where $\delta_m=\alpha^{\text{th}}$-quantile $\{\lvert y_i-f(x_i)\rvert \}$ |
| Classification |                Deviance                 |      $k^{th}$ component: $I(y_i=\mathcal G_k)-p_k(x_i)$      |



<center><h4>GBDT Algorithm</h4></center>

- Input : training set $D = \{(x_1, y_1), \ldots, (x_N, y_N)\}$, loss function $L(y, f(x))$
- Output : boosting tree $\hat{f}(x)$

1. Initialize $f_0(x) = \arg\min_\gamma \sum_{i=1}^{N} L(y_i, \gamma)$
2. For $m = 1$ to $M$ : 
    1. For $i = 1, 2, \ldots, N$ compute $r_{im} = \bigg[\frac{\partial L(y_i,f(x_i))}{\partial f(x_i)}\bigg]_{f=f_{m-1}}$
    2. Fit a regression tree to the target residual $r_{im}$, giving terminal regions $R_{jm}$ (表示第 j 个样本在第 m 个基模型上的残差) , $j = 1, \ldots, J_m$ 
    3. For $j = 1, \ldots, J_m$, compute $\gamma_{jm} = \arg\min_\gamma \sum_{x_i \in R_{jm}} L(y_i, f_{m-1}(x_i) + \gamma)$
    4. Update $f_m(x) = f_{m-1}(x) + \sum_{j=1}^{J_m} \gamma_{jm}I(x_i \in R_{jm})$
3. $\hat{f}(x) = f_M(x)$



**Regularization**

- <font color=red>Shrinkage</font> : the step 2.4 is modified as $f_m(x) = f_{m-1}(x) + \nu \sum_{j=1}^{J_m} \gamma_{jm}I(x_i \in R_{jm})$

- <font color=red>Subsampling</font> : at each iteration, sample a fraction $\eta$ of the training set and grow the next tree using the subsample

- Shrinkage + subsampling : best performance



**Feature importance and Partial Dependence Plots**

- Feature importance
    - When fitting a single tree $T$, at each node $t$, one feature $X_{v(t)}$ and one separate value $X_{v(t)} = c_{v(t)}$ are chosen to improve a certain quantity of criterion (e.g. GINI, entropy, squared error, etc.)
    - Sum all these improvements $i_t$ brought by each feature $X_k$ over all internal nodes: $I_k(T) = \sum_{t=1}^{J-1} i_t I(v(t) = k)$
    - Average the improvements of all trees $\Rightarrow$ importance of that feature: $I_k=\frac{1}{M} \sum_{m=1}^{M} I_k(T_m)$
- Partial Dependence Plots
    - Partial dependence of $f(X)$ on $X_S$ : $f_S(X_S) = E_{X_C}f(X_S, X_C)$
    - Estimate by empirical mean : $\hat{f}_S(X_S) = \frac{1}{N} \sum_{i=1}^{N} f(X_S, X_{iC})$

---



## VII. Clustering

- Different from classification : it is <font color=red>unsupervised learning</font> ; no outputs or labels
- Central goal : Optimize the similarity (or dissimilarity) between the individual objects being clustered :
	- Obtain <font color=blue>great similarity</font> of samples <font color=blue>within</font> cluster
	- Obtain <font color=#c4c400>small similarity</font> of samples <font color=#c4c400>between</font> clusters
- Cost functions : not related to the outputs, but related to the similarity
- Two kinds of input data :
	- $n × n$ similarity (dissimilarity) matrix $D$ : only depends on the distances between pairs of samples ; may lose some information on data
	- Original data with features $X \in R^{n×d}$



### K-Mean Clustering

<center><font size=5>Idea</font></center>

- Data set $\{x_i\}_{i=1}^n$, $x_i \in \mathbb{R}^d$
- Representatives : Mass center of $k$th-cluster $C_k$ is $c_k$, $k = 1, \ldots, K$
- Sample $x_i$ belongs to cluster $k$ if $d(x_i, c_k) < d(x_i, c_m)$ for $m \neq k$, where $d(x_i, x_j)$ is dissimilarity function
- Make the mass centers well-located so that the average distance between each sample to its cluster center is as small as possible

<center><img src="bd39.png" style="zoom:90%"></center>



<center><font size=5>Optimization Problem</font></center>

- Let $C : \{1, \ldots, n\} \rightarrow \{1, \ldots, k\}$ be the assignment from the data indices to the cluster indices. $C(i) = k$ means $x_i \in C_k$
- Total point scatter : 
    - $T = \frac{1}{2} \sum_{i=1}^{n} \sum_{j=1}^{n} d(x_i, x_j) =\frac{1}{2} \sum_{k=1}^{K} \sum_{C(i)=k}\left( \sum_{C(j)=k} d_{ij} + \sum_{C(j)\neq k} d_{ij}\right) = W(C) + B(C)$
- Loss function ($d$ is the distance) :
    - within-cluster point scatter $W(C) = \frac{1}{2} \sum_{k=1}^{K} \sum_{C(i)=k} \sum_{C(j)=k} d_{ij}$ ; 
    - between-cluster point scatter $B(C) = \frac{1}{2} \sum_{k=1}^{K} \sum_{C(i)=k} \sum_{C(j)\neq k} d_{ij}$
- Minimize $W(C)$ is equivalent to maximize $B(C)$



#### Hierarchical Clustering

- Clustering in different hierarchies, generating tree structure 
- Two approaches : 
    - <font color=blue>Agglomerate clustering : bottom-up</font>
    - <font color=blue>Divisive clustering : top-down</font> 
- Limitation : once merged or divided, the operation cannot be modified

<h5>Agglomerate Clustering</h5>

- Given n samples and proximity matrix, do the following steps : 
    1. Let every observation represent a singleton cluster 
    2. Merge the two closest clusters into one single cluster 
    3. Calculate the new proximity matrix (dissimilarity between two clusters) 
    4. Repeat step 2 and 3, until all samples are merged into one cluster 

- Three methods for computing intergroup dissimilarity : 
    - Single linkage (SL) 
    - Complete linkage (CL) 
    - Average linkage (AL)

<h5>Generalized Agglomerative Scheme</h5>

()



### DBSCAN

<center><font size=5>Concept</font></center>

- Three type of points :
    - **Core point** : # of samples in its $\epsilon$-neighborhood $> \text{MinPts}$
    - **Boundary point** : it lies in the $\epsilon$-neighborhood of some core point, # of samples in its $\epsilon$-neighborhood $< \text{MinPts}$ 
    - **Noise point** : neither core point nor boundary point, it lies in the sparse region

<center><img src="bd40.png" style="zoom:70%"></center>





### Model Assessment

<center><font size=5>Purity</font></center>

**Def.** Total purity defined as 


$$
\text{Purity}\triangleq \sum_i \frac{n_i}{n}p_i=\sum_i\frac{n_i}{n}(\max_j p_{ij})
$$


E.g. $\text{purity}=\frac{6}{17}\cdot \frac{4}{6}+\frac{6}{17}\cdot\frac{5}{6}+\frac{5}{17}\cdot\frac{3}{5}=0.71$ 

```
|-------|-------|--------|
| A B B | A A A |  A  A  |
|       |       |        |
| B B C | A A B | C C C  |
|-------|-------|--------|
```

<center><font size=5>Confusion Matrix</font></center>

<div>
<img src="bd41.png" style="zoom:70%">
</div>
