---
title: matplotlib中文乱码问题
date: 2020-04-15 09:30:44
tags:
categories: 
- devops
---

两种方法
## 在代码中嵌入配置代码
首先下载SimHei字体，在运行代码的机器上装上

其次是在代码中加入如下设置，表示将字体替换为SimHei，支持中文 
```
import matplotlib.pyplot as plt
plt.rcParams['font.sans-serif'] = ['SimHei']
plt.rcParams['axes.unicode_minus'] = False
```
## 一劳永逸的方法
省略了

## 参考
* https://blog.csdn.net/fwj_ntu/article/details/82116733
* https://www.zhihu.com/question/25404709
* https://segmentfault.com/a/1190000005144275