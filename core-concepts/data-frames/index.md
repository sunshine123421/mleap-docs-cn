# 数据帧

数据帧用于在执行一个机器学习工作流时存储数据。类似于SQL表，数据帧用模式存储数据类型，用数据行存储
真正的数据。

Spark, Scikit-learn和MLeap都有他们自己的数据帧版本。Tensorflow用输入输出图进行转换，从而很
容易地和数据帧结构连接。

## Spark数据帧

Spark数据帧针对分布式计算做了优化，在大数据处理方面表现优秀。但是它很重量级，因为需要处理网络不通
场景，执行计划编译，处理冗余，以及其他一些分布式上下文中的需求。Spark数据帧还提供了许多机器学习
工作流之外的功能，比如大数据集的连接，Mapping & Reducing，SQL查询等等。

## Scikit-learn数据帧

Scikit-learn数据帧是由[Pandas](http://pandas.pydata.org/)和[NumPy](http://www.numpy.org/)
提供的轻量级数据结构，相当多功能和Spark数据帧保持一致，只是去掉了分布式特性。

## MLeap数据帧: Leap Frames

Leap Frames是非常轻量级的数据结构，目的是支持非常基本的操作和机器学习转换。因其简单，在实时预测
和小规模批量预测方面做了高度优化。Leap Frames是基于Spark数据帧之上的抽象，仍然保留了高效的批量
模式数据存储的能力。

### Leap Frame例子

下面是一个Leap Frame JSON格式的例子，来源于[AirBnb demo](https://github.com/combust/mleap-demo/blob/master/notebooks/airbnb-price-regression.ipynb):

```json
{
  "schema": {
    "fields": [{
      "name": "state",
      "type": "string"
    }, {
      "name": "bathrooms",
      "type": "double"
    }, {
      "name": "square_feet",
      "type": "double"
    }, {
      "name": "bedrooms",
      "type": "double"
    }, {
      "name": "review_scores_rating",
      "type": "double"
    }, {
      "name": "room_type",
      "type": "string"
    }, {
      "name": "cancellation_policy",
      "type": "string"
    }]
  },
  "rows": [["NY", 2.0, 1250.0, 3.0, 50.0, "Entire home/apt", "strict"]]
}
```

## Tensorflow

[Tensorflow](https://www.tensorflow.org/)不像Spark，Scikit-learn，MLeap一样有数据帧的概念。
Tensorflow依赖于通过转换图连接的输入节点和输出节点。这个框架和数据帧是完美兼容的，因为某些列刚好可以
作为输入节点的数据，而输出节点的数据可以放在数据帧的新增列上。Leap Frames很特别的被设计为兼容
Tensorflow图，Spark数据帧和Scikit-learn数据帧。
