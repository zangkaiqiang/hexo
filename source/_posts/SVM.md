---
title: SVM
date: 2020-04-09 09:36:03
tags:
categories: 
- algorithm
---
# 了解SVM
通过寻找得寻找有着最大间隔的超平面实现分类。

# 函数间隔和几何间隔
$$
f(x)=w^Tx+b
$$

此时的超平面为$w^Tx+b=0$所定义

定义函数间隔
$$
r_h=y(w^Tx+b)=yf(x)
$$

在训练数据集$T$中所有的样本点$(x_i,y_i)$，距离超平面最近的点与超平面的函数间隔是超平面关于数据集$T$的函数间隔
$$
r_h=min(r_i) (i=1,2,3...n)
$$
函数间隔的问题是当成比缩放$w,b$，超平面是不会有改变，但是函数间隔会等比变化。

此时需要几何间隔出场了。
$x_0$为$x$在超平面$(w,b)$上的投影，$r$为$x$到超平面的距离
$$
x = x_0+r\frac{w}{||w||} \\\\
w^Tx=w^Tx_0+r\frac{w^T w}{||w||} \\\\
w^Tx=-b+r\frac{||w||^2}{||w||}\\\\
r=\frac{w^Tx+b}{||w||}=\frac{f(x)}{||w||}\\\\
r_g=yr = \frac{yf(x)}{||w||}\\\\
r_g = \frac{r_h}{||w||}
$$
可以看出几何间隔就是函数间隔除以$||w||$，超平面参数的缩放不会改变几何间隔。

# 最大间隔分类器
数据集的函数间隔定义为数据集距离超平面最近的点的间隔，所以几何间隔也是最近的点到超平面的距离。

所以最大间隔分类器的目标函数可以定义为
$$
max(r_g)=max(\frac{r_h}{||w||})
$$
# 拉格朗日对偶变换
根据间隔定义
$$
y_i(w^Tx_i+b)=r_{hi}\geqslant r_h (i = 1,2...n)
$$
我们假设函数间隔为1，那么有
$$
r_h=1\\\\
y_i(w^Tx_i+b)\geqslant1
$$
那么目标函数可以转换为
$$
max\frac{1}{||w||}\quad s.t.,y_i(w^Tx_i+b)\geqslant1
$$
该目标函数等价于
$$
min\ \frac{1}{2}||w||^2\quad s.t.,y_i(w^Tx_i+b)\geqslant1
$$
这种转换是为了计算方便。

定义拉格朗日函数
$$
L(w,b,\alpha)=\frac{1}{2}||w||^2-\sum^n_{i=1}a_i(y_i(w^Tx_i+b)-1)
$$
那么
$$
\theta(w)=\max\limits_{\alpha\geqslant0}L(w,b,\alpha)
$$
此时的目标函数为
$$
\min\ \theta(w) = \min\limits_{w,b} \max\limits_{\alpha\geqslant0}L(w,b,\alpha) = p \\\\
\max \limits_{\alpha\geqslant 0} \min\limits_{w,b}L(w,b,\alpha) = d \\\\
d \leqslant p
$$
在满足某些条件的情况下，这两者相等，这个时候就可以通过求解对偶问题来间接地求解原始问题。转化为对偶问题更容易求解。

## 对偶问题的求解
此时的目标函数
$$
\max \limits_{\alpha\geqslant 0} \min\limits_{w,b}L(w,b,\alpha)
$$
先对$w,b$求极小
$$
\frac{\partial L}{\partial w} = 0\longrightarrow w=\sum^n_{i=1}\alpha_iy_ix_i \\\\
\frac{\partial L}{\partial b}=0 \longrightarrow \sum^n_{i=1}\alpha_iy_i = 0
$$

将其带入$L$

$$
L(w,b,\alpha)=\frac{1}{2}||w||^2-\sum^n_{i=1}a_i(y_i(w^Tx_i+b)-1)\\\\
=\sum^n_{i=1}\alpha_i-\frac{1}{2}\sum^n_{i,j=1}\alpha_i\alpha_jy_iy_jx^T_ix_j
$$
此时的拉格朗日函数只包含了一个变量
$$
\max\limits_\alpha\sum^n_{i=1}\alpha_i-\frac{1}{2}\sum^n_{i,j=1}\alpha_i\alpha_jy_iy_jx^T_ix_j \\\\
s.t.,\alpha_i\geqslant0,i=1,2...n\\\\
\sum^n_{i=1}\alpha_iy_i = 0
$$

# 核函数
当处理线性不可分的数据时，无法在当前维度找到超平面，此时可以选择将该低维数据映射到高维空间。
映射到高维空间时可能发生维度不可控的问题，当维度过大时，将很难计算。

核函数就是解决这样的问题。既可以将数据映射到高维空间，有可以在低维空间进行计算。perfect！
## 多项式核
多项式核就是非常容易造成维度过大的问题
## 高斯核函数
$$
k(x_1,x_2)=\exp^{(-\frac{||x_1-x_2||^2}{2\sigma^2})}
$$
该核函数可以将数据映射到无穷维
$$
Talor变换
$$

# 松弛变量处理outliers

# SMO算法推导


