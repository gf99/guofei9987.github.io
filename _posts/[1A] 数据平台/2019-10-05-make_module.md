---
layout: post
title: 打包Python库
categories:
tags: 1_1_算法平台
keywords:
description:
order: 173
---

## 打包前的准备


### 目录结构
```
scikit-opt/
    └── sko/
        └── __init__.py
    setup.py
```

### setup.py
```py
from setuptools import setup
setup(name='scikit-opt',
      version='0.1',
      description='Genetic Algorithm, Particle Swarm Optimization, Simulated Annealing, Ant Colony Algorithm in Python',
      url='https://github.com/guofei9987/scikit-opt', # 随意填写，一般是项目的 github 地址
      author='Guofei',
      author_email='guofei9987@foxmail.com',
      license='MIT',
      packages=['sko'],
      install_requires=['numpy', 'scipy', 'matplotlib', 'pandas'], # 指定此包的依赖
      zip_safe=False # 为了兼容性，一般填 False
      )
```
- name: 包的名字，也是将来用户使用 `pip install` 时的名字
- version： 每次上传的版本号应该不一样


### 在本地试一试
```bash
$pip install .
```
这样你就可以在自己的电脑上的任何目录中导入包了
```py
import sko
```

（如果只是想做一个包，到这里就算完事了，但如果你想让全世界人方便地用 `pip install` 安装你的包，看下面）

## 上传

### 打包
打包（官网上的步骤，不过还没试过，我用的上面的方法打包）

```bash
# 更新 setuptools wheel 这两个工具
python3 -m pip install --user --upgrade setuptools wheel

# 打包
python setup.py sdist bdist_wheel
```

### 上传

先在 https://pypi.org/ 有个账号

```bash
# 更新 twine 这个工具
$python -m pip install --user --upgrade twine  

# 上传
python -m twine upload --repository-url https://upload.pypi.org/legacy/ dist/*
```



## 享受成功！
上传成功后，全世界所有人都可以使用pip下载你的包啦！
```
$pip install scikit-opt
```

## 参考资料


[CSDN](https://blog.csdn.net/tlonline/article/details/79751658) 中文的，但是打包上传那一部分过时了。  
[官方网站](https://packaging.python.org/tutorials/packaging-projects/#uploading-your-project-to-pypi)

[setup.py 指南](http://blog.konghy.cn/2018/04/29/setup-dot-py/#part-2bb23566e92e12ab)
