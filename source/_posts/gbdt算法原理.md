---
title: gbdt算法原理
date: 2020-04-10 10:07:45
tags:
categories: 
- algorithm
---
已经有很多文章讲了GBDT的原理，这里就记录下一些个人不容易理解的地方。

## 负梯度
$$
r_{mi}=-[\frac{\partial L(y,f(x_i)}{\partial f(x_i)}]\\\\
s.t.,{f(x)=f_{m-1}(x)}
$$
<!--more-->
这里的负梯度是损失函数在当前模型的负梯度，并不是我们常见的对变量的梯度。原因是算法是要对当前模型做增量更新，并不是对$x$做增量更新。
## $C$值
$$
c_{mi}=\arg \min \limits_c \sum_{x_i R_{mj}}L(y_i,f_{m-1}(x_i)+c)
$$
这个公式困扰了我很长一段时间。$c$是干嘛的？加$c$那之前生成的决策树还有什么用？

之前生成的决策树只是为了确定第$m$个弱学习器的叶子节点的区域$R_{m,j}$,然后使用$m-1$步的模型来对该叶子结点的区域进行拟合，$R_{m,j}$和$R_{m-1,j}$是不一样的，所以每一步并不可以直接去加梯度生成的决策树，而是根据新的区域划分，使用上一步的模型来重新确定变化。该变化就是$c$。

## 参考
* https://www.cnblogs.com/pinard/p/6140514.html
* https://blog.csdn.net/aaa_aaa1sdf/article/details/81588382
* https://blog.csdn.net/shine19930820/article/details/65633436