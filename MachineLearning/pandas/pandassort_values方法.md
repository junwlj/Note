pandas.sort_values方法详解
天选憨憨曹冒冒
天选憨憨曹冒冒
20 人赞同了该文章
在之前的文章里我们详细的讨论了pandas中的pandas.loc方法以及pandas.iloc方法，今天在看《Python数据分析实战》的时候又发现了一个pandas中已经被deprecated的方法，我们今天就来聊一聊pandas.DataFrame.sort_values方法，同样的我们先看文档：

DataFrame.sort_values(by, axis=0, ascending=True, inplace=False, kind='quicksort', na_position='last')
Sort by the values along either axis
可以看到这个方法就是按照DataFrame的行或者列来进行排序，参数列表里面有'by', 'axis', 'ascending', 'inplace', 'kind', 'na_position'这几个参数，现在我们就来看一看每个参数是什么作用：

>>> import numpy as np
>>> import pandas as pd
>>> df = pd.DataFrame({
    'col1' : ['A', 'A', 'B', np.nan, 'D', 'C'],
    'col2' : [2, 1, 9, 8, 7, 4],
    'col3' : [0, 1, 9, 4, 2, 3]
})
>>> print(df)
  col1  col2  col3
0    A     2     0
1    A     1     1
2    B     9     9
3  NaN     8     4
4    D     7     2
5    C     4     3
这里定义了一个6行3列的DataFrame，其中有一个空值。

axis这个参数的默认值为0，匹配的是index，跨行进行排序，当axis=1时，匹配的是columns，跨列进行排序

by这个参数要求传入一个字符或者是一个字符列表，用来指定按照axis的中的哪个元素来进行排序

>>> print(df.sort_values(by='col1'))
  col1  col2  col3
0    A     2     0
1    A     1     1
2    B     9     9
5    C     4     3
4    D     7     2
3  NaN     8     4
>>> print(df.sort_values(by=['col1', 'col2']))
  col1  col2  col3
1    A     1     1
0    A     2     0
2    B     9     9
5    C     4     3
4    D     7     2
3  NaN     8     4
ascending这个参数的默认值是True，按照升序排序，当传入False时，按照降序进行排列

>>> print(df.sort_values(by='col1', ascending=False))
  col1  col2  col3
4    D     7     2
5    C     4     3
2    B     9     9
0    A     2     0
1    A     1     1
3  NaN     8     4
>>> print(df.sort_values(by='col1', ascending=True))
  col1  col2  col3
0    A     2     0
1    A     1     1
2    B     9     9
5    C     4     3
4    D     7     2
3  NaN     8     4
kind这个参数表示按照什么样算法来进行排序，默认值是quicksort（快速排序），也可以传入mergesort（归并排序）或者是heapsort（堆排序），至于具体每种算法是如何实现的，我们这里按下不表，同样的，对于inplace这个参数我们也不做讨论对于涉及到的In-place algorithm（原地算法）感兴趣的可以看看这里
最后一个参数na_position是针对DataFrame中的空缺值的，默认值是last表示将空缺值放在排序的最后，也可以传入first放在最前：

>>> print(df.sort_values(by='col1', ascending=False, na_position='first'))
  col1  col2  col3
3  NaN     8     4
4    D     7     2
5    C     4     3
2    B     9     9
0    A     2     0
1    A     1     1
>>> print(df.sort_values(by='col1', ascending=False, na_position='last'))
  col1  col2  col3
4    D     7     2
5    C     4     3
2    B     9     9
0    A     2     0
1    A     1     1
3  NaN     8     4
今天pandas.DataFrame.sort_values这个方法我们就讲到这里啦！其实pandas.Series也有sort_values方法，但是和Dataframe.sort_values的用法很接近，我就不赘述啦！有兴趣的同学可以去看看文档，今天文章里涉及的代码都可以在我的Github里找到，文章和代码中如果出现了什么错误还烦请各位不吝赐教，批评指正！

发布于 2018-03-27 23:01
