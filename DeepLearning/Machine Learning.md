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








