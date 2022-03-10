# Numpy笔记

​		NumPy代表Numerical Python，它是一个由多维数组对象和一组处理这些数组的例程组成的库。使用NumPy，可以执行数组的数学和逻辑运算。

### 一、使用Numpy

​		在Python3中使用numpy时首先要导入numpy的包，即使用:

```python
import numpy as np
np.__version__
```

### 二、创建ndarray对象

​		在Numpy中定义的最重要的对象时ndarray的n维数组，它描述了相同类型的项目的集合。可以使用从零开始的索引来访问集合中的项目。

​		ndarray中的每个项目在内存中占用相同的块的大小，在ndarray中每个元素是数据类型的对象（被称为的目的 **D型**）

-   使用list列表和numpy.array()函数即数组函数创建numpy
-   使用numpy库中提供的相关函数

#### 1.使用list方式创建多维数组ndarray对象

`numpy.array(object, dtype = None, copy = True, order = None, subok = Flase, ndmin= 0)`

- `object`:任何暴露数据接口方法的对象都会返回一个数组或任何(嵌套)序列
- `dtype`：数组的期望类型
- `copy`：默认情况为`True`，对象被复制
- `order`: C(行主)或F(列主)或A（任何）（默认）

​		即使用np.arrry()数组的方式创建，并在其中指定数组中元素的类型：

```python
arr1 = np.array([1,2,3], dtype=np.int8)
arr1
```

```python
# 输出结果为
arr1([1,2,3], dtype=int8)
```

​		**注意：在创建numpy数组对象时，数组对象中的元素都是统一的，如果对象中中的元素不统一时，对象中的元素则会按照int->float->str的类型从低到高依次转换**

对于ndarray数组来说，其是可以直接迭代的。

​		同时也可以使用元组对象创建ndarray数组。

#### 2.使用numpy函数创建ndarray数组

​		在numpy中用于创建数组的函数有：

-   np.ones(shape, dtpy = None, order=‘C’)

    -   该函数返回ndarray对象中的元素都是1
    -   shape表示创建ndarray的维度
        -   shape=5表示一个一维的数组
        -   shape=(3,3) 表示创建一个3*3二维的数组
        -   shape=(3,3,2) 表示创建一个3\*3\*2的三维数组(即3行2列的三维数组)
    -   dtype表示数据类型
        -   np.int/int8/int16/int32/float/float32……(numpy中的数据类型)
        -   str/int/float(Python 中的数据类型)
    -   order代表在内存中的存储顺序
        -   可选F或C
            -   F 多维数组中的列优先
            -   C多维数组中行优先

    ```python
    arr2 = np.ones([3,3])
    arr3
    输出结果：
    array([[ 1.,  1.,  1.],
           [ 1.,  1.,  1.],
           [ 1.,  1.,  1.]])
    np.ones([3,3],dtype=int)
    ```

-   np.zeros()与ones用法相同，不过该函数生成的数组全为0 

-   np.full(shape, fill_value, dtype=None, order=‘C’)

    -   fill_value 在数组中填充的内容

    ```python
    #  创建一个3*3的三维数组，数组中每一个元素都是9
    arr4 = np.full(shape=(3, 3), 9, dtype=np.int)
    arr4
    
    # 输出结果为
    
    array([[9, 9, 9],
           [9, 9, 9],
           [9, 9, 9]], dtype=int）
    ```

-   np.eye(N, M=None, k=0, dtype=float)

    -   n阶单位矩阵(对角线值为1，其他全为0) 
    -   N表示行数
    -   M表示列数，未指定时为N
    -   k=0表示ndarray数组按对角线填充1，>0时在对角线上方偏移k，<0时在对角线下方偏移k

#### 3.创建等差数列

-   np.linspace(start, stop, num=Number, endpoint=True, retstep=False, dtype=None)

    -   指定元素的个数生成一个等差数列的ndarray
    -   start开始的数，stop结束的数值
    -   endpoint默认为True，表示包含stop的值，为False时则不包含stop的值
    -   num表示生成元素的个数
    -   retstep默认未False，表示不显示等差间隔值，为True表示显示间隔值
    -   dtype表示数据类型

    ```python
    arr5 = np.linspace(2, 100, num=50, endpoint=True).astype(int)
    arr5
    # 结果：
    array([  2,   4,   6,   8,  10,  12,  14,  16,  18,  20,  22,  24,  26,
            28,  30,  32,  34,  36,  38,  40,  42,  44,  46,  48,  50,  52,
            54,  56,  58,  60,  62,  64,  66,  68,  70,  72,  74,  76,  78,
            80,  82,  84,  86,  88,  90,  92,  94,  96,  98, 100])
    ```

-   np.arange([start,]stop, [step,] dtype=None) 

    -   指定步长创建一个等差数列
    -   start表示开始的数值，默认为0
    -   step表示步长，默认为1

    ```python
    arr6 = np.arange(100, step=2) # 不包含stop值, 默认start是0
    arr6
    # 输出结果为：
    array([ 0,  2,  4,  6,  8, 10, 12, 14, 16, 18, 20, 22, 24, 26, 28, 30, 32,
           34, 36, 38, 40, 42, 44, 46, 48, 50, 52, 54, 56, 58, 60, 62, 64, 66,
           68, 70, 72, 74, 76, 78, 80, 82, 84, 86, 88, 90, 92, 94, 96, 98])
    ```

    ​	**注：使用该函数时，要注意，其包头不包尾**

#### 4.随机生成ndarray

-   np.random.random(size=None)

    -   生成0到1的随机小数，左闭右开区间[0.0,1.0)
    -   size为生成元素的个数，可以是int或是tuple类型，默认为1(即生成的ndarray为几维的)

-   np.random.randint(low， high=None，size=None， dtype=‘1)

    -   生成[low,high)区间的随机整数
    -   low最小数值(包含)，high最大值不包含，当high值为None时，随机生成的值在区间[0,low）之间
    -   size为元素个数，可以是int或tuple类型，默认为1

-   np.random.uniform()

    -   用法和randiint相同，但其生成的是小数

-   np.random.randn(0, d1, ..., dn)

    -   从标准正态分布中返回一个样本

    ```python
    np.random.randn(10)
    # 每次每次都不一样
    # 输出结果：
    array([-1.74976547,  0.3426804 ,  1.1530358 , -0.25243604,  0.98132079,
            0.51421884,  0.22117967, -1.07004333, -0.18949583,  0.25500144])
    np.random.randn(2, 3)
    # 输出结果：
    array([[-0.9491133,  0.40652333,  1.73164599],
           [-0.32518003, -0.43931223,  1.11204961]])
    ```

-   np.random.normal(loc=0.0, scale=1.0, size=None)

    -   从高斯分布中随机抽取一个样本
        -   高斯分布：$f(x)=\frac{1}{\sqrt{2\pi}}\exp(-\frac{(x-\mu)^2}{2\sigma^2})$
        -   标准正态分布 :$f(x) = \frac{1}{\sqrt{2\pi}}e(-\frac{x^2}{2})$
    -   可以控制期望值和方差变化
    -   loc为期望值，决定正态分布的位置
    -   scale方差，决定了分布的幅度
    -   size可以是int或tuple，表示元数个数

    **注：也可通过np.random.seed(seed=None)来设置随机种子，在指定随机种子之后，每一次随机产生的数都是一样的。**

### 三、ndarray的属性

-   ndim：维度
-   shape：形状(各维度的长度)
-   size:总长度
-   dtype: 元素类型

### 四、numpy的基本操作

#### 1.索引

```
d = np.random.randint(1, 10, size=10)
array([6, 5, 2, 7, 2, 5, 1, 1, 5, 6])

d[0] = 6
d[[0,2,3]] = array([6,2,7])
```

#### 2.切片

行切片：a[1: 2] —>表示切割出第二行的数据

列切片： a[:1, 1:2] ———>表示切割出第一行所在第二列的数据

#### 3.变形

numpy的变形操作可以通过`np.reshape(size)`函数进行变形，**注意：**size参数是一个tuple，并且在变形前后，ndarray对象的元素数量是一样的。

`np.reshape(-1,1)表示讲一个不知道行数的ndarray对象转化为一个多行一列的ndarray对象。例如：`

``````python
arr1 = np.random.randint(1, 100, size=(2,3))

arr1
Out[10]: 
array([[57, 50, 70],
       [32,  8, 54]])

arr1.reshape(-1,1)
Out[11]: 
array([[57],
       [50],
       [70],
       [32],
       [ 8],
       [54]])
``````

其他方式同理。

#### 4.转置

通过`transpose()/t`实现行列转置：

``````python
arr1.transpose()
Out[28]: 
array([[57, 32],
       [50,  8],
       [70, 54]])

arr1.T
Out[29]: 
array([[57, 32],
       [50,  8],
       [70, 54]])
``````

**numpy中实现数组的的转置操作实质上是对于行列式的转置T操作**

#### 5.级联

- `np.concatenate((a1, a2, a3, ...), axis=0)`级联的方向默认式shape中`tuple`的第一个值所代表的维度方向，可通过`axis`参数改变级联的方向

  ```python
  a1 = np.array([[1, 2], 
                 [3, 4]])
  
  a2 = np.array([[10, 20], 
                 [30, 40]])
  
  # axis=0表示垂直方向级联， 0表示最外层
  # axis=1表示水平方向级联， 1表示第二层， 
  #                       -1表示最内层，如果是二维数组时，-1和1是一样的
  np.concatenate((a1, a2), axis=-1)  
  ```

  ```python
  array([[ 1,  2, 10, 20],
         [ 3,  4, 30, 40]])
  ```

- `np.hstack((a1, a2, ...))`水平方向级联

  ```python
  np.hstack((a1, a2))
  
  
  array([[ 1,  2, 10, 20],
         [ 3,  4, 30, 40]])
  ```

- `np.vstack((a1, a2, ...))`垂直方向上级联

  ```python
  np.vstack((a1, a2))
  
  array([[ 1,  2],
         [ 3,  4],
         [10, 20],
         [30, 40]])
  ```

#### 6.切分

- `np.split(ary, indices_or_sections, axis=0)` 
  - `indices_or_sections`可以是int或者范围；如果是int类型，必须式可以被总行或总列整除，否则会报错
- `np.hsplit()` 等价于 axis=1 
- `np.vsplit()`等价于axis=0 

#### 7.聚合操作

`ndarry`的聚合操作主要有：

- 求和：`np.sum()`
- 最大值、最小值：`np.max()/np.min()`
- 平均：`np.mean()`

#### 8.矩阵积

`np.dot(A,B)`

在矩阵的积中分为点积运算和矩阵中元素的相乘。矩阵的点积运算是指通过如下的方式进行点积运算：

```python
import numpy as np

np.random.seed(1)
a = np.random.randint(1, 20, size=(2, 2))
b = np.arange(1, 20, 5, dtype=np.int).reshape(2,2)
c = b * a
m = b.reshape(-1, 1)
z = np.dot(b, a)
np.meshgrid()
print(a)
print(b)
print(z)
print(c)
```

```python
[[ 6 12]
 [13  9]]
[[ 1  6]
 [11 16]]
点积运算：
[[ 84  66]
 [274 276]]
元素的相乘：
[[  6  72]
 [143 144]]
```

以上演示的是两个二维数组之间的点积和相乘，在做矩阵乘法运算的时候，要注意相乘的点积之间之间的区别，在使用点积运算的时候两个矩阵中，第一个矩阵的列和第二个矩阵的行相同，最后的结果矩阵是一个第一个矩阵的行和第二个矩阵的列构成一个a×b的矩阵。

### 五、Numpy广播机制

​		广播是指在算数运算期间处理不同幸好在那个的数组的能力。数组的算数运算通常在相同的元素上完成，如果两个ndarry有两个完全相同的形状，那么这些操作将顺利执行。

产生广播的规则：

- 具有最小的ndmin的数组在其形状上预置为1
- 输出形状的每个维度的大小式该维度中输入大小的最大值
- 如果输入的大小和输出大小匹配或者其值恰好为1，则可以使用输入的值进行计算
- 如果输入的维度大小为1，则该维度中的第一个数据条目将用于沿该维度的所有计算

即：

- 数组的形状完全相同，数组具有相同的维度数量，每个维度的长度可以是常用长度或者是1
- 尺寸太小的阵列可能会将其形状预先设置为1的尺寸，使得上述熟悉为真

```Python
import numpy as np
a = np.array([[0, 0, 0],
              [10, 10, 10],
              [20, 20, 20],
              [30, 30, 30]])
b = np.array([1.0, 2.0, 3.0])
print('First ndarry')
print(a, '\n')
print('Second ndarray')
print(b, '\n')

print('a + b')
print(a + b)
```

```python
First ndarry
[[ 0  0  0]
 [10 10 10]
 [20 20 20]
 [30 30 30]] 

Second ndarray
[1. 2. 3.] 

a + b
[[ 1.  2.  3.]
 [11. 12. 13.]
 [21. 22. 23.]
 [31. 32. 33.]]
```

<font color='red'>注意：axis=1表示维度按行，axis=0表示维度是纵向的，当axis=-1时，表示的是维度的最里层</font>

#### 【附加】：

##### np.tile()

tile函数的主要功能就是将一个数组重复一定次数形成一个新的数组,但是无论如何,最后形成的一定还是一个数组

```python 
from numpy 	import *
a = [1,2,3]
b = tile(a,3)#numpy中的一个函数
b
array([1, 2, 3, 1, 2, 3, 1, 2, 3])

c = tile(a,(1,3)) #将a重复3次形成一行的数组
c
array([[1, 2, 3, 1, 2, 3, 1, 2, 3]])

d = tile(a,(2,3)) #将a重复3次形成2行的数组
d
array([[1, 2, 3, ..., 1, 2, 3],
       [1, 2, 3, ..., 1, 2, 3]])
```

b = tile(a,(m,n)):即是把a数组里面的元素复制n次放进一个数组c中，然后再把数组c复制m次放进一个数组b中

 tile函数的定义与说明
　　函数格式tile(A,reps)

　　A和reps都是array_like

　　A的类型众多，几乎所有类型都可以：array, list, tuple, dict, matrix以及基本数据类型int, string, float以及bool类型。

　　**reps的类型也很多，可以是tuple，list, dict, array, int, bool.但不可以是float, string, matrix类型。**

`percentile`

*a.ndim* --产看数组的维度

`np.shape用于查看数组的形状，对于一个a = np.shape[0]返回的是数据的一维的数据`

啊