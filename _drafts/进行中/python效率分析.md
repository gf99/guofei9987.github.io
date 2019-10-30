## 循环
测试函数：
```
import time
import datetime
def func1(i):
    print('start: ',datetime.datetime.now())
    time.sleep(1)
    print('end: ',datetime.datetime.now())
    return None

func1(1)
```
几种实现
```
%%time
a=[func1(i) for i in range(5)]

%%time
b=list(map(func1,range(5)))

%%time

c=[]
for i in range(5):
    c.append(func1(i))
```

发现
- 消耗的时间差不多
- map并没有实现网上说的并行


网上查询发现，用这个可以实现并行
```python
from multiprocessing import Pool
from multiprocessing.dummy import Pool as ThreadPool

pool = ThreadPool() # ThreadPool(4), 不指定进程数，则使用全部线程

pool.map(func1,range(5))
```

另一个并行工具
mpi4py，基于MPI-1/MPI-2


## 实战
在实数优化问题上，差别不是很大。可能是因为循环内部的运算较为简单，不足以抵消多线程带来的不良后果。
