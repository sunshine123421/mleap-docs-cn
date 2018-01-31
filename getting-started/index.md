# 开始之旅

无论是结合PySpark，Scikit-learn，Spark，Tensorflow或是MLeap运行时（MLeap Runtime)使用MLeap，
入门都非常容易。大多数你所需要的都可以在[Maven Central](https://search.maven.org/)或者[PyPi](https://pypi.python.org/pypi)
下载安装。但是如果你尝试通过MLeap执行Tensorflow转换器（Transformer），你需要构建自己的jar包。

可以很容易的通过交互式Scala命令行，Python命令行或者notebook体验MLeap。

## 典型MLeap流程

一个典型的MLeap流程包含3部分：
1. 训练（Training）：用当前传统方式编写机器学习工作流（ML Pipeline）代码
2. 序列化（Serialization）：序列化所有的数据处理工作流和算法到Bundle.ML文件
3. 执行（Execution）：用MLeap运行时执行序列化的工作流，而不依赖于Spark或者Scikit(但你仍需要TensorFlow库)

### 训练

MLeap的设计目标是对当前构建机器学习工作流的方式影响最小。我们希望你在原有的Scala或者Python代码上，
加上尽量少的附加代码就可以支持MLeap功能。

你将在后面的‘基本用法’章节看到，大多数情况下你只需要引入一些MLeap库就可以了（scikit-learn除外）。

### 序列化

一旦你的机器学习工作流训练完毕，MLeap可以将整个机器学习/数据工作流和训练算法（线性模型，树模型，神经网络）序列化到Bundle.ML。
序列化生成一个`bundle`，物理上代表了一个可以部署，共享，浏览的机器学习工作流和算法。

### 执行

MLeap最初的目标是使得Spark机器学习工作流的预测评分（scoring)不依赖于Spark。这个功能是由MLeap运行时提供支持，
它加载序列化`bundel`，然后基于后面章节介绍的LeapFrames执行。

我们有提过MLeap运行时执行速度惊人吗？基准测试记录显示在LeapFrames上执行时间都在毫秒级别，RESTful API服务响应时间在5毫秒以内。

提示：目前MLeap运行时仅仅支持Java/Scala库，我们计划未来会添加python绑定。

## 组件（Components）

### MLeap Spark


### MLeap Scikit


### MLeap Bundle


### MLeap运行时
