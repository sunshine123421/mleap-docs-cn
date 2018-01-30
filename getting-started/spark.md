# 开始Spark

MLeap Spark integration provides serialization of Spark-trained ML
pipelines to [MLeap Bundles](../mleap-bundle/). MLeap also provides
several extensions to Spark, including enhanced one hot encoding, one vs
rest models and unary/binary math transformations.

MLeap Spark集成能够将Spark训练的机器学习工作流序列化到[MLeap Bundles](../mleap-bundle/)。
MLeap还提供了对Spark的几种扩展，包括加强的独热编码（one hot encoding），one-vs-rest模型算法
和一元/二元数学转换器。

## 将MLeap Spark加入到你的项目

MLeap Spark和其snapshots版本都放在了Maven中心，所以可以非常方便的通过Maven构建文件
或者SBT获取。MLeap目前分别用Scala 2.10和2.11做了编译，因为我们希望保持和Spark
使用Scala版本的兼容性。

### 使用SBT

```sbt
libraryDependencies += "ml.combust.mleap" %% "mleap-spark" % "0.8.0"
```

在Spark中使用MLeap扩展：

```sbt
libraryDependencies += "ml.combust.mleap" %% "mleap-spark-extension" % "0.8.0"
```

### 使用Maven

```pom
<dependency>
  <groupId>ml.combust.mleap</groupId>
  <artifactId>mleap-spark_2.11</artifactId>
  <version>0.8.0</version>
</dependency>
```

在Spark中使用MLeap扩展：

```pom
<dependency>
  <groupId>ml.combust.mleap</groupId>
  <artifactId>mleap-spark-extension_2.11</artifactId>
  <version>0.8.0</version>
</dependency>
```

1. 参考[build instructions](./building.html)从源码构建MLeap
2. 参考[core concepts](../core-concepts/)机器学习工作流综述
3. 参考[Spark documentation](http://spark.apache.org/docs/latest/ml-guide.html)学习如何在Spark中训练机器学习工作流
4. 参考[Demo notebooks](https://github.com/combust/mleap-demo/tree/master/notebooks)如何用PySpark和MLeap序列化你的工作流到Bundle.ML
