input_function -> input_fn






## 《编写高质量代码：改善Python程序的91个建议》
一些 Pythonic 的做法

- 包的命名采用小写、单数，且短小
- 包通常仅作为命名空间，如只包含空的 `__init__.py` 文件

### 建议
避免劣质的代码
- 避免用大小写作为不同的对象，例如 A 是一个对象，a是另一个对象
- 避免混淆。例如，用同一个词反复作为不同的对象。用内建名称作为变量名（`list`这种），使用o,l（与数字0, 1 混淆）
- 不要怕过长的变量名，相反，不要过分缩写


PEP8 是一个关于 Python 编码风格的指南。  

```bash
pip install pycodestyle

pycodestyle demo.py
```

#### 常量
Python中的常量有 True, False, None 少量几个，不支持自定义常量，通常用 **大写字母+下划线** 来提醒用户这按照常量来理解。

## 还有
```python
# 用字符串的方式导入包，不过不推荐使用
__import__('numpy')
```


```python
# 这是一个dict，内容是已经加载的包
sys.modules

# 这是一个list，内容是命名空间中所有变量
dir()
```

（不是严格标准）尽量避免使用类似 `from scikit-opt import GA`，因为在大型项目中，如果频繁使用这个句式，会增加命名空间冲突的可能性。

## with语句
with语句做了什么？
1. 计算表达式的值，返回一个上下文管理对象
2. 加载上下文管理对象的 `__exit__()` 方法以备后用
3. 调用上下文管理器对象的 `__enter__()` 方法
4. 如果with语句中设置了目标对象，把 `__enter__()` 方法的返回值赋值给目标对象
5. 执行 `with` 中的代码块
6. 如果正常结束，调用 `__exit__()` 方法，忽略返回值。
7. 如果发生异常，调用 `__exit__()` 方法，把异常类型、值、traceback信息传递给 `__exit__()`。如果 `__exit__()` 返回 True，异常被挂起，程序继续执行；如果返回False，抛出异常。

with语句好处是无论何种方式跳出代码块，`__exit__()` 总是被执行


你可以定义自己的上下文管理器：
```python
class MyContextManager(object):
    def __enter__(self):
        print('enter')

    def __exit__(self, exc_type, exc_val, exc_tb):
        print('leaving', exc_type, exc_val, exc_tb)
        if exc_type is None:
            return False
        if exc_type is ZeroDivisionError:
            print('ZeroDivisionError')
            return True
        print('other error')
        return True

with MyContextManager():
    1/0
```

## 多用else
for循环后面可以跟else，当循环自然终结时，会执行else；如果被break打断，不执行else

```python
for i in range(4):
    print(i)
    if i>5:break
else:
    print('no more then 5')
```


try 后面跟else，当try语句没发生异常时，执行else，全语句： `try-except-else-finnaly`

## 可变对象和不可变对象


```python
class STUDENT:
    def __init__(self, name, course=[]):
        self.name = name
        self.course = course

    def add_course(self, course_name):
        self.course.append(course_name)


Lucy = STUDENT('Lucy')
Lily = STUDENT('Lily')

Lucy.add_course('math')
Lily.add_course('PE')

print(Lucy.course)
print(Lily.course)
```

很惊讶的发现，虽然 `Lucy` 和 `Lily` 是不同的对象，但course却指向同一个地址。这是因为默认参数仅仅评估一次。

## 配置文件
常见有XML，ini

`ConfigParser` 是一个常用的解析配置文件的包

`getboolean('section1', 'option1')` 可以解析 yes/no, true/false, on/off

## traceback来调试报错信息
## logging 来记录日志

logger分为五个等级
1. DEBUG，
2. INFO，正常的信息
3. WARNING
4. ERROR
5. CRITICAL


```python
import logging

logging.Logger.setLevel()
logging.Logger.addHandler()
logging.Logger.removeHandler()
logging.Logger.addFilter()

logging.Logger.debug()
logging.Logger.info()
logging.Logger.warning()
logging.Logger.error()
```

一个简单案例
```python
import logging

logging.basicConfig(
    level=logging.DEBUG,
    filename='log.txt',
    filemode='w',
)

k = 0
for i in range(5):
    k += i
    logging.info('[INFO]:calling i={i}'.format(i=i))
```


## 如何优化代码
1. 最优先：保证代码可运行、不破坏结构、秩序
2. 权衡不同的方面
3. 定义指标，防止与客户的理解出现偏差
4. 不能牺牲代码可读性


## 如何命名
作者：hzwer
链接：https://zhuanlan.zhihu.com/p/104963169
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

add 加
anc, ancestor 祖先
ans, answer 答案
bel, belong 属于
best 最佳的
build 建立
block 障碍
ch, char 字符
check 判定
color 颜色
cmp, compare 比较
cnt, count 计数器
cur, current 当前量
deg, degree 度数
dep, depth 深度
del 删除（delete是保留字）
delta 增量
diff, difference 差别
dist, distance 距离
div, division 除法，部分
dp 动态规划
edge 边
extra 额外的
fa, father 父亲
factor 因子
fail 失败
flag 标志
flow 流
from 来自
get 得到
Hash 哈希表（hash是保留字）
heap 堆
in 入
ind, index 标号
inq 在队列里
inf, infinity 无穷大
init, initialize 初始化
insert 插入
inv, inverse 翻转，颠倒
last 最后一个
len, length 长度
lim, limit 极限
low, lower 下边的
mat, matrix 矩阵
mid, middle 中间量
mod 模
modify 修改
mp, map 映射
mst 最小生成树
mul, multiply 乘法
node 结点
num, number 数量
nxt 后继（next是保留字）
out 出
pa, pair 对子
pre, precursor 前驱
prime 质数
pos, position 位置
prod, product 乘积
put 放置
que, queue 队列
query 询问
rank 秩
res, result 结果
res, residual 剩余
scc 强连通分量
size 大小
split 分裂
start 开始
stk, stack 栈
str 字符串
suc, succeed 后继
sum 和
tim 时间（time是保留字）
tmp, temporary 临时量
tree 树
to 表目的
unite 联合
up, upper 上边的
update 更新
used 使用过的
val, value 值
vec, vector 向量
vis, visit 访问
zero 零
