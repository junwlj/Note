pandas在dataframe中提供了丰富的统计、合并、分组、缺失值等操作函数。

1.统计函数

df.count() #非空元素计算
 df.min() #最小值
 df.max() #最大值
 df.idxmin() #最小值的位置，类似于R中的which.min函数
 df.idxmax() #最大值的位置，类似于R中的which.max函数
 df.quantile(0.1) #10%分位数
 df.sum() #求和
 df.mean() #均值
 df.median() #中位数
 df.mode() #众数 
 df.var() #方差
 df.std() #标准差
 df.mad() #平均绝对偏差
 df.skew() #偏度
 df.kurt() #峰度
 df.describe() #一次性输出多个描述性统计指标
