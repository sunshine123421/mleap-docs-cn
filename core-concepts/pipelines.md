# Pipelines

机器学习pipeline由一系列操作data frame的transformer组成。它允许我们将
特征提取的transformer和预测算法合并在一起。pipeline可以是简单的一个transformer，
也可以很复杂，包含上百个特征提取transformer和多个预测算法


# 简单Pipeline样例

下面这幅图展示了一个简单pipeline，可以被序列化成一个bundle，然后在MLeap运行环境加载并预测评分。
MLeap支持序列化以及在连续型和离散型特征数据上执行Transformers。更复杂的pipeline可能包含降维
transformer（例如PCA）和特征选择transformer（例如Chi-Squared)。

<img src="../assets/images/simple-pipeline.jpg" alt="Very Simple Pipeline"/>


# 复杂Pipelines

想要见识更复杂的pipelines，可以点开此链接[MLeap demo notebooks](https://github.com/combust/mleap-demo).
