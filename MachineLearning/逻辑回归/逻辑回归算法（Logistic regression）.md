# 逻辑回归算法（Logistic regression）

#### 目标：

- 目标：经典的二分类的算法（分类还是回归）
- 机器学习算法选择：先逻辑回归，再用复杂
- 逻辑回归的决策边界：可以是线性，也可以是非线性

**注：在机器学习建模中，在未知分类的情况首先使用逻辑回归算法**

#### Sigmoid函数

- 公式：

	- $g(z) = \frac{1}{1+e^{-z}}$

- 自变量取值为任意实数，值域[0,1]

- 解释：将任意输入映射到了[0,1]的区间中，则我们可以在线性回归中可以得到一个预测值，再将该值映射到sigmoid函数中，即完成由值到概率的转换，也就是分类任务

- 预测函数：$h_0(x) = g(\theta^Tx) = \frac{1}{1+e^{-\theta^Tx}}$

	- 其中$$\theta^Tx = \sum_{i=1}^{n}\theta_ix_i = \theta_0 + \theta_1x_1 + …… + \theta_nx_n$$

- 分裂任务：

	- $P(y=1|x;\theta) = h_0(x)$
	- $P(y=0|x;\theta) = 1-h_0(x)$

	即$P(y|x;\theta) = (h_0(x))^y(1-h_0(x))^{1-y}$

- 解释：对于二分类任务(0,1)，整合后y取0只保留$(1-h_0(x))^{1-y}$，y取1只保留$(h_0(x))^y$

#### Logistic regression

- 似然函数：$L(\theta) = \Pi_{i=1}^{m}P(y_i|x_i;\theta)  = (h_0(x_i))^{y_i}(1-h_0(x_i))^{1-y_i}$
- 对数似然：$l(\theta) = logL(\theta) = \sum_{i=1}^{m}(y_ilogh_0(x_i) + (1-y_i)log(1-h_0(x_i)))$
- 此时应用梯度上升求最大值，引入$J(\theta) = -\frac{1}{m}l(\theta)$转换为梯度下降任务

**似然函数求极大值，即对梯度上升问题的求解**

$l(\theta) = logL(\theta) = \sum_{i=1}^{m}(y_ilogh_{\theta}(x_i) + (1-y_i)log(1-h_{\theta}(x_i)))$

![1564670039023](E:\资料\Note\images\siran.png)

