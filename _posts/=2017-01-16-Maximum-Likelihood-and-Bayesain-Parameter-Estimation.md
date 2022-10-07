---
type: article
title: Maximum Likelihood and Bayesain Parameter Estimation
key: 2018011601
tags: MachineLearning ToBeCompleted
comment: false
mathjax: true
---

概率密度估计的方法：

- **参数法(见最大似然和贝叶斯估计)**：假定概率密度函数形式，$p(\mathbf{x} \mid \omega_i) = p(\mathbf{x} \mid \theta_i)$

  -Distribution: Gaussian, Gamma, Bernouli

  -Estimation: Maximun-likelyhood, Bayesian Estimation

- 非参数法：可以表示任意概率分布

  -Parzen Window

  -K-NN

- Semi-Parametric

  -Distribution: Gaussian mexture

  -Estimation: Expectation-Maximization(EM)

### [](#header-1)一、参数估计

根据贝叶斯决策理论知道了如何根据先验概率和类条件概率密度来设计最优分类器，实际问题中通常得不到有关问题的结构的全部知识，可以利用样本来估计问题中涉及的先验概率和类条件概率，然后再设计分类器。

训练样本少，特征维数大时估计类条件概率密度难度较大。

**假设类条件概率密度是一个已知形式的分布**，将类条件概率密度参数化，这样就把问题从估计完全未知的类条件概率密度转化为了估计参数

最大似然估计与贝叶斯参数估计有本质区别：

最大似然估计把待估计的参数看作是确定的量，只是其取值未知，最佳估计就是使得产生已观测到的样本（即训练样本）的概率为最大的那个值；而贝叶斯估计则把待估计的参数看作是复合某种先验概率分布的随机变量，对样本进行观测的过程，就是把先验概率密度转化为后验概率密度，利用样本的信息修正了对参数的初始估计值

### [](#header-2)二、最大似然估计

**1. 原理和推导**

最大似然估计再训练样本增多时通常收敛的比较好，而且相比其它参数估计的方法更适合实际使用

根据各类别的参数向量$\pmb \theta_i$对它所属的类别$\mathcal{D}_i$所起的作用都是相互独立的假设（即属于类别$\mathcal{D}_i$的训练样本对参数向量$\pmb \theta_j(i \neq j)$的估计不提供任何信息），那么简化后的问题表述如下：

已知样本集$\mathcal{D}$，其中每一个样本都是独立的根据已知形式的概率密度函数$p(\mathbf{x} \mid \pmb \theta)$抽取得到的，使用这些样本估计概率密度函数中的参数向量

由于样本是独立抽取的，因此：

$$\displaystyle p(\mathcal{D} | \pmb \theta) = \prod \limits_{k=1}^n p(\mathbf{x}_k|\pmb \theta)$$

$p(\mathcal{D} \mid \pmb \theta)$看成是参数向量$\pmb \theta$的函数，称作关于样本集$\mathcal{D}$的似然函数，最大似然估计即是估计使得$p(\mathcal{D} \mid \pmb \theta)$达到最大的那个参数向量$\hat{\pmb\theta}$

$\nabla_\theta p(\mathcal{D} \mid \pmb \theta)=0$的解可能有解析解，也可能需要迭代求解（梯度下降）

一般使用对数似然函数，容易计算：

$$l(\pmb \theta) = p(\mathcal{D} \mid \pmb \theta)$$

$$\displaystyle l(\pmb \theta )=\sum \limits_{k=1}^n \ln p(\mathbf{x}_k|\pmb \theta)$$

$$\displaystyle \nabla_{\pmb\theta} l =\sum \limits_{k=1}^n \nabla_{\pmb\theta} \ln p(\mathbf{x}_k|\pmb \theta)$$

最大似然估计等价于参数分布为均匀分布的最大后验概率估计：

$$\underset{\pmb \theta}{\max} l(\pmb \theta)p(\pmb \theta)$$

$$\displaystyle p(\pmb\theta|\mathcal{D})= \frac{p(\mathcal{D}|\pmb\theta)p(\pmb\theta)}{p(\mathcal{D})}$$

**2. 高斯情况：** $\pmb \mu$未知，$\pmb \Sigma$已知

样本服从多元高斯分布：

$$\displaystyle p(\mathbf{x}_k|\pmb\mu)=\frac{1}{(2\pi)^{d/2} |\pmb\Sigma|^{1/2}} \exp \left[-\frac{1}{2}(\mathbf{x}_k-\pmb\mu)^t\pmb\Sigma^{-1}(\mathbf{x}_k-\pmb\mu)\right]$$

$$\displaystyle \ln p(\mathbf{x}_k|\pmb\mu)=-\frac{1}{2} \ln [(2\pi)^d|\pmb\Sigma|]-\frac{1}{2}(\mathbf{x}_k-\pmb\mu)^t\pmb\Sigma^{-1}(\mathbf{x}_k-\pmb\mu)$$

先将式子展开，然后应用矩阵求导，而且${\pmb\Sigma^{-1}}^t = \pmb\Sigma^{-1}$：

$$\displaystyle \nabla_{\pmb\mu} \ln p(\mathbf{x}_k|\pmb\mu) =  \pmb\Sigma^{-1}(\mathbf{x}_k-\pmb\mu)$$

$$\displaystyle \nabla_{\pmb\theta} l =\sum \limits_{k=1}^n \nabla_{\pmb\theta} \ln p(\mathbf{x}_k|\pmb \theta)=\sum \limits_{k=1}^n \pmb\Sigma^{-1}(\mathbf{x}_k-\pmb\mu) = 0$$

方程两边同时乘上$\pmb\Sigma$：

$$\sum \limits_{k=1}^n (\mathbf{x}_k-\pmb\mu) = 0$$

$$\displaystyle \hat{\pmb\mu} = \frac{1}{n}\sum \limits_{k=1}^n \mathbf{x}_k$$

对均值的最大似然估计就是对全体样本取平均，均值的最大似然估计等于样本均值

**3. 高斯情况：** $\pmb \mu$未知，$\pmb \Sigma$未知

$\pmb \mu$和$\pmb \Sigma$均未知的情况是实际应用中更典型的情况

**case 1: 单变量高斯函数**

$$\displaystyle p(x_k|\mu,\sigma^2)=\frac{1}{\sqrt{2\pi}\sigma}\exp \left[-\frac{1}{2}\left(\frac{x_k-\mu}{\sigma}\right)^2\right]$$

参数向量$\pmb\theta$，$\pmb\theta_1=\mu, \pmb\theta_2=\sigma^2$

$$\displaystyle p(x_k|\pmb\theta)=\frac{1}{\sqrt{2\pi}\pmb\theta_2^{1/2}}\exp \left[-\frac{1}{2}\left(\frac{x_k-\pmb\theta_1}{\pmb\theta_2^{1/2}}\right)^2\right]$$

对数似然函数为：

$$\displaystyle \ln p(x_k|\pmb\theta)=-\frac{1}{2}\ln 2\pi \pmb\theta_2-\frac{1}{2\pmb\theta_2}(x_k-\pmb\theta_1)^2$$

对$\pmb\theta$求导：

$$\displaystyle \nabla_{\pmb\theta}l=\nabla_{\pmb\theta}\ln p(x_k|\pmb\theta)=\left[\begin{array}{c}\displaystyle \frac{1}{\pmb\theta_2}(x_k-\pmb\theta_1^2)\\\displaystyle -\frac{1}{2\pmb\theta_2}+\frac{(x_k-\pmb\theta_1)^2}{2\pmb\theta_2^2}\end{array}\right]$$

对数似然函数的极值条件：

$$\displaystyle \sum \limits_{k=1}^n\frac{1}{\hat{\pmb\theta_2} }(x_k-\hat{\pmb\theta_1^2})=0$$

$$\displaystyle \sum \limits_{k=1}^n -\frac{1}{2\hat{\pmb\theta_2} }+\frac{(x_k-\hat{\pmb\theta_1})^2}{2\hat{\pmb\theta_2^2} } = 0$$

最大似然估计结果：

$$\displaystyle \hat{\mu} = \hat{\pmb\theta_1}=\frac{1}{n}\sum \limits_{k=1}^n x_k$$

$$\displaystyle \hat{\sigma^2} = \hat{\pmb\theta_2} = \frac{1}{n}\sum \limits_{k=1}^n (x_k-\hat{\mu})^2$$

**case 2: 多元高斯函数**

$$\displaystyle p(\mathbf{x} | \pmb\mu, \pmb\Sigma )=\frac{1}{(2\pi)^{d/2}|\pmb\Sigma|^{1/2}}\exp\left[-\frac{1}{2}(\mathbf{x}-\pmb\mu)^t\pmb\Sigma^{-1}(\mathbf{x}-\pmb\mu)\right]$$

![](https://ws3.sinaimg.cn/large/006tNc79ly1fnrir8ohrfj31120jcad4.jpg)

![](https://ws4.sinaimg.cn/large/006tNc79ly1fnriri5m4cj311613w0z4.jpg)

### [](#header-3)二、贝叶斯参数估计
