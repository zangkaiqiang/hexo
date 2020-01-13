---
title: Hello
date: 2020-01-09 16:56:49
tags:
---

# A Parallel Hybrid Evolutionary Metaheuristic for the Vehicle Routing Problem with Time Windows

## Hermann Gehring and Jörg Homberger

## 概要

* 本文使用两阶可以并行的程序来解决带时间窗的车辆路径规划问题
    1. 第一阶段的目的是最小化车辆的数目
        * 选择y个可行的邻域
            * 使用节约里程法的修改版本初始化初始化解决方案
            * 使用随机的move operator产生可行的邻域
            * 对生成的邻域使用局部搜索算法，生成减少了车辆使用数目的二段邻域
            * 评估二段邻域，选择是否将其插入实际邻域方案
            * 循环此操作直到生成y个可行的邻域
        * 从y个可行的邻域中选择最佳方案，如果该方案大于已经存在的best方案，则将best替换为此方案
        * 重复上述操作一定次数后得到best初始化方案
    2. 第二阶段的目的是最小化历程数，前提条件是不会增加第一阶段产生的车辆数
        * 使用禁忌搜索算法优化

## 知识点

* 节约历程算法
* 局部搜索算法
* 禁忌搜索算法
