tf2.0

删除了 placeholder，session

## 示意1

导入包，载入数据
```python
import tensorflow as tf
from tensorflow import keras

fashion_mnist = keras.datasets.fashion_mnist
(train_images, train_labels), (test_images, test_labels) = fashion_mnist.load_data()
x_valid, y_valid = train_images[:5000], train_labels[:5000]
x_train, y_train = train_images[5000:], train_labels[5000:]

class_names = ['T-shirt/top', 'Trouser', 'Pullover', 'Dress', 'Coat',
               'Sandal', 'Shirt', 'Sneaker', 'Bag', 'Ankle boot']
```

构建模型，运行模型
```python
model = keras.Sequential([
    keras.layers.Flatten(input_shape=(28, 28)),
    keras.layers.Dense(128, activation='relu'),
    keras.layers.Dense(10, activation='softmax')
])
# 也可以用 model.add(keras.layers.Dense(128, activation='relu')) 来一步一步添加

model.compile(optimizer='adam',
              loss='sparse_categorical_crossentropy',
              metrics=['accuracy'])

history = model.fit(x=x_train, y=y_train, epochs=10, validation_data=(x_valid, y_valid))
# 或者 validation_split = 0.01 也可以自动分验证集

predictions = model.predict(test_images)
```

#### 显示模型的一些情况
```python
model.layers
model.summary()

history.history # 存放的是迭代过程中的一些值

model.evaluate(test_images, test_labels)
```

这里自编一个显示指标图形的代码


```python
import matplotlib.pyplot as plt

fig,ax=plt.subplots(1,1)

for i,j in history.history.items():
    ax.plot(j,label=i)

ax.set_ylim(0,1)
ax.legend()

plt.show()
```




### callbacks
- EarlyStopping
- ModelCheckpoint
- TensorBoard


```python
import os
logdir='.\\callbacks'
output_model_file=os.path.join(logdir,'fashin_mnist_model.h5')
callbacks=[keras.callbacks.TensorBoard(logdir),
           keras.callbacks.ModelCheckpoint(output_model_file,save_best_only=True), # 如果是 False，保存最后一个
           keras.callbacks.EarlyStopping(min_delta=1e-3,patience=5)

]


history = model.fit(x=train_images, y=train_labels, epochs=100, validation_split = 0.01 ,callbacks=callbacks)
```

TensorBoard：（跑不通？？？）
```bash
tensorboard --logdir=callbacks
```

### 归一化

代码待补，因为下面的 batch normalization，在数据归一化的前提下，效果才更好。

### batch normalization



批归一化可以缓解深度神经网络的梯度消失，因为每一层更加规整。

批归一化有3种做法：（下面代码中）
1. 先归一化再激活，
2. 先激活后归一化，
3. 用selu作为激活函数，这是个自带Normalization的激活函数，而且相比之下，训练速度快，训练效果好

```python
model = keras.Sequential([
    keras.layers.Flatten(input_shape=(28, 28))
])
for _ in range(20):
    model.add(keras.layers.Dense(100,activation='selu'))

    # 先激活，后归一化
    #     model.add(keras.layers.Dense(100,activation='relu'))
    #     model.add(keras.layers.BatchNormalization())

    # 先归一化，再激活
    # model.add(keras.layers.Dense(100))
    # model.add(keras.layers.BatchNormalization())
    # model.add(keras.layers.Activation('relu'))

model.add(keras.layers.Dense(10, activation='softmax'))
```


### dropout

一般不会给每一层都添加dropout，而是最后几层。
```python
model.add(keras.layers.Dropout(rate=0.5))
model.add(keras.layers.AlphaDropout(rate=0.5))
```

关于 AlphaDropout：
- 均值和方差不变
- 因此归一化性质不变
- 因此可以和 BatchNormalization 连用














## 示例2
需要下载两个包，分别是训练好的模型、数据
```bash
!pip install -q tensorflow-hub
!pip install -q tensorflow-datasets
```

### 使用训练好的模型
下面是一些训练好的 word embedding 模型
- google/tf2-preview/gnews-swivel-20dim/1
- google/tf2-preview/gnews-swivel-20dim-with-oov/1 与上面这个相同，但是 2.5% 作为OOV，因此适合用来 task 的 vocabulary 不重合的情况。
- google/tf2-preview/nnlm-en-dim50/1 很大的模型，1M vocabulary + 50 dimensions
- google/tf2-preview/nnlm-en-dim128/1 更大的模型 1M vocabulary + 128 dimensions

使用方法有两种：
#### 第一种使用方法
```python
import tensorflow_hub as hub

embed = hub.load("https://hub.tensorflow.google.cn/google/tf2-preview/gnews-swivel-20dim/1")
embeddings = embed(["cat is on the mat", "dog is in the fog"])
```

#### 第二种使用方法

```python
hub_layer = hub.KerasLayer("https://hub.tensorflow.google.cn/google/tf2-preview/gnews-swivel-20dim/1", output_shape=[20],
                           input_shape=[], dtype=tf.string)

model = keras.Sequential()
model.add(hub_layer)
model.add(keras.layers.Dense(16, activation='relu'))
model.add(keras.layers.Dense(1, activation='sigmoid'))

model.summary()
```

### 完整的代码
导入包
```python
import tensorflow as tf

!pip install -q tensorflow-hub
!pip install -q tensorflow-datasets
import tensorflow_hub as hub
import tensorflow_datasets as tfds


print("Version: ", tf.__version__)
print("Eager mode: ", tf.executing_eagerly())
print("Hub version: ", hub.__version__)
print("GPU is", "available" if tf.config.experimental.list_physical_devices("GPU") else "NOT AVAILABLE")



train_validation_split = tfds.Split.TRAIN.subsplit([6, 4])

(train_data, validation_data), test_data = tfds.load(
    name="imdb_reviews",
    split=(train_validation_split, tfds.Split.TEST),
    as_supervised=True)



train_examples_batch, train_labels_batch = next(iter(train_data.batch(10)))
train_examples_batch
```

下载已经训练好的模型，作为 embedding

```python
embedding = "https://hub.tensorflow.google.cn/google/tf2-preview/gnews-swivel-20dim/1"
hub_layer = hub.KerasLayer(embedding, input_shape=[],
                           dtype=tf.string, trainable=True)
hub_layer(train_examples_batch[:3])
```

用 keras 建模
```python
model = tf.keras.Sequential()
model.add(hub_layer)
model.add(tf.keras.layers.Dense(16, activation='relu'))
model.add(tf.keras.layers.Dense(1, activation='sigmoid'))

model.summary()
```

标准流程了
```python
model.compile(optimizer='adam',
              loss='binary_crossentropy',
              metrics=['accuracy'])

history = model.fit(train_data.shuffle(10000).batch(512),
                    epochs=20,
                    validation_data=validation_data.batch(512),
                    verbose=1)
```

加上一个模型评估
```python
results = model.evaluate(test_data.batch(512), verbose=2)

for name, value in zip(model.metrics_names, results):
    print("%s: %.3f" % (name, value))
```


（未完）

## callback

- earlystopping
