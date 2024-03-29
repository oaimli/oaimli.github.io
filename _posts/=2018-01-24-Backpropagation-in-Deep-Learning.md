---
type: article
title: Backpropagation in Deep Learning
key: 2018012301
tags: MachineLearning
comment: false
mathjax: true
---

误差反向传播算法是训练神经网络最常用且最基础的算法

这里以三层神经网络为基础，结合不同的准则函数和激活（转移）函数对误差反向传播算法做出详细的计算

### [](#header-1)一、三层感知器

![](https://ws1.sinaimg.cn/large/006tNc79ly1fnqd1z2938j30hq0b1whk.jpg)

**网络描述：**

训练数据输入输出对：$\{ x_i^k,t_j^k\}$，$k$表示第$k$个样本，$k=1,2,\cdots,n$

输出层节点的输出：$z_j^k$

隐含层节点的输出：$y_h^k$

输入信号：$x_i^k$

输入层端点数：$d+1$

输入层节点$i$至隐含层$h$的权重：$w_{ih}$

隐含层节点$h$至输出层$j$的权重：$w_{hj}$

目标：使$z_1\approx t_1,z_2\approx t_2,\cdots,z_c\approx t_z$

误差：$E(\mathbf{w})=\sum \limits_{k}J(\mathbf{w})^k$，$J(\mathbf{w})^k$表示单个样本的误差

**每个样本所经历的计算：**

对第$k$个样本，隐含层$h$节点的输入加权和为：

$$\displaystyle net_h^k=\sum \limits_i w_{ih}x_i^k$$

经过隐含层的激励($f_1$)，隐含层节点$h$的输出为：

$$\displaystyle y_h^k=f_1(net_h^k)=f_1\left(\sum \limits_i w_{ih}x_i^k\right)$$

输出层$j$节点的输入加权和为：

$$\displaystyle net_j^k=\sum \limits_h w_{hj}y_h^k=\sum \limits_h w_{hj}f_1(net_h^k)=\sum \limits_h w_{hj}f_1\left(\sum \limits_i w_{ih}x_i^k\right)$$

经过输出层的激励($f_2$)，输出层节点$j$的输出为：

$$\displaystyle z_j^k = f_2(net_j^k)=f_2\left(\sum \limits_h w_{hj}f_1\left(\sum \limits_i w_{ih}x_i^k\right)\right)$$

### [](#header-2)二、依赖复合函数链式求导计算

更新权重采用的是训练神经网络的$\delta$规则(梯度下降):

$$w_{ij} =w_{ij} + \Delta w_{ij}$$

$$\displaystyle \Delta w_{ij} = -\eta \frac{\partial E(\mathbf{w})}{\partial w_{ij}}$$

隐含层至输出层：（激励函数为SoftMax时推导略有不同）

$$\begin{align*} \Delta w_{hj}  &= -\eta \frac{\partial E(\mathbf{w})}{\partial w_{hj}} \\&=-\eta \sum \limits_k\frac{\partial E(\mathbf{w})}{\partial z_{j}^k} \frac{\partial z_j^k}{\partial net_j^k} \frac{\partial net_j^k}{\partial w_{hj}}\\&=-\eta \sum \limits_k f_2'(net_j^k)\frac{\partial E(\mathbf{w})}{\partial z_{j}^k} y_h^k\end{align*}$$

输入层至隐含层：

$$\begin{align*} \Delta w_{ih}  &= -\eta \frac{\partial E(\mathbf{w})}{\partial w_{ih}} \\&=-\eta \sum \limits_{k,j}\frac{\partial E(\mathbf{w})}{\partial z_{j}^k} \frac{\partial z_j^k}{\partial net_j^k} \frac{\partial net_j^k}{\partial y_{h}^k} \frac{\partial y_{h}^k}{\partial net_{h}^k} \frac{\partial net_{h}^k}{\partial w_{ih}}\\&=-\eta \sum \limits_{k,j}\frac{\partial E(\mathbf{w})}{\partial z_{j}^k} f_2'(net_j^k) w_{hj} f_1'(net_h^k) x_i^k\\&=-\eta \sum \limits_{k} \left \lbrace f_1'(net_h^k) \left \lbrace\sum \limits_j \frac{\partial E(\mathbf{w})}{\partial z_{j}^k} f_2'(net_j^k) w_{hj} \right \rbrace x_i^k \right\rbrace\end{align*}$$

### [](#header-3)三、准则函数

**1. MSE 误差平方和损失（最常用）**

$$\displaystyle E(\mathbf{w})=\sum \limits_{k}J(\mathbf{w})^k=\frac{1}{2}\sum \limits_{k,j} (t_j^k - z_j^k)^2 $$

$$\displaystyle \frac{\partial E(\mathbf{w})}{\partial z_j^k} = -(t_j^k-z_j^k)$$

**2. 交叉熵损失**

$$\displaystyle E_{ce}(\mathbf{w})=\sum \limits_{k}J(\mathbf{w})^k=\sum \limits_{k,j} t_j^k \ln (t_j^k/z_j^k) $$

$$\displaystyle \frac{\partial E_{ce}(\mathbf{w})}{\partial z_j^k} = -t_j^k/z_j^k$$

**3. HingeLoss损失**

$$\displaystyle E_{hinge}(\mathbf{w})=\sum \limits_{k}J(\mathbf{w})^k=\sum \limits_{k,j} \max \lbrace 0,1-t_j^k z_j^k \rbrace$$

**4. Minkowski损失**

$$\displaystyle E_{mink}(\mathbf(w))=\sum \limits_{k}J(\mathbf{w})^k=\sum \limits_{k,j}|t_j^k-z_j^k|^R,1\leq R<2$$

### [](#header-4)四、激励函数

**1. Sigmoid函数**

$$\displaystyle f(s) = \frac{1}{1+e^{-s}}$$

$$\displaystyle f'(s) = f(s)\left(1-f(s)\right) $$

**2. SoftMax**

$$\displaystyle f(net_j^k) = \frac{e^{net_j^k}}{\sum \limits_m e^{net_m^k}}$$

$$\displaystyle f'_2(net_j^k) = \frac{\partial z_m^k}{\partial net_j^k} = \begin{cases}z_j^k(1-z_j^k) & m=j \\-z_m^kz_j^k & m \neq j\end{cases}$$

**3. 双曲正切函数**

$$\displaystyle f(s) = \frac{e^s+e^{-s}}{e^s+e^{-s}}$$

$$f'(s) = 1-f^2(s)$$

### [](#header-5)五、Case 1: MSE+Sigmoid+Sigmoid

> 准则函数：MSE，隐含层激励函数：Sigmoid，输出层激励函数：Sigmoid 

**MSE:**

$$\displaystyle E(\mathbf{w})=\sum \limits_{k}J(\mathbf{w})^k=\frac{1}{2}\sum \limits_{k,j} (t_j^k - z_j^k)^2 $$

$$\displaystyle \frac{\partial E(\mathbf{w})}{\partial z_j^k} = -(t_j^k-z_j^k)$$

**Sigmoid:**

$$\displaystyle f_1(net_h^k)=\frac{1}{1+e^{-net_h^k}}$$

$$\displaystyle f_1'(net_h^k) = f_1(net_h^k)\left(1-f_1(net_h^k)\right) =y_h^k(1-y_h^k)$$

**Sigmoid:**

$$\displaystyle f_2(net_j^k)=\frac{1}{1+e^{-net_j^k}}$$

$$\displaystyle f_2'(net_j^k) = f_2(net_j^k)\left(1-f_2(net_j^k)\right) = z_j^k(1-z_j^k)$$

**隐含层至输出层：**

$$\begin{align*} \Delta w_{hj}  &=-\eta \sum \limits_k f_2'(net_j^k)\frac{\partial E(\mathbf{w})}{\partial z_{j}^k} y_h^k\\&=\eta \sum \limits_k  f_2'(net_j^k) (t_j^k-z_j^k)y_h^k \end{align*}$$

![](https://ws2.sinaimg.cn/large/006tNc79ly1fnqnkoac4qj30iw0e2gpm.jpg)

![](https://ws4.sinaimg.cn/large/006tNc79ly1fnqnqqcxb3j30iw0c9dkt.jpg)

**输入层至隐含层：**

$$\begin{align*} \Delta w_{ih}  &=-\eta \sum \limits_{k} \left \lbrace f_1'(net_h^k) \left \lbrace\sum \limits_j \frac{\partial E(\mathbf{w})}{\partial z_{j}^k} f_2'(net_j^k) w_{hj} \right \rbrace x_i^k \right\rbrace \\&=\eta \sum \limits_{k} \left \lbrace f_1'(net_h^k) \left \lbrace\sum \limits_j (t_i^k-z_j^k) f_2'(net_j^k) w_{hj} \right \rbrace x_i^k \right\rbrace\end{align*}$$

![](https://ws2.sinaimg.cn/large/006tNc79ly1fnqq7nvrgej30iw0azgov.jpg)

![](https://ws4.sinaimg.cn/large/006tNc79ly1fnqq80snjwj30iw0bcjuo.jpg)

![](https://ws2.sinaimg.cn/large/006tNc79ly1fnqq8770j6j30iw0ch42q.jpg)

### [](#header-6)六、Case 2: MSE+Sigmoid+SoftMax

> 准则函数：MSE，隐含层激励函数：Sigmoid，输出层激励函数：SoftMax

**MSE:**

$$\displaystyle E(\mathbf{w})=\sum \limits_{k}J(\mathbf{w})^k=\frac{1}{2}\sum \limits_{k,j} (t_j^k - z_j^k)^2 $$

$$\displaystyle \frac{\partial E(\mathbf{w})}{\partial z_j^k} = -(t_j^k-z_j^k)$$

**Sigmoid:**

$$\displaystyle f_1(net_h^k)=\frac{1}{1+e^{-net_h^k}}$$

$$\displaystyle f_1'(net_h^k) = f_1(net_h^k)\left(1-f_1(net_h^k)\right) =y_h^k(1-y_h^k)$$

**SoftMax**

$$\displaystyle f_2(net_j^k) = \frac{e^{net_j^k}}{\sum \limits_i e^{net_i^k}}$$

$$\displaystyle f'_2(net_j^k) = \frac{\partial z_m^k}{\partial net_j^k} = \begin{cases}z_j^k(1-z_j^k) & m=j \\-z_m^kz_j^k & m \neq j\end{cases}$$

**隐含层至输出层：**

> 这里切记不能直接带第二节中的公式，因为SoftMax激活函数需要分情况讨论

$$\begin{align*} \Delta w_{hj}  &= -\eta \frac{\partial E(\mathbf{w})}{\partial w_{hj}} \\&=-\eta \sum \limits_k  \frac{\partial E(\mathbf{w})}{\partial net_j^k} \frac{\partial net_j^k}{\partial w_{hj}}\\&=-\eta \sum \limits_k y_h^k \sum \limits_{m=1}^c \frac{\partial E(\mathbf{w})}{\partial z_{m}^k} \frac{\partial z_m^k}{\partial net_j^k} \\&=\eta \sum \limits_k y_h^k \sum \limits_{m=1}^c (t_m^k-z_m^k) \frac{\partial z_m^k}{\partial net_j^k} \\&=\eta \sum \limits_k y_h^k \left \lbrace\sum \limits_{m\neq j}^c (t_m^k-z_m^k) (-z_j^kz_m^k)+(t_j^k-z_j^k)z_j^k(1-z_j^k)\right \rbrace\\&=\eta \sum \limits_k y_h^k \left \lbrace\sum \limits_{m = 1}^c (t_m^k-z_m^k) (-z_j^kz_m^k)+(t_j^k-z_j^k)z_j^k\right \rbrace\end{align*}$$

**输入层至隐含层：**

> 这里切记不能直接带第二节中的公式，因为SoftMax激活函数需要分情况讨论

$$\begin{align*} \Delta w_{ih}  &= -\eta \frac{\partial E(\mathbf{w})}{\partial w_{ih}} \\&=-\eta \sum \limits_{k}\frac{\partial E(\mathbf{w})}{\partial y_{h}^k} \frac{\partial y_{h}^k}{\partial net_{h}^k} \frac{\partial net_{h}^k}{\partial w_{ih}}\\&=-\eta \sum \limits_{k}f_1'(net_h^k) x_i^k \frac{\partial E(\mathbf{w})}{\partial y_{h}^k} \\&=-\eta \sum \limits_{k} x_i^kf_1'(net_h^k)  \left \lbrace\sum \limits_{m=1}^c \frac{\partial E(\mathbf{w})}{\partial net_{m}^k} \frac{net_m^k}{y_h^k} \right \rbrace \\&=-\eta \sum \limits_{k} x_i^kf_1'(net_h^k)  \left \lbrace\sum \limits_{m=1}^c \frac{\partial E(\mathbf{w})}{\partial net_{m}^k} w_{hm} \right \rbrace \\&=-\eta \sum \limits_{k} x_i^kf_1'(net_h^k)  \left \lbrace\sum \limits_{m=1}^c w_{hm}\left \lbrace \sum \limits_{n=1}^c\frac{\partial  E(\mathbf{w})}{z_n^k}\frac{z_n^k}{\partial net_{m}^k} \right \rbrace \right \rbrace \\&= \eta \sum \limits_{k} x_i^kf_1'(net_h^k)  \left \lbrace\sum \limits_{m=1}^c w_{hm}\left \lbrace \sum \limits_{n\neq m}^c(t_n^k-z_n^k)(-z_n^kz_m^k)+(t_m^k-z_m^k)z_m^k(1-z_m^k) \right \rbrace \right \rbrace\\&= \eta \sum \limits_{k} x_i^kf_1'(net_h^k)  \left \lbrace\sum \limits_{m=1}^c w_{hm}\left \lbrace \sum \limits_{n=1}^c(t_n^k-z_n^k)(-z_n^kz_m^k)+(t_m^k-z_m^k)z_m^k \right \rbrace \right \rbrace\end{align*}$$

### 

### [](#header-7)七、Case 3: 交叉熵+Sigmoid+SoftMax

> 准则函数：交叉熵，隐含层激励函数：Sigmoid，输出层激励函数：SoftMax ，其余步骤与第六节雷同

$$\displaystyle E_{ce}(\mathbf{w})=\sum \limits_{k}J(\mathbf{w})^k=\sum \limits_{k,j} t_j^k \ln (t_j^k/z_j^k) $$

$$\displaystyle \frac{\partial E_{ce}(\mathbf{w})}{\partial z_j^k} = -t_j^k/z_j^k$$

