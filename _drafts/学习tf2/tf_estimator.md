tf.estimator
- `tf.estimator` 预装了很多机器学习模型
- 继承自`tf.estimator.Estimator`
- 可以自定义 estimator： https://www.tensorflow.org/tutorials/estimator/keras_model_to_estimator




需要做这几件事：
* Create one or more input functions.
* Define the model's feature columns.
* Instantiate an Estimator, specifying the feature columns and various
  hyperparameters.
* Call one or more methods on the Estimator object, passing the appropriate
  input function as the source of the data.


### input functions
可以自己写一个函数， return features, labels（具体不写了，用的时候查）

更推荐的方法是使用 tf.data

### feature columns
tf.feature_column


例如，如果是feature 是 float，那么：
```python
my_feature_columns = [
    tf.feature_column.numeric_column(key='feature1')
]
```

>[NumericColumn(key='feature1', shape=(1,), default_value=None, dtype=tf.float32, normalizer_fn=None)]


这个可以很复杂，参见 [this guide](https://www.tensorflow.org/guide/feature_columns)

### Instantiate an estimator

* `tf.estimator.DNNClassifier` for deep models that perform multi-class
  classification.
* `tf.estimator.DNNLinearCombinedClassifier` for wide & deep models.
* `tf.estimator.LinearClassifier` for classifiers based on linear models.


```python
classifier = tf.estimator.DNNClassifier(
    feature_columns=my_feature_columns,
    # Two hidden layers of 30 and 10 nodes respectively.
    hidden_units=[30, 10],
    # The model must choose between 3 classes.
    n_classes=3)
```    
