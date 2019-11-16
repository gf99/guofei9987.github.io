### 安装
```
pip install -U Sphinx
```

### 初始化一个空的项目
```
sphinx-quickstart
```

### 创建网站
```
sphinx-build -b html sourcedir builddir
```
这个里面，sourcedir 是你存放source的目录名，builddir是你想存放html的目录名

或者，目录中有个 `make.bat`，你可以用 `make html`


## 好用的功能
### 代码引用
```rst
.. py:function:: enumerate(sequence[, start=0])

   Return an iterator that yields tuples of an index and an item of the
   *sequence*. (And so on.)


.. function:: enumerate(sequence[, start=0])

   ...


The :py:func:`enumerate` function can be used for ...

.. py:class:: name
.. py:method:: name
```

#### Autodoc
做这一步之前，应当先去 `conf.py` 文件。找到 `extensions`，这是一个 `list`，添加一个字符串 `'sphinx.ext.autodoc'`

然后这样就可以了
```
.. autofunction:: io.open
```

或者整个包
```
.. automodule:: io
   :members:
```
