# 监督学习 Supervised learning
## 定义
给定输入x和与其映射的正确标签y进行学习，给出更多输入x的预测和猜测。[learns from being given "right answers"]
## 场景
spam filtering、speech recognition、 machine translation、 online advertising、 self-driving car、 visual inspection(目视检查)
## 回归算法 Regression
我们试图从无数可能的数字中预测一个数字[Predict a number infinitely many possible outputs]
## 分类算法 Classification
与回归不同，预测结果是有限集合[Predict categories small number of possible outputs]
# 无监督学习 Unsupervised learning
给定的数据与任何输出标签y无关，我们的工作是找到一些结构或模式，在数据中找到想要的答案。[Data only comes with inputs x, but not output labels y. Algorithm has to find structure in the data.]
## 聚类算法 Clustering
无监督学习算法可能决定将数据分配给两个不同的组或两个不同的集群。比例Google News查看新闻文章并将相关故事组合在一起[Group similar data points together]
## 异常检测 Anomaly detection
Find unsual data points
## 降维 Dimensionality reduction
Compress data using fewer numbers
# 线性回归 Linear Regression
Supervised learning model.Data has "right answers"
Regression model predicts numbers
Linear regression with one variable = Univariate linear regression
## Terminology
### Training Set: Data used to train the model
#### x = "input" varible = feature
#### y = "output" varible = "target" varible
#### m = number of training examples
#### (x, y) = single training example
#### (x(i), y(i)) = (i)th training example
#### f = model
#### y-hat = the prediction = estimated y
# 代价函数 cost function
代价函数的思想是机器学习中最普遍和最重要的思想之一，用于线性回归和训练世界上许多最先进的人工智能模型。
输入特征x，输出目标y的训练集。 拟合这个训练集的模型是线性函数f_w,b(x)= wx + b
w和b称为parameters(参数)，是可以在训练期间调整以改进模型的变量。 也被称为coefficients(系数)和weights(权重)
成本函数j(w,b)
如何衡量一条直线与训练数据的拟合程度，为此，我们将构建一个成本函数。成本函数采用预测y-hat并通过取预测y-hat 减去目标y得到的差异(误差)的平方，对每个训练示例的平方差进行累加， 最后除以训练样本个数(按照惯例，机器学习使用的成本函数实际上使除以2倍训练示例个数，为了后面的计算更整洁)。最终目的是找到使成本函数最小的w和b的值。
平方误差成本函数是迄今为止线性回归最常用的函数。
# 梯度下降 Gradient Descent
用于找出最小的代价函数，不仅用于线性回归，还用于训练一些最先进的神经网络模型(深度学习模型)
梯度下降是一种可用于尝试最小化任何函数的算法，而不仅仅是线性回归的成本函数。事实证明，梯度下降适用于更一般的函数(具有两个以上参数的模型的其他成本函数)
梯度下降找到局部最小值就停止了，不会继续下去，这个局部最小值不一定是最小值。
w = w - alpha * [dJ(w,b)/dw]
alpha 称为 学习率0到1的值，控制下坡的步幅
[dJ(w,b)/dw] 告诉下坡方向
对于梯度下降算法，将重复计算w、b，直到算法收敛
正确的同步更新:
tmp_w = w - alpha * [dJ(w,b)/dw]
tmp_b = b - alpha * [dJ(w,b)/db]
w = tmp_w
b = tmp_b
错误的更新更新:
tmp_w = w - alpha * [dJ(w,b)/dw]
w = tmp_w
tmp_b = b - alpha * [dJ(w,b)/db]
b = tmp_b
## 批量梯度下降 Batch gradient descent
Batch : Each step of gradient descent used all the training examples.
# 多类特征 Multiple Features(variables)
xj = jth feature
n = number of features
x(i) = features of ith training example
xj(i) = value of feature j in ith training example
fw,b(x) = w1 * x1 + w2 * x2 + …… + wn * xn + b = w · x + b
该模型称为具有多输入特征的多元线性回归(multiple linear regression), 并不是多元回归(multivariate regression)
# 向量化 Vectorization
## Parameters and features
w = [w1 w2 w3] n = 3
b is a number
x = [x1 x2 x3]
```
w = np.array([1.0,2.5,-3.3])
b = 4
x = np.array([10,20,30])
```
Without vectorization
fw,b(x) = w1 * x1 + w2 * x2 + w3 * x3 + b  
```
f = 0
for j in range(0,n):
	f = f + w[j] * x[j]
f = f + b	
``` 
Vectorization
fw,b(x) = w · x + b
```
f = np.dot(w,x) + b
```
## Gradient descent
w = (w1, w2, ..., w16)
d = (d1, d2, ..., d16) dereivatives
w = np.array([0.5, 1.3, ..., 3.4])
d = np.array([0.3, 0.2, ..., 0.4])
compute wj = wj - 0.1(learning rate ) * dj for j = 1 ... 16
with vectorization 
w = w - 0.1d
Vector notation
Parameters 	w = [w1, ..., wn]
Model 		fw,b(x) = w · x + b
Cost function 	J(w, b)
Gradient descent 	repeat {
						wj = wj - alpha * [dJ(w,b)/dw] = wj - alpha * (1/m) sumUp((fw,b(x) - y)) * x)
						b  = b - alpha * [dJ(w,b)/db]
					}

 An alternative to gradient descent
 Normal equation
 - only for linear regression
 - solve for w, b without iterations, but slow when number of features is large

# Feature scaling









































