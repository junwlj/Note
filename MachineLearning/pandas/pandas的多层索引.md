## pandas的多层索引

#### Series 的多层索引

-   隐式方式：直接使用多级的list指定
-   显示方式：使用MultiIndex类的from_arrays()/from_tuple()/from_product()方法创建

<font color='red'>【建议】推荐使用呢MultiIndex.from_product()方法<font>

#### DataFrame 多层索引

-   多层索引的创建同Series
-   DataFrame可以创建多层列索引

#### 多层索引的操作与切片

-   最外层的表示第一级层次索引，level=0
-   最内层的表示最末级层次索引，level=-1，如果层次为2层，也可以是level=1
-   选择某一内层索引标签时必须写全（以元组的方式)

#### 多层索引的堆操作

-   stack() 将列的多层索引转成行索引【默认情况将列的最内层索引转换为行的最内层索引】
    -   stack(level=0) 将列的最外层索引转换为行的最内层索引
-   unstack() 将行的多层索引转成列索引
-   两者都可以通过level指定转换的所有层级，0是最外层，-1是最内层

#### 多层索引的聚合操作

-   min()/max()/mean()/sum() 聚合函数可以指定level，表示按照那一层进行聚合

#### 排列

-   take()指定排列的顺序
-   np.random.permutation(n) 随机排列

#### 数据