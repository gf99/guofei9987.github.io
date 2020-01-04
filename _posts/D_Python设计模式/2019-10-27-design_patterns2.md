---
layout: post
title: 【Python】装饰器
categories:
tags: Python设计模式
keywords:
description:
order: 1004
---

## 为什么
我们要做一个 fibonacci 计算函数，自然想到用递归
```python
def fibonacci(n):
    if n < 0:
        raise ValueError('n>=0')
    return n if n in (0, 1) else fibonacci(n - 1) + fibonacci(n - 2)
```
你会发现这里有大量重复计算，导致计算效率极低。自然想到改进，把已经计算好的函数值记录下来
```python
known = {0: 0, 1: 1}
def fibonacci2(n):
    if n in known:
        return known[0]
    else:
        res = fibonacci2(n - 1) + fibonacci2(n - 2)
        known[n] = res
    return res
```
实验发现，这个速度快了很多。新问题，如果你想写一个包，里面有不同的递归算法（帕斯卡三角等），这样写会让代码一团乱麻，因为每个方法你都要定义一个known。

## 装饰器
### 闭包
```python
def print_msg():
    msg = "I'm closure"

    # printer是嵌套函数
    def printer():
        print(msg)

    return printer


closure = print_msg()
closure()  # 输出 I'm closure
```

### 一个普通的装饰器
```python
import functools


def log(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        print('call {}():'.format(func.__name__))
        print('args = {}'.format(*args))
        return func(*args, **kwargs)

    return wrapper


@log
def test(p):
    print(test.__name__ + " param: " + p)


test("I'm a param")
# 输出：
# call test():
# args = I'm a param
# test param: I'm a param
```

分析：等价于这个
```python
def log(func):
    def wrapper(*args, **kwargs):
        print('call {}():'.format(func.__name__))
        print('args = {}'.format(*args))
        return func(*args, **kwargs)

    return wrapper


def test(p):
    print(test.__name__ + " param: " + p)


log(test)("I'm a param")
```

- `@functools.wraps(func)`。它能把原函数的元信息拷贝到装饰器里面的 func 函数中。函数的元信息包括docstring、name、参数列表等等。
- 如果删掉，会发现 `log(test)` 的名字变成了 log.wrapper



### 带参数的装饰器

```python
import functools


def log_with_param(param):
    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args, **kwargs):
            print('call %s():' % func.__name__)
            print('args = {}'.format(*args))
            print('log_param = {}'.format(param))
            return func(*args, **kwargs)

        return wrapper

    return decorator


@log_with_param("param")
def test_with_param(p):
    print(test_with_param.__name__ + ' with args ' + str(p))


test_with_param(1)
```

## 应用
能显示运行时间的装饰器


```python
import time


def timer(func):
    def d_func(*args, **kwargs):
        start_time = time.time()
        result = func(*args, **kwargs)
        end_time = time.time()
        print(end_time - start_time)
        return result

    return d_func


@timer
def myfunc(a):
    time.sleep(2)
    print(a)


myfunc(3)
```


## 附：scipy的做法
来源：
```python
from scipy import optimize
optimize.fmin
```
解释一下，ncalls是一个计数器，记录函数被调用的次数
```python
def wrap_function(function, args):
    ncalls = [0]
    if function is None:
        return ncalls, None

    def function_wrapper(*wrapper_args):
        ncalls[0] += 1
        return function(*(wrapper_args + args))

    return ncalls, function_wrapper
```

## 参考资料

【印】Chetan Giridhar:《Python 设计模式》, 中国工信出版集团
