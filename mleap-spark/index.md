# MLeap Spark集成

MLeap的Spark集成附带以下功能集:
* 将Bundle.ML中的转换和工作流序列化/反序列化
* 支持额外的特征转换和模型（例如：SVM, OneVsRest, MapTransform）
* 支持自定义的转换

要使用MLeap，您不必更改现有工作流的构建方式，因此接下来的文章更集中于如何从bundle.ml序列化和反序列化你的工作流。
如何在Spark外执行工作流，参考[MLeap Runtime](../mleap-runtime/index.md)章节。

# 用Spark序列化

使用Spark进行序列化和反序列化几乎与MLeap完全相同。唯一的不同点是序列化和反序列化Spark工作流我们需要引入不同的隐式支持类。

## 构建一个简单的Spark工作流

```scala
import ml.combust.bundle.BundleFile
import ml.combust.bundle.serializer.SerializationFormat
import org.apache.spark.ml.feature.{StringIndexerModel, VectorAssembler}
import org.apache.spark.ml.mleap.SparkUtil
import ml.combust.mleap.spark.SparkSupport._
import resource._

// Create a sample pipeline that we will serialize
// And then deserialize using various formats
val stringIndexer = new StringIndexerModel(labels = Array("Hello, MLeap!", "Another row")).
  setInputCol("a_string").
  setOutputCol("a_string_index")
val featureAssembler = new VectorAssembler().setInputCols(Array("a_double")).
  setOutputCol("features")

// Because of Spark's privacy, our example pipeline is considerably
// Less interesting than the one we used to demonstrate MLeap serialization
val pipeline = SparkUtil.createPipelineModel(Array(stringIndexer, featureAssembler))
```

## 序列化到压缩文件

为了序列化成压缩文件，确认URI以`jar:file`作为前缀，以`.zip`作为后缀。

例如
`jar:file:/tmp/mleap-bundle.zip`.

### JSON格式

```scala
for(bundle <- managed(BundleFile("jar:file:/tmp/mleap-examples/simple-json.zip"))) {
  pipeline.writeBundle.format(SerializationFormat.Json).save(bundle)
}
```

### Protobuf格式

```scala
for(bundle <- managed(BundleFile("jar:file:/tmp/mleap-examples/simple-protobuf.zip"))) {
  pipeline.writeBundle.format(SerializationFormat.Protobuf).save(bundle)
}
```

## 序列化到目录

为了序列化到目录，确认URI以`file`作为前缀。

For example `file:/tmp/mleap-bundle-dir`

### JSON格式

```scala
for(bundle <- managed(BundleFile("file:/tmp/mleap-examples/simple-json-dir"))) {
  pipeline.writeBundle.format(SerializationFormat.Json).save(bundle)
}
```

### Protobuf格式

```scala
for(bundle <- managed(BundleFile("file:/tmp/mleap-examples/simple-protobuf-dir"))) {
  pipeline.writeBundle.format(SerializationFormat.Protobuf).save(bundle)
}
```

## 反序列化

反序列化像序列化一样容易。您不需要知道MLeap包的序列化格式，您只需知道包的位置。

### Zip包

```scala
// Deserialize a zip bundle
// Use Scala ARM to make sure resources are managed properly
val zipBundle = (for(bundle <- managed(BundleFile("jar:file:/tmp/mleap-examples/simple-json.zip"))) yield {
  bundle.loadSparkBundle().get
}).opt.get
```

### 目录包

```scala
// Deserialize a directory bundle
// Use Scala ARM to make sure resources are managed properly
val dirBundle = (for(bundle <- managed(BundleFile("file:/tmp/mleap-examples/simple-json-dir"))) yield {
  bundle.loadSparkBundle().get
}).opt.get
```
