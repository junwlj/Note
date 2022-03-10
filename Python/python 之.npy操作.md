# python 之.npy操作

##### 1.什么是`.npy`文件

`.npy`文件是由安装了**numpy**库的python软件包创建的*Numpy*数组文件，它包含一个<font color='red'>排列</font>以*Numpy（.npy）*文件格式保存。<font color='red'>.npy</font>文件存在在任何计算机上重建阵列所需的所有信息，包括<font color='red'>dtype</font>和<font color='red'>shape</font>信息。

##### 2.`.npy`文件的读取保存

```python
import numpy as np

x =  [[1, 2, 3], [1, 2, 3], [1, 2, 3] ]
# .npy 文件的保存
np.save('filename.npy', x)
# .npy 文件加载
np.load('filename.npy')
```









