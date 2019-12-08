安装 cuda

## GPU支持
先在 nvidia 官网安装 cuda https://developer.nvidia.com/cuda-downloads

然后安装 tensorflow-gpu
```bash
pip install tensorflow-gpu
```

列出可用的设备
```
tf.config.experimental.list_physical_devices()
tf.config.experimental.list_physical_devices("GPU")
tf.config.experimental.list_physical_devices("CPU")
```


切换回CPU的方法
```
import os
os.environ["CUDA_VISIBLE_DEVICES"]="-1"
```


运行，并看用了哪个设备
```python
tf.debugging.set_log_device_placement(True)

with tf.device('/device:GPU:0'):
  a = tf.constant([[1.0, 2.0, 3.0], [4.0, 5.0, 6.0]])
  b = tf.constant([[1.0, 2.0], [3.0, 4.0], [5.0, 6.0]])
  c = tf.matmul(a, b)

print(c)
```
