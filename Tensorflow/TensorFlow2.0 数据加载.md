# TensorFlow 数据加载与预处理



### 2.1 图像加载

为了高效地读取数据，比较有帮助的一种做法是对数据进行序列化并将其存储在一组可线性读取的文件（每个文件 100-200MB）中。这尤其适用于通过网络进行流式传输的数据。这种做法对缓冲任何数据预处理也十分有用。

TFRecord 格式是一种用于存储二进制记录序列的简单格式。

[协议缓冲区](https://developers.google.com/protocol-buffers/?hl=zh-cn)是一个跨平台、跨语言的库，用于高效地序列化结构化数据。

协议消息由 `.proto` 文件定义，这通常是了解消息类型最简单的方法。

`tf.Example` 消息（或 protobuf）是一种

灵活的消息类型，表示 `{"string": value}` 映射。它专为 TensorFlow 而设计，并被用于 [TFX](https://tensorflow.google.cn/tfx/?hl=zh-cn) 等高级 API。

本笔记本将演示如何创建、解析和使用 `tf.Example` 消息，以及如何在 `.tfrecord` 文件之间对 `tf.Example` 消息进行序列化、写入和读取。

注：这些结构虽然有用，但并不是强制的。您无需转换现有代码即可使用 TFRecord，除非您正在使用 [tf.data](https://tensorflow.google.cn/guide/datasets?hl=zh-cn) 且读取数据仍是训练的瓶颈。有关数据集性能的提示，请参阅[数据输入流水线性能](https://tensorflow.google.cn/guide/performance/datasets?hl=zh-cn)。

`TensorFlow`读取图像数据的方式，主要包括3种，主要包括， 

### 2.2 CSV数据加载

### 2.3 文本数据加载









