## Tensor基本操作

生成
```python
tf.zeros([10, 5])
```

运算
```python
tf.add(1, 2)
tf.add([1, 2], [3, 4])
tf.square(5)
tf.reduce_sum([1, 2, 3])
tf.square(2) + tf.square(3)
```


矩阵计算
```python
tf.matmul([[1]], [[2, 3]])
```

map
```python
tf.data.Dataset.from_tensor_slices([1, 2, 3, 4, 5, 6])
ds_tensors.shuffle(2).batch(2).map(tf.square)
# 这是个 <dataset> 用法见于相关内容
```

## 自动微分


```python
x = tf.ones((2, 2))

# persistent=True 就可以多次运行 t.gradient，否则只能运行一次
with tf.GradientTape(persistent=True) as t:
  t.watch(x)
  y = tf.reduce_sum(x)
  z = tf.multiply(y, y)

# Derivative of z with respect to the original input tensor x
dz_dx = t.gradient(z, x)
dz_dx

dz_dy = t.gradient(z, y)
dz_dy
```

高阶微分
```python
x = tf.Variable(1.0)  # Create a Tensorflow variable initialized to 1.0

with tf.GradientTape() as t:
  with tf.GradientTape() as t2:
    y = x * x * x
  # Compute the gradient inside the 't' context manager
  # which means the gradient computation is differentiable as well.
  dy_dx = t2.gradient(y, x)
d2y_dx2 = t.gradient(dy_dx, x)
```

## 训练
```
v = tf.Variable(3.0)
v.assign(tf.square(v))
```
