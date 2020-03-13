---
layout: post
title: 【Python数据结构2】Queue & Stack
categories:
tags: 8-数据结构与算法
keywords:
description:
order: 552
---



## list实现栈和队列
我们知道，list天然地适合做 Stack，即尾部入，尾部出。  
我们又知道 list 删除头部的元素是极为低效的，解决方法是很简单，只要增加一个指向头部的指针即可，。  



## queue 介绍
一种 first-in-first-out (FIFO) 算法
### queue 案例1
queue的一种典型应用场景是Breadth-first Search (BFS)

### queue 案例2
题目灵感来自[LeetCode题目](https://leetcode.com/problems/baseball-game/discuss/119575/Python-4-liner),我给出的解答见于[这里](https://github.com/guofei9987/leetcode_python/blob/master/%5B682%5D%5BBaseball%20Game%5D%5BEasy%5D.py)  

例子：
```python
input_queue=[1,2,3,4,5]    
stack=[2,1]
pointer=0
for i in input_queue:
    stack.append(i)
    # 后两行是先出的功能：
    stack_out=stack[pointer]
    pointer+=1

# 事实上，因为可以使用stack[-1],stack[-2]这些命令，所以 pointer 这个变量往往不必定义
# 会有内存浪费，定期清理即可，参考代码： stack=stack[pointer:]
```


## Circular Queue
用list来模拟Cirular Queue
```py
num_list[i%len_list]
```

### stack
Last-in-first-out Data Structure（先进先出表）

### 案例1
深度优先搜索 Depth-First Search (DFS)
## 案例
[200. Number of Islands](https://leetcode.com/problems/number-of-islands/description/)  
