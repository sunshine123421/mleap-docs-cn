# Data Frames

Data Frames用于在执行一个机器学习Pipeline时存储数据。类似于SQL表，Data Frames拥有存储列数据类型的模式和真正存储数据的行。

Spark, Scikit-learn和MLeap都有他们自己的Data Frame版本。Tensorflow用一张输入输出表执行转换，很容易和Data Frame结构交互。

## Spark Data Frames

Spark Data Frames针对分布式计算做了优化，在大数据处理方面表现优秀。它非常重量级，因为需要处理网络不通的场景，执行计划的编译，冗余，以及其他一些分布式上下文中的需求。Spark Data Frames还提供了许多机器学习Pipeline之外的功能，比如大数据集的连接，Mapping，Reducing，SQL查询等等。

## Scikit-learn Data Frames

Scikit-learn Data Frames是由[Pandas](http://pandas.pydata.org/)和[NumPy](http://www.numpy.org/)提供的轻量级数据结构，相当多功能和Spark Data Frames保持一致，只是没有了分布式特性。

## MLeap Data Frames: Leap Frames

Leap Frames是非常轻量级的数据结构，目的是支持非常基本的操作和机器学习转换。因其简单，在实时预测和小规模批量预测方面做了高度优化。Leap Frames是基于Spark Data Frames之上的抽象，所以并没有失去作为高效的批量模式数据存储的能力。

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

[Tensorflow](https://www.tensorflow.org/)不像Spark，Scikit-learn和MLeap一样有Data Frames的概念。Tensorflow依赖于用一张转换图连接起来的输入节点和输出节点。这个框架恰好和Data Frames是兼容的，因为某些列刚好可以作为输入节点的数据，而输出节点的数据可以放在Data Frame的新添加列上。Leap Frames很特别的被设计为兼容Tensorflow graphs，Spark Data Frames和Scikit-learn Data Frames。
