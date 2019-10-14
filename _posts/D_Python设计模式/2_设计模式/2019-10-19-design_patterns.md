---
layout: post
title: 【Python】【设计模式】1
categories:
tags: Python设计模式
keywords:
description:
order: 1004
---

设计模式是开发者和架构师的宝贵经验，是具有泛用性的方法论。


## 单例模式
确保类有且只有一个对象
```py
class A(object):
    def __new__(cls):
        if not hasattr(cls, 'instance'):
            cls.instance = super(A, cls).__new__(cls)
        return cls.instance


obj1 = A()
obj2 = A()
obj1 is obj2
```
用途：  
数据库、打印机、导入库等只要一个实例的场景
缺点：  
1. 对象可能在某个地方被误改，但开发人员并不知道
2. 可能会提高耦合性（类似全局变量）
