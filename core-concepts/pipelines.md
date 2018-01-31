# 工作流

机器学习工作流由一系列操作数据帧的转换器组成。它允许我们将特征提取的转换器和预测算法合并在一起。
工作流可以是简单的一个转换器，也可以很复杂，包含上百个特征提取转换器和多个预测算法。


# 简单工作流样例

下面这幅图展示了一个简单工作流，可以被序列化成一个bundle，然后在MLeap运行时加载并预测评分。
MLeap支持序列化以及在连续型和离散型特征数据上执行转换器。更复杂的工作流可能包含降维
转换器（例如PCA）和特征选择转换器（例如Chi-Squared)。

<img src="../assets/images/simple-pipeline.jpg" alt="Very Simple Pipeline"/>


# 高级工作流

想要见识更复杂的工作流，可以参考[MLeap demo notebooks](https://github.com/combust/mleap-demo).
