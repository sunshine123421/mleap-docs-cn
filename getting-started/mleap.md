# 开始MLeap

MLeap运行时本身提供了执行序列化的机器学习工作流所需的一切。但是它不包含任何训练机器学习工作流所需的部分。所以要开始
使用MLeap，你必须将MLeap加入到你的项目中。

## 将MLeap加入到你的项目

MLeap和其snapshots版本都放在了Maven中心，所以可以非常方便的通过Maven构建文件
或者SBT获取。MLeap目前分别用Scala 2.10和2.11做了编译，因为我们希望保持和Spark
使用Scala版本的兼容性。

### 使用SBT

```sbt
libraryDependencies += "ml.combust.mleap" %% "mleap-runtime" % "0.8.0"
```

### 使用Maven

```pom
<dependency>
  <groupId>ml.combust.mleap</groupId>
  <artifactId>mleap-runtime_2.11</artifactId>
  <version>0.8.0</version>
</dependency>
```

1. 参考[build instructions](./building.html)从源码构建MLeap
2. 参考[core concepts](../core-concepts/)机器学习工作流综述
3. 参考[basic usage](../basic/)在MLeap中转换LeapFrames。
