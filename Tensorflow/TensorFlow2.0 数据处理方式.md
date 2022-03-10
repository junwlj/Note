[Toc]
## 1、使用tensorflow_datasets

tensorflow_datasets是一个非常有用的库，其中包含了很多数据集，通过运行：`tfds.list_builders()`可以查看其中包含的所有数据集。
在这里，使用猫狗数据集举例。

### 1.1 导入需要的库

```python
import os
import numpy as np
import tensorflow as tf
import matplotlib.pyplot as plt
import tensorflow_datasets as tfds
```

### 1.2 加载数据集

```python
(raw_train, raw_validation, raw_test), metadata = tfds.load(
    'cats_vs_dogs',
    split=['train[:80%]', 'train[80%:90%]', 'train[90%:]'],
    shuffle_files=False,
    batch_size=None,
    with_info=True,
    as_supervised=True,
)
```
上述函数说明：
输入：
- name：数据集的名称，可以通过运行tfds.list_builders()获得。
- split：如何划分数据集，如果不进行划分，则只得到训练集（即全部样本）。
- shuffle_files：是否打乱。
- batch_size：是否每次分批取出。如果为None，则每次取出一个样本，shape为三维；如果为一个大于1的数字，则每次取出多个样本，shape为四维；如果为1，每次取出一个样本，shape为四维（第一维为1）。
- with_info：是否输出数据集信息。
- as_supervised：为True时，函数会返回一个二元组 (input, label)，而不是返回 FeaturesDict。
输出：
- (raw_train, raw_validation, raw_test)：split之后的数据。
metadata：数据集信息。

### 1.3 查看数据集中某些样本的信息
```python
for image, label in raw_train.take(2):
    print(image.shape)
    print(label)
```
上述代码中，我们取出了两个训练样本的特征（图片）和标签，得到结果为：

```(262, 350, 3)
tf.Tensor(0, shape=(), dtype=int64)
(428, 500, 3)
tf.Tensor(1, shape=(), dtype=int64)
```

由此可见，此数据集中的图片大小是不一致的。如果我们想知道标签所代表的种类（猫or狗?）我们可以通过以下代码查看：

```Python
get_label_name = metadata.features['label'].int2str
for image, label in raw_train.take(2):
    print(image.shape)
    print(label)
    print(get_label_name(label))
```
此时会输出：
```
(262, 350, 3)
tf.Tensor(0, shape=(), dtype=int64)
cat
(428, 500, 3)
tf.Tensor(1, shape=(), dtype=int64)
dog
```
### 1.4 将样本标准化
IMG_SIZE = 160 # All images will be resized to 160x160

```Python
def format_example(image, label):
    image = tf.cast(image, tf.float32)
    image = (image/127.5) - 1
    image = tf.image.resize(image, (IMG_SIZE, IMG_SIZE))
    return image, label
train = raw_train.map(format_example)
validation = raw_validation.map(format_example)
test = raw_test.map(format_example)
```

当然，这里也可以用下面的代码代替：

```python
for image, label in raw_train:
    image = tf.cast(image, tf.float32)
    image = (image/127.5) - 1
    image = tf.image.resize(image, (IMG_SIZE, IMG_SIZE))
```
但这将会非常花时间！！！

### 1.5 将样本打乱、分批
如果在导入数据集的时候没有shuffle和分批，那么可以在之后进行。

BATCH_SIZE = 32
SHUFFLE_BUFFER_SIZE = 1000
train_batches = train.shuffle(SHUFFLE_BUFFER_SIZE).batch(BATCH_SIZE)
validation_batches = validation.batch(BATCH_SIZE)
test_batches = test.batch(BATCH_SIZE)
1
2
3
4
5
1.6 查看最终的训练样本
至此，通过运行

for image_batch, label_batch in train_batches.take(1):
    print(image_batch.shape)
    print(label_batch.shape)
1
2
3
我们可以得到：

(32, 160, 160, 3)
(32,)
1
2
将此输入模型，即可进行训练。

2、将已有的csv文件作为数据集
在这里，使用鸢尾花数据集举例。
首先，先下载鸢尾花数据集。

train_dataset_url = "https://storage.googleapis.com/download.tensorflow.org/data/iris_training.csv"
train_dataset_fp = tf.keras.utils.get_file(fname=os.path.basename(train_dataset_url),
                                          origin=train_dataset_url)
print(train_dataset_fp)
1
2
3
4
train_dataset_fp即鸢尾花数据集在电脑上的地址。

2.1 将数据从csv文件中取出
在这里，有两种方法查看csv文件中的数据，一是使用Pandas库，二是使用numpy库。

2.1.1 用Pandas库查看数据
features = pd.read_csv(train_dataset_fp)
print(features.head())
dataset_ = features.values
1
2
3
可以查看前五行数据为：


2.1.2 用numpy库查看数据
dataset_ = np.loadtxt(open(train_dataset_fp), skiprows=1, delimiter=",")
1
2.2 数据标准化
data_mean = dataset_.mean(axis=0)
data_std = dataset_.std(axis=0)

dataset_ = (dataset_-data_mean)/data_std
1
2
3
4
2.3 划分训练集和测试集
因为这个数据集本身不分训练集和测试集，所以在这里要用sklearn库进行划分。

from sklearn.model_selection import train_test_split
train, test = train_test_split(dataset_, test_size=0.2)
1
2
2.4 划分特征与标签
train_x = train[:, :-1].astype(np.float32)
train_y = train[:, -1].astype(np.float32)
test_x = test[:, :-1].astype(np.float32)
test_y = test[:, -1].astype(np.float32)
1
2
3
4
2.5 切片处理
dataset_train = tf.data.Dataset.from_tensor_slices((train_x, train_y)).shuffle(train_y.shape[0]).batch(32)
dataset_test = tf.data.Dataset.from_tensor_slices((test_x, test_y)).shuffle(test_y.shape[0]).batch(32)
1
2
将此输入模型，即可进行训练。

3、使用tf.keras.datasets
3.1导入数据集
(x, y), (x_test, y_test) = tf.keras.datasets.fashion_mnist.load_data()
1
3.2 特征归一化
因为这里特征是图片，所以除以255即可。

def preprocess(x, y):

    x = tf.cast(x, dtype=tf.float32) / 255.
    y = tf.cast(y, dtype=tf.int32)
    return x,y
1
2
3
4
5
3.3 切片
batchsz = 128

db = tf.data.Dataset.from_tensor_slices((x,y))
db = db.map(preprocess).shuffle(10000).batch(batchsz)

db_test = tf.data.Dataset.from_tensor_slices((x_test,y_test))
db_test = db_test.map(preprocess).batch(batchsz)
1
2
3
4
5
6
7
将此输入模型，即可进行训练。

4、使用tf.feature_column（主要针对结构化数据）
在这里，我们使用心脏病数据集进行举例。
数据集中有数值（numeric）和类别（categorical）类型的列，如下图所示：


4.1 导入需要的库
import numpy as np
import pandas as pd
import tensorflow as tf
from tensorflow import feature_column
from tensorflow.keras import layers
from sklearn.model_selection import train_test_split
1
2
3
4
5
6
4.2 导入数据集
URL = 'https://storage.googleapis.com/applied-dl/heart.csv'
dataframe = pd.read_csv(URL)
dataframe.head()
1
2
3


4.3 划分训练集、测试集和验证集
train, test = train_test_split(dataframe, test_size=0.2)
train, val = train_test_split(train, test_size=0.2)
print(len(train), 'train examples')
print(len(val), 'validation examples')
print(len(test), 'test examples')
1
2
3
4
5
193 train examples
49 validation examples
61 test examples
1
2
3
4.4 定义从 Pandas Dataframe 创建 tf.data 数据集的函数
def df_to_dataset(dataframe, shuffle=True, batch_size=32):
    dataframe = dataframe.copy()
    labels = dataframe.pop('target')
    ds = tf.data.Dataset.from_tensor_slices((dict(dataframe), labels))
    if shuffle:
        ds = ds.shuffle(buffer_size=len(dataframe))
    ds = ds.batch(batch_size)
    return ds
1
2
3
4
5
6
7
8
4.5 创建 tf.data 数据集
batch_size = 5 # 小批量大小用于演示
train_ds = df_to_dataset(train, batch_size=batch_size)
val_ds = df_to_dataset(val, shuffle=False, batch_size=batch_size)
test_ds = df_to_dataset(test, shuffle=False, batch_size=batch_size)
1
2
3
4
此处返回的皆为字典形式。
可以通过以下方式查看数据集信息：

for feature_batch, label_batch in train_ds.take(1):
    print('Every feature:', list(feature_batch.keys()))
    print('A batch of ages:', feature_batch['age'])
    print('A batch of targets:', label_batch )
1
2
3
4
Every feature: ['age', 'sex', 'cp', 'trestbps', 'chol', 'fbs', 'restecg', 'thalach', 'exang', 'oldpeak', 'slope', 'ca', 'thal']
A batch of ages: tf.Tensor([61 59 58 42 40], shape=(5,), dtype=int32)
A batch of targets: tf.Tensor([1 1 0 1 0], shape=(5,), dtype=int32)
1
2
3
4.6 按照类别转换数据
feature_columns = []

# 数值列
for header in ['age', 'trestbps', 'chol', 'thalach', 'oldpeak', 'slope', 'ca']:
    feature_columns.append(feature_column.numeric_column(header))

# 分桶列
age = feature_column.numeric_column("age")
age_buckets = feature_column.bucketized_column(age, boundaries=[18, 25, 30, 35, 40, 45, 50, 55, 60, 65])
feature_columns.append(age_buckets)

# 分类列
thal = feature_column.categorical_column_with_vocabulary_list(
      'thal', ['fixed', 'normal', 'reversible'])
thal_one_hot = feature_column.indicator_column(thal)
feature_columns.append(thal_one_hot)

# 嵌入列
thal_embedding = feature_column.embedding_column(thal, dimension=8)
feature_columns.append(thal_embedding)

# 组合列
crossed_feature = feature_column.crossed_column([age_buckets, thal], hash_bucket_size=1000)
crossed_feature = feature_column.indicator_column(crossed_feature)
feature_columns.append(crossed_feature)
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
数值列
数值列（numeric column） 是最简单的列类型。它用于表示实数特征。使用此列时，模型将从 dataframe 中接收未更改的列值。
用‘age’列举例：

age_column = feature_column.numeric_column('age')
for x, y in train_ds.take(1):
    print(tf.keras.layers.DenseFeatures(age_column)(x).numpy())
1
2
3
[[66.]
 [39.]
 [70.]
 [48.]
 [63.]]
1
2
3
4
5
可见数值列并不发生变化。

分桶列
如果不希望将数字直接输入模型，而是根据数值范围将其值分成不同的类别。考虑代表一个人年龄的原始数据。我们可以用 分桶列（bucketized column）将年龄分成几个分桶（buckets），而不是将年龄表示成数值列。

age_column = feature_column.numeric_column('age')
age_buckets = feature_column.bucketized_column(age_column, boundaries=[18, 25, 30, 35, 40, 45, 50, 55, 60, 65])
for x, y in train_ds.take(1):
    print(tf.keras.layers.DenseFeatures(age_buckets)(x).numpy())
1
2
3
4
[[0. 0. 0. 0. 0. 1. 0. 0. 0. 0. 0.]
 [0. 0. 0. 0. 0. 1. 0. 0. 0. 0. 0.]
 [0. 0. 0. 0. 0. 0. 0. 0. 1. 0. 0.]
 [0. 0. 0. 0. 0. 0. 0. 1. 0. 0. 0.]
 [0. 0. 0. 0. 0. 0. 1. 0. 0. 0. 0.]]
1
2
3
4
5
分类列
在此数据集中，thal 用字符串表示（如 ‘fixed’，‘normal’，或 ‘reversible’）。我们无法直接将字符串提供给模型。相反，我们必须首先将它们映射到数值。分类词汇列（categorical vocabulary columns）提供了一种用 one-hot 向量表示字符串的方法（就像您在上面看到的年龄分桶一样）。词汇表可以用 categorical_column_with_vocabulary_list 作为 list 传递，或者用 categorical_column_with_vocabulary_file 从文件中加载。

thal = feature_column.categorical_column_with_vocabulary_list(
      'thal', ['fixed', 'normal', 'reversible'])
thal_one_hot = feature_column.indicator_column(thal)
for x, y in train_ds.take(1):
    print(tf.keras.layers.DenseFeatures(thal_one_hot)(x).numpy())
1
2
3
4
5
[[0. 0. 1.]
 [0. 1. 0.]
 [0. 1. 0.]
 [0. 0. 1.]
 [0. 1. 0.]]
1
2
3
4
5
嵌入列
假设我们不是只有几个可能的字符串，而是每个类别有数千（或更多）值。 由于多种原因，随着类别数量的增加，使用 one-hot 编码训练神经网络变得不可行。我们可以使用嵌入列来克服此限制。嵌入列（embedding column）将数据表示为一个低维度密集向量，而非多维的 one-hot 向量，该低维度密集向量可以包含任何数，而不仅仅是 0 或 1。嵌入的大小（在下面的示例中为 8）是必须调整的参数。

thal_embedding = feature_column.embedding_column(thal, dimension=8)
for x, y in train_ds.take(1):
    print(tf.keras.layers.DenseFeatures(thal_embedding)(x).numpy())
1
2
3
[[ 0.36323512 -0.10599072 -0.16521429 -0.44111866  0.39538452  0.25446087
  -0.56295955 -0.1078408 ]
 [ 0.36323512 -0.10599072 -0.16521429 -0.44111866  0.39538452  0.25446087
  -0.56295955 -0.1078408 ]
 [-0.1679268  -0.14216028  0.52936536  0.34576175 -0.10905012 -0.09870762
   0.15268394 -0.40206134]
 [ 0.36323512 -0.10599072 -0.16521429 -0.44111866  0.39538452  0.25446087
  -0.56295955 -0.1078408 ]
 [ 0.36323512 -0.10599072 -0.16521429 -0.44111866  0.39538452  0.25446087
  -0.56295955 -0.1078408 ]]
1
2
3
4
5
6
7
8
9
10
组合列
将多种特征组合到一个特征中，称为特征组合（feature crosses），它让模型能够为每种特征组合学习单独的权重。此处，我们将创建一个 age 和 thal 组合的新特征。

crossed_feature = feature_column.crossed_column([age_buckets, thal], hash_bucket_size=5)
crossed_feature = feature_column.indicator_column(crossed_feature)
for x, y in train_ds.take(1):
    print(tf.keras.layers.DenseFeatures(crossed_feature)(x).numpy())
1
2
3
4
[[0. 0. 0. 0. 1.]
 [0. 0. 0. 1. 0.]
 [0. 1. 0. 0. 0.]
 [0. 0. 1. 0. 0.]
 [0. 1. 0. 0. 0.]]
1
2
3
4
5
维数通过调整hash_bucket_size来改变。

4.7 建立一个新的特征层
现在我们已经定义了我们的特征列，我们将使用密集特征（DenseFeatures）层将特征列输入到我们的 Keras 模型中。

feature_layer = tf.keras.layers.DenseFeatures(feature_columns)
1
本来在这一步之后就属于模型建立方面的了，但是在建模时我们需要将feature_layer作为一层写入模型，如4.8所示。

4.8 建模
model = tf.keras.Sequential([
  feature_layer,
  layers.Dense(128, activation='relu'),
  layers.Dense(128, activation='relu'),
  layers.Dense(1, activation='sigmoid')
])

model.compile(optimizer='adam',
              loss='binary_crossentropy',
              metrics=['accuracy'],
              run_eagerly=True)

model.fit(train_ds,
          validation_data=val_ds,
          epochs=5)

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
5、直接从文件夹中读取图片
我们用horse2zebra数据集举例：此数据集中包含4个文件夹，分别是horse训练集、zebra训练集、horse测试集以及zebra测试集。每个训练集中都包含1000多张 (256, 256, 3) 的彩色图片（掺有一些灰度图片，之后会在代码中删掉）。

5.1 将图片导入
PATH = 'C:\\Users\\ThinkPad\\.keras\\datasets\\horse2zebra/'
train_horses = tf.data.Dataset.list_files(PATH+'trainA/*.jpg')
train_zebras = tf.data.Dataset.list_files(PATH+'trainB/*.jpg')
test_horses = tf.data.Dataset.list_files(PATH+'testA/*.jpg')
test_zebras = tf.data.Dataset.list_files(PATH+'testB/*.jpg')
1
2
3
4
5
此时导入的是字符串类型的dataset。

5.2 将图片转换为需要的类型
def load(image_file):
    image = tf.io.read_file(image_file)
    image = tf.image.decode_jpeg(image)
    image = tf.cast(image, tf.float32)

    return image
1
2
3
4
5
6
打印出一张图片查看：

img = load(PATH+'trainB/n02391049_2.jpg')
# casting to int for matplotlib to show the image
plt.figure()
plt.imshow(img/255.0)
1
2
3
4


5.3 删除dataset中的灰度图
for dirname, _, filenames in os.walk(PATH+'trainB'):
    for filename in filenames:
        img = load(os.path.join(dirname, filename))
        if img.shape != (256, 256, 3):
            print(filename)
            print(img.shape)
            os.remove(os.path.join(dirname, filename))
1
2
3
4
5
6
7
5.4 加入batch和shuffle
AUTOTUNE = tf.data.experimental.AUTOTUNE
train_horses = train_horses.map(
    load, num_parallel_calls=AUTOTUNE).cache().shuffle(
    1000).batch(1)

train_zebras = train_zebras.map(
    load, num_parallel_calls=AUTOTUNE).cache().shuffle(
    1000).batch(1)

test_horses = test_horses.map(
    load, num_parallel_calls=AUTOTUNE).cache().shuffle(
    1000).batch(1)

test_zebras = test_zebras.map(
    load, num_parallel_calls=AUTOTUNE).cache().shuffle(
    1000).batch(1)
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
将此输入模型，即可进行训练。

6、使用 wget.download 在官网下载数据集
以热狗数据集举例。

6.1 去官网下载数据集
import os
import wget
data = os.getcwd()+'/data'
base_url = 'https://apache-mxnet.s3-accelerate.amazonaws.com/'
wget.download(
    base_url + 'gluon/dataset/hotdog.zip',
    data)
1
2
3
4
5
6
7
其中，os.getcwd() 返回的是当前 .py 文件所在的文件夹。wget.download(data, dir) 是将 data 数据集（压缩包）下载到 dir 文件夹中。

6.2 解压数据集压缩包
import zipfile
with zipfile.ZipFile('data', 'r') as z:
	z.extractall(os.getcwd())
1
2
3
6.3 读取图像文件
创建两个 tf.keras.preprocessing.image.ImageDataGenerator 实例来分别读取训练数据集和测试数据集中的所有图像文件。 这里我们将训练集图片全部处理为高和宽均为224像素的输入。此外，我们对RGB（红、绿、蓝）三个颜色通道的数值做标准化。

import pathlib
train_dir = 'hotdog/train'
test_dir = 'hotdog/test'
train_dir = pathlib.Path(train_dir)
train_count = len(list(train_dir.glob('*/*.jpg')))
test_dir = pathlib.Path(test_dir)
test_count = len(list(test_dir.glob('*/*.jpg')))

CLASS_NAMES = np.array([item.name for item in train_dir.glob('*') if item.name != 'LICENSE.txt' and item.name[0] != '.'])
CLASS_NAMES

image_generator = tf.keras.preprocessing.image.ImageDataGenerator(rescale=1./255)  # 标准化
BATCH_SIZE = 32
IMG_HEIGHT = 224
IMG_WIDTH = 224

train_data_gen = image_generator.flow_from_directory(directory=str(train_dir),
                                                    batch_size=BATCH_SIZE,
                                                    target_size=(IMG_HEIGHT, IMG_WIDTH),
                                                    shuffle=True,
                                                    classes = list(CLASS_NAMES))

test_data_gen = image_generator.flow_from_directory(directory=str(test_dir),
                                                    batch_size=BATCH_SIZE,
                                                    target_size=(IMG_HEIGHT, IMG_WIDTH),
                                                    shuffle=True,
                                                    classes = list(CLASS_NAMES))
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
7、导入文本（用于文本分类）
对于文本分类的问题，我们一般需要将每个单词对应一个数字，而对于文本生成的问题，我们一般需要将每个字母对应一个数字。

在这里，我们将使用 tf.data.TextLineDataset 来加载文本文件。TextLineDataset 通常被用来以文本文件构建数据集（文件中的一行为一个样本) 。这适用于大多数的基于行的文本数据（例如，诗歌、小说或错误日志) 。

7.1 导入需要的库
import tensorflow as tf

import tensorflow_datasets as tfds
import os
1
2
3
4
7.2 得到文本所在目录
7.2.1 下载数据集
如果是自己的数据集，这一步可以跳过。

在这里，我们将使用相同作品（荷马的伊利亚特）的三个不同版本的英文翻译举例，以文本的每一行作为样本特征，以作者为标签。

DIRECTORY_URL = 'https://storage.googleapis.com/download.tensorflow.org/data/illiad/'
FILE_NAMES = ['cowper.txt', 'derby.txt', 'butler.txt']

for name in FILE_NAMES:
    text_dir = tf.keras.utils.get_file(name, origin=DIRECTORY_URL+name)
1
2
3
4
5
7.2.2 查找目录地址
parent_dir = os.path.dirname(text_dir)
parent_dir
1
2
7.3 生成 dataset
7.3.1 为每个类别的样本都单独生成一个数据集
def labeler(example, index):
    return example, tf.cast(index, tf.int64)

labeled_data_sets = []

for i, file_name in enumerate(FILE_NAMES):
    lines_dataset = tf.data.TextLineDataset(os.path.join(parent_dir, file_name))
    labeled_dataset = lines_dataset.map(lambda ex: labeler(ex, i))
    labeled_data_sets.append(labeled_dataset)
1
2
3
4
5
6
7
8
9
其中，我们首先使用了函数 tf.data.TextLineDataset()：这个函数的输入是一个文件地址，输出是一个 dataset。dataset 中的每一个元素就对应了文件中的一行。
比如：

a = tf.data.TextLineDataset(os.path.join(parent_dir, 'cowper.txt'))
for each in a.take(5):
    print(each)
1
2
3
tf.Tensor(b"\xef\xbb\xbfAchilles sing, O Goddess! Peleus' son;", shape=(), dtype=string)
tf.Tensor(b'His wrath pernicious, who ten thousand woes', shape=(), dtype=string)
tf.Tensor(b"Caused to Achaia's host, sent many a soul", shape=(), dtype=string)
tf.Tensor(b'Illustrious into Ades premature,', shape=(), dtype=string)
tf.Tensor(b'And Heroes gave (so stood the will of Jove)', shape=(), dtype=string)
1
2
3
4
5
然后我们将得到的 dataset 映射到 labeler 函数中，将标签添加到 dataset 中：

b = a.map(lambda ex: labeler(ex, 0))
for each in b.take(5):
    print(each)
1
2
3
(<tf.Tensor: id=95344, shape=(), dtype=string, numpy=b"\xef\xbb\xbfAchilles sing, O Goddess! Peleus' son;">, <tf.Tensor: id=95345, shape=(), dtype=int64, numpy=0>)
(<tf.Tensor: id=95346, shape=(), dtype=string, numpy=b'His wrath pernicious, who ten thousand woes'>, <tf.Tensor: id=95347, shape=(), dtype=int64, numpy=0>)
(<tf.Tensor: id=95348, shape=(), dtype=string, numpy=b"Caused to Achaia's host, sent many a soul">, <tf.Tensor: id=95349, shape=(), dtype=int64, numpy=0>)
(<tf.Tensor: id=95350, shape=(), dtype=string, numpy=b'Illustrious into Ades premature,'>, <tf.Tensor: id=95351, shape=(), dtype=int64, numpy=0>)
(<tf.Tensor: id=95352, shape=(), dtype=string, numpy=b'And Heroes gave (so stood the will of Jove)'>, <tf.Tensor: id=95353, shape=(), dtype=int64, numpy=0>)
1
2
3
4
5
7.3.2 将三个 dataset 合并成一个 dataset
all_labeled_data = labeled_data_sets[0]
for labeled_dataset in labeled_data_sets[1:]:
    all_labeled_data = all_labeled_data.concatenate(labeled_dataset)
1
2
3
7.3.3 将 dataset 打乱
BUFFER_SIZE = 50000
all_labeled_data = all_labeled_data.shuffle(BUFFER_SIZE, reshuffle_each_iteration=False)
1
2
我们可以打印 dataset 中前5个元素：

for ex in all_labeled_data.take(5):
    print(ex)
1
2
(<tf.Tensor: id=95461, shape=(), dtype=string, numpy=b"Uprear'd, a wonder even in eyes divine.">, <tf.Tensor: id=95462, shape=(), dtype=int64, numpy=0>)
(<tf.Tensor: id=95463, shape=(), dtype=string, numpy=b'hecatombs, but to the daughter of great Jove alone he had made no'>, <tf.Tensor: id=95464, shape=(), dtype=int64, numpy=2>)
(<tf.Tensor: id=95465, shape=(), dtype=string, numpy=b'strode onward. The Argives were elated as they beheld him, but the'>, <tf.Tensor: id=95466, shape=(), dtype=int64, numpy=2>)
(<tf.Tensor: id=95467, shape=(), dtype=string, numpy=b'"Friends, Grecian Heroes, Ministers of Mars,'>, <tf.Tensor: id=95468, shape=(), dtype=int64, numpy=1>)
(<tf.Tensor: id=95469, shape=(), dtype=string, numpy=b'sin against their oaths--of them and their children--may be shed upon'>, <tf.Tensor: id=95470, shape=(), dtype=int64, numpy=2>)
1
2
3
4
5
可见此三个类别的样本都已经包含在 dataset 中了。

7.4 将文本编码成数字形式
7.4.1 建立词汇表并统计词汇表中的单词数量
tokenizer = tfds.features.text.Tokenizer()

vocabulary_set = set()
for text_tensor, _ in all_labeled_data:
    some_tokens = tokenizer.tokenize(text_tensor.numpy())
    vocabulary_set.update(some_tokens)

vocab_size = len(vocabulary_set)
vocab_size
1
2
3
4
5
6
7
8
9
16913
1
其中 tokenizer = tfds.features.text.Tokenizer() 的目的是实例化一个分词器，tokenizer.tokenize 可以将一句话分成多个单词，例如：

for text_tensor, _ in all_labeled_data.take(1):
    print(text_tensor)
    print(text_tensor.numpy())
	print(tokenizer.tokenize(text_tensor.numpy()))
1
2
3
4
tf.Tensor(b"Uprear'd, a wonder even in eyes divine.", shape=(), dtype=string)
b"Uprear'd, a wonder even in eyes divine."
['Uprear', 'd', 'a', 'wonder', 'even', 'in', 'eyes', 'divine']
1
2
3
7.4.2 建立编码器
encoder = tfds.features.text.TokenTextEncoder(vocabulary_set)
1
我们可以拿一个样本实验：

example_text = next(iter(all_labeled_data))[0].numpy()
print(example_text)

encoded_example = encoder.encode(example_text)
print(encoded_example)
1
2
3
4
5
b'I mean to pound his flesh, and smash his bones.'
[1677, 9644, 1762, 15465, 12945, 9225, 13806, 5555, 12945, 4829]
1
2
然后，我们将编码器写成函数供以后调用：

def encode(text_tensor, label):
    encoded_text = encoder.encode(text_tensor.numpy())
    return encoded_text, label
1
2
3
7.4.3 对所有样本进行编码
def encode_map_fn(text, label):
    # py_func doesn't set the shape of the returned tensors.
    encoded_text, label = tf.py_function(encode,
                                       inp=[text, label],
                                       Tout=(tf.int64, tf.int64))

    # `tf.data.Datasets` work best if all components have a shape set
    #  so set the shapes manually:
    encoded_text.set_shape([None])
    label.set_shape([])

    return encoded_text, label


all_encoded_data = all_labeled_data.map(encode_map_fn)
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
其中，我们使用了 tf.py_function(func, inp, Tout, name=None) 函数：

作用：包装 Python 函数，让 Python 代码可以与 tensorflow 进行交互。

参数：

  func ：自己定义的python函数名称

  inp ：自己定义python函数的参数列表，写成列表的形式，[tensor1,tensor2,tensor3] 列表的每一个元素是一个Tensor对象，

  Tout：它与自定义的python函数的返回值相对应的，

      当Tout是一个列表的时候 ，如 [ tf.string,tf,int64,tf.float] 表示自定义函数有三个返回值，即返回三个tensor，每一个tensor的元素的类型与之对应；
      当Tout只有一个值的时候，如tf.int64，表示自定义函数返回的是一个整型列表或整型tensor；
      当Tout没有值的时候，表示自定义函数没有返回值。
1
2
3
4
5
6
7
8
9
注意：如果这里不使用 tf.py_function 而是使用 dataset.map，程序会报错：

AttributeError: 'Tensor' object has no attribute 'numpy'
1
这是因为 datastep.map(function) 给解析函数 function 传递进去的参数，即上面的 encode(text_tensor, label) 中的 text_tensor 和 label 是 Tensor 而不是 EagerTensor 。可以这样理解：

因为对一个数据集 dataset.map 的时候，并没有预先对每一组样本先进行 map 中映射的函数运算，而仅仅是告诉 dataset，你每一次拿出来的样本时要先进行一遍 function 运算之后才使用的，所以 function 的调用是在每次迭代 dataset 的时候才调用的，但是预先的参数 text_tensor 和 label 只是一个“容器”，迭代的时候采用数据将这个“容器”填起来，而在运算的时候，虽然将数据填进去了，但是 text_tensor 和 label 依然还是一个 Tensor 而不是 EagerTensor，所以才会出现上面的问题。

此时，我们得到的最终 dataset 中的样本已经从文本转换成了数字向量：

for i in all_encoded_data.take(5):
    print(i)
1
2
(<tf.Tensor: id=225383, shape=(8,), dtype=int64, numpy=
array([ 1438, 14227,  5791, 16819, 11806, 13990, 10168, 11243],
      dtype=int64)>, <tf.Tensor: id=225384, shape=(), dtype=int64, numpy=0>)
(<tf.Tensor: id=225388, shape=(13,), dtype=int64, numpy=
array([ 3194,  4566,  1762, 15273,  9726,   377,  5972,   556, 11565,
       13400,  5594,  5132,  9271], dtype=int64)>, <tf.Tensor: id=225389, shape=(), dtype=int64, numpy=2>)
(<tf.Tensor: id=225393, shape=(12,), dtype=int64, numpy=
array([ 9549,  7697, 12367,   901,  7439,  4679,  3366, 11629,  5709,
        4866,  4566, 15273], dtype=int64)>, <tf.Tensor: id=225394, shape=(), dtype=int64, numpy=2>)
(<tf.Tensor: id=225398, shape=(6,), dtype=int64, numpy=array([   88, 12816, 14312,  7786,   377, 10566], dtype=int64)>, <tf.Tensor: id=225399, shape=(), dtype=int64, numpy=1>)
(<tf.Tensor: id=225403, shape=(13,), dtype=int64, numpy=
array([ 7888,  8908,  2313, 13645,   377, 12262, 13806,  2313,  7788,
        3289,  1718, 12822,  2595], dtype=int64)>, <tf.Tensor: id=225404, shape=(), dtype=int64, numpy=2>)
1
2
3
4
5
6
7
8
9
10
11
12
13
7.5 将数据集分割为测试集和训练集
BATCH_SIZE = 64
TAKE_SIZE = 5000
train_data = all_encoded_data.skip(TAKE_SIZE).shuffle(BUFFER_SIZE)
train_data = train_data.padded_batch(BATCH_SIZE, ((None, ), ()))

test_data = all_encoded_data.take(TAKE_SIZE)
test_data = test_data.padded_batch(BATCH_SIZE, ((None, ), ()))
1
2
3
4
5
6
7
使用 tf.data.Dataset.take 和 tf.data.Dataset.skip 来建立一个小一些的测试数据集和稍大一些的训练数据集。tf.data.Dataset.take(TAKE_SIZE) 表示取 TAKE_SIZE 个样本做测试集；tf.data.Dataset.skip(TAKE_SIZE) 表示取 总样本数-TAKE_SIZE 个样本做训练集。

在数据集被传入模型之前，数据集需要进行分批处理。最典型的是，每个批次中的样本大小与格式需要一致。但是数据集中样本并不全是相同大小的（每行文本字数并不相同）。因此，我们使用 tf.data.Dataset.padded_batch（而不是 batch ）将样本填充到相同的大小，这里把形状设置成 (None, ) 之后，它会判断在这个批次中的最长样本的单词个数，然后将该批次所有其他样本用零填充到这个长度。

sample_text, sample_labels = next(iter(test_data))
sample_text[0], sample_labels[0]
1
2
(<tf.Tensor: id=225755, shape=(15,), dtype=int64, numpy=
 array([ 1438, 14227,  5791, 16819, 11806, 13990, 10168, 11243,     0,
            0,     0,     0,     0,     0,     0], dtype=int64)>,
 <tf.Tensor: id=225759, shape=(), dtype=int64, numpy=0>)
1
2
3
4
由于我们引入了一个新的 token 来编码（填充零），因此词汇表大小增加了一个。

vocab_size += 1
1
之后在训练的时候，直接将 train_data 输入词嵌入层即可。训练的详细信息请参照Tensorflow2.0之文本分类确定文章译者。

8、导入文本（用于文本生成）
对于文本分类的问题，我们一般需要将每个单词对应一个数字，但而对于文本生成的问题，我们一般需要将每个字母对应一个数字。

8.1 导入需要的库
import tensorflow as tf

import numpy as np
import os
import time
1
2
3
4
5
8.2 导入数据
8.2.1 得到数据集地址
path_to_file = tf.keras.utils.get_file('shakespeare.txt', 'https://storage.googleapis.com/download.tensorflow.org/data/shakespeare.txt')
path_to_file
1
2
'C:\\Users\\ThinkPad\\.keras\\datasets\\shakespeare.txt'
1
8.2.2 读取数据
# 读取并解码
text = open(path_to_file, 'rb').read().decode(encoding='utf-8')
print ('Length of text: {} characters'.format(len(text)))
1
2
3
Length of text: 1115394 characters
1
在文本分类的问题中，文本长度是指文本中的字符（字母、数字、标点符号、换行符、空格）个数，这里共有 1115394 个。

我们可以尝试打印前10个字母：

print(text[:10])
1
First Citi
1
8.3 处理数据
8.3.1 提取文本中的非重复字符
vocab = sorted(set(text))
print ('{} unique characters'.format(len(vocab)))
1
2
这里 set() 函数的作用是去掉文本中重复的字符，sort() 函数对得到的字符进行排序，举个例子：

x = ['a', 'c', 'b', 'a', 'd']
y = set(x)
z = sorted(set(x))
print(y)
print(z)
1
2
3
4
5
{'b', 'd', 'c', 'a'}
['a', 'b', 'c', 'd']
1
2
8.3.2 创建从非重复字符到索引的映射
char2idx = {u:i for i, u in enumerate(vocab)}
idx2char = np.array(vocab)
# 将文本映射到数字向量
text_as_int = np.array([char2idx[c] for c in text])
1
2
3
4
char2idx：一个字典，将字符映射到数字索引。
idx2char：一个数组，将数字索引映射回字符。

打印 char2idx 的前五个元素：

for char,_ in zip(char2idx, range(5)):
    print('{}: {},'.format(repr(char), char2idx[char]))
1
2
'\n': 0,
' ': 1,
'!': 2,
'$': 3,
'&': 4,
1
2
3
4
5
这里我们使用了 repr() 函数，它的作用是将输入转化为字符串的形式，如：

print('没有repr：'+'\n')
print('有repr：'+repr('\n'))
1
2
没有repr：

有repr：'\n'
1
2
3
8.4 创建训练样本 / 目标
8.4.1 创建 tf.data
char_dataset = tf.data.Dataset.from_tensor_slices(text_as_int)
1
打印前五个样本为：

for i in char_dataset.take(5):
    print(idx2char[i.numpy()])
1
2
F
i
r
s
t
1
2
3
4
5
这里的每个样本进包含一个字符，但我们希望一个样本中输出多个字符，此时，我们可以使用 dataset.batch 函数。如果我们希望每个样本包含100个字符，那么我们需要让 dataset 每次输出101个字符，这样一来，在创建训练样本时，我们需要其中前100个字符，创建训练目标时，我们需要其中后100个字符。

seq_length = 100
examples_per_epoch = len(text)//seq_length
sequences = char_dataset.batch(seq_length+1, drop_remainder=True)

for item in sequences.take(5):
    print(repr(''.join(idx2char[item.numpy()])))
1
2
3
4
5
6
'First Citizen:\nBefore we proceed any further, hear me speak.\n\nAll:\nSpeak, speak.\n\nFirst Citizen:\nYou '
'are all resolved rather to die than to famish?\n\nAll:\nResolved. resolved.\n\nFirst Citizen:\nFirst, you k'
"now Caius Marcius is chief enemy to the people.\n\nAll:\nWe know't, we know't.\n\nFirst Citizen:\nLet us ki"
"ll him, and we'll have corn at our own price.\nIs't a verdict?\n\nAll:\nNo more talking on't; let it be d"
'one: away, away!\n\nSecond Citizen:\nOne word, good citizens.\n\nFirst Citizen:\nWe are accounted poor citi'
1
2
3
4
5
8.4.2 划分样本和目标
def split_input_target(chunk):
    input_text = chunk[:-1]
    target_text = chunk[1:]
    return input_text, target_text

dataset = sequences.map(split_input_target)
1
2
3
4
5
6
查看 dataset中的一个样本：

for input_example, target_example in  dataset.take(1):
    print ('Input data: ', repr(''.join(idx2char[input_example.numpy()])))
    print ('Target data:', repr(''.join(idx2char[target_example.numpy()])))
1
2
3
Input data:  'First Citizen:\nBefore we proceed any further, hear me speak.\n\nAll:\nSpeak, speak.\n\nFirst Citizen:\nYou'
Target data: 'irst Citizen:\nBefore we proceed any further, hear me speak.\n\nAll:\nSpeak, speak.\n\nFirst Citizen:\nYou '
1
2
8.4.3 将 dataset 打乱、分批
BATCH_SIZE = 64
# 设定缓冲区大小，以重新排列数据集
# （TF 数据被设计为可以处理可能是无限的序列，
# 所以它不会试图在内存中重新排列整个序列。相反，
# 它维持一个缓冲区，在缓冲区重新排列元素。）
BUFFER_SIZE = 10000
dataset = dataset.shuffle(BUFFER_SIZE).batch(BATCH_SIZE, drop_remainder=True)
1
2
3
4
5
6
7
之后在训练的时候，直接将 dataset 输入词嵌入层即可。训练的详细信息请参照Tensorflow2.0之文本生成莎士比亚作品。
------------------------------------------------
版权声明：本文为CSDN博主「cofisher」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/qq_36758914/article/details/104736266
