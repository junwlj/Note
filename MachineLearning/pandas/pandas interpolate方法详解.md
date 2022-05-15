Python Pandas dataframe.interpolate()用法及代码示例
Python是进行数据分析的一种出色语言，主要是因为以数据为中心的python软件包具有奇妙的生态系统。 Pandas是其中的一种，使导入和分析数据更加容易。

Pandas dataframe.interpolate()函数本质上是用来填充NA DataFrame 或系列中的值。但是，这是一个非常强大的函数，可以填补缺失的值。它使用各种插值技术来填充缺失值，而不是hard-coding值。

用法： DataFrame.interpolate(method=’linear’, axis=0, limit=None, inplace=False, limit_direction=’forward’, limit_area=None, downcast=None, **kwargs)

参数：
method:{“线性”，“时间”，“索引”，“值”，“最近”，“零”，“线性”，“二次”，“三次”，“重心”，“克罗格”，“多项式”，“样条”，“ piecewise_polynomial”，“ from_derivatives”，“ pchip”，“ akima”}

axis:0填充column-by-column和1填充row-by-row。
limit:要填充的连续NaN的最大数量。必须大于0。
limit_direction:{“前进”，“后退”，“两者”}，默认为“前进”
limit_area:无(默认)无填充限制。inside 仅填充有效值包围的NaN(内插)。outside 仅将NaN填充到有效值之外(外推)。如果指定了限制，则将沿该方向填充连续的NaN。
inplace:如果可能，更新NDFrame。
downcast:如果可能，请向下转换dtypes。
kwargs:关键字参数传递给插值函数。

返回值：在NaN处插补的相同形状的Series或DataFrame

范例1：采用interpolate()函数使用线性方法填充缺失值。

# importing pandas as pd
import pandas as pd

# Creating the dataframe
df = pd.DataFrame({"A":[12, 4, 5, None, 1],
                   "B":[None, 2, 54, 3, None],
                   "C":[20, 16, None, 3, 8],
                   "D":[14, 3, None, None, 6]})

# Print the dataframe
df


让我们使用线性方法对缺失值进行插值。请注意，线性方法会忽略索引，并将值等距地对待。

# to interpolate the missing values
df.interpolate(method ='linear', limit_direction ='forward')
输出：


正如我们看到的输出，第一行中的值无法填充，因为值的填充方向为forward并且没有可用于插值的先前值。

范例2：采用interpolate()函数使用线性方法向后插值缺失值，并限制最大连续数Na可以填充的值。

# importing pandas as pd
import pandas as pd

# Creating the dataframe
df = pd.DataFrame({"A":[12, 4, 5, None, 1],
                   "B":[None, 2, 54, 3, None],
                   "C":[20, 16, None, 3, 8],
                   "D":[14, 3, None, None, 6]})

# to interpolate the missing values
df.interpolate(method ='linear', limit_direction ='backward', limit = 1)
输出：

请注意第四列，因为我们将极限设置为1，所以仅填充了一个缺失值。最后一行的缺失值无法填充，因为在该值之后可以插值的行不存在。
