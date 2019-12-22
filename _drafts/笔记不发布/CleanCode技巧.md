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


```
# 这是一个dict，内容是已经加载的包
sys.modules

# 这是一个list，内容是命名空间中所有变量
dir()
```

（不是严格标准）尽量避免使用类似 `from scikit-opt import GA`，因为在大型项目中，如果频繁使用这个句式，会增加命名空间冲突的可能性。
