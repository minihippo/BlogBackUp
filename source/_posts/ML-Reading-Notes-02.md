---
title: ML Reading Notes-02
date: 2018-01-23 18:13:37
tags:
	- notes
	- ML
reward: true
toc: ture
---

# Regression

**Background**:以pokeman go为例学习regression。对pokeman进化后的cp值问题建模。   
               训练数据有十只不同pokeman，它们有初始cp值及其他各种属性。   

<!--more-->

**Steps:**    
① Model: a set of function   
　　linear $y = b+w\*x\_{cp}$,  bias & weight   
② Goodness of function: train data and set loss function $L$ to evaluate goodness of $f$.    
　　 $L(f) = \sum\_{n=1}^{10}(\hat{y}^n - f(x\_{cp}^n))^2$,  input a function, output how bad it is.  
　　sum over examples to calculate variance; $\hat{y}^n$—true value    
③ Best function: gradient descent   
　　pick the best function $f^\* = arg min\_f L(f)$, related to $w,b$

<u>**How to find the best function?**</u>
## Gradient Descent

#### 1.one parameter

**pre**: consider $L$ with only one parameter $w$  

**steps**:   
1.randomly pick an intial value $w^0$ 
2.compute $w^0$处导数:$$\frac{dL}{dw}|\_{w=w^0}$$3.update $$w^1 = w^0 -\eta\frac{dL}{dw}|\_{w=w^0}$$$\eta$ is learning rate/step length

**一维梯度下降过程**：

<img src="/ML-ReadingNotes/2-1.png" style="zoom:50%">  
梯度下降方法由于初始点的选择常常只能找到局部最优点，非全局最优解。**回归问题**由于是凸面的因此局部最优就是全局最优。

##### 2.two parameter

**pre**: $f^\* = arg min\_f L(f)$　 =>　 $ w^\*,b^\* = arg min\_(w,b) L(w,b)$  

**steps**:   
1.randomly pick an intial value $w^0, b^0$   
2.compute $w^0, b^0$处偏导数$$\frac{\partial L}{\partial w}|\_{w=w^0,b=b^0},\frac{\partial L}{\partial b}|\_{w=w^0,b=b^0}$$

**gradient**: $$\nabla L(w, b)=[\frac{\partial L}{\partial w},\frac{\partial L}{\partial b} ]^T$$

**二维梯度下降过程**：

<img src="/ML-ReadingNotes/2-2.png" style="zoom:50%">

##### 3. 梯度收敛的情况

　　极小值、鞍点  
<img src="/ML-ReadingNotes/2-3.png" style="zoom:40%">  
因此收敛并不代表找到了最小值或极小值。

### Model Generalization

比较了一次、二次、三次、四次、五次式模型对应的训练误差和泛化误差，它们之间的关系如下：

<img src="/ML-ReadingNotes/2-4.png" style="zoom:50%">  
复杂模型会包含简单模型，因此也更有可能包含最优解。

#### 泛化性能差

redesign the model:  $y = b+ \sum w\_i x\_i$ , consider more hidden factors,  
　　　　　　　　　　模型中也可以考虑属性的平方

#### overfitting

**def**: a more complex model does not always lead to a better performance on testing data.   

**solution**: Regularization　　 $\lambda\sum(w\_i)^2 $   

**Regularization**  
origin loss：$$L = \sum\_n (\hat{y}^n - (b+\sum w\_ix\_i))^2,$$
只考虑预测值和真实值的误差
improved loss：$$L = \sum\_n (\hat{y}^n - (b+\sum w\_ix\_i))^2+\lambda\sum(w\_i)^2,$$     
考虑了weight的误差.当weight越小，function受到输入数值大小的影响越小，也就越平滑   
$$y+\sum w\_i \Delta x\_i = b + \sum (x\_i + \Delta x\_i) $$   
因此$\lambda$越大，考虑smooth就越多，training error就越少。

<img src="/ML-ReadingNotes/2-5.png" style="zoom:40%">

