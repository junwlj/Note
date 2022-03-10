# TensorFlow屏蔽警告信息处理方案

在tensorflow中的警告信息等级如下所示：

| lever | lever forhumans | Lever Description                                |
| ----- | --------------- | ------------------------------------------------ |
| 0     | DEBUG           | all message are logged(Default)                  |
| 1     | INFO            | INFO messageare not printed                      |
| 2     | WARING          | INFO and WARNING message are note printed        |
| 3     | ERROTR          | INFO, WARNING and ERROR message are note printed |

在TensorFlow运行过程通过在<font color='red'>导入TensorFlow之前</font>添加如下代码以屏蔽运行过程中产生的日志信息：

```python
import os
os.environ['TF_CPP_MIN_LOG_LEVEL'] = '2'
# 0 打印所有信息
# 1 info信息不会打印
# 2 info、warning 信息不会被打印
# 3 info、warning、error 都不会被打印
import tensorflow as tf
tf.__version__
```

<font size='5' color='red'>注意：在导入过程中一定注意先后顺序</font>

