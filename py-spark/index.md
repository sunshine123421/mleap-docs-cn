# MLeap PySpark集成

MLeap的PySpark集成具有下列特点:
* 将Bundle.ML中的转换器和工作流序列化/反序列化
* 支持额外的特征转换器和模型（例如：SVM, OneVsRest, MapTransform）
* 支持自定义转换器

使用MLeap，您不必更改现有工作流的构建方式，因此接下来的文章更集中于如何从bundle.ml序列化和反序列化你的工作流。如何在Spark外执行工作流，参考[MLeap Runtime](../mleap-runtime/index.md)章节。

# PySpark序列化

使用PySpark进行序列化和反序列化几乎与MLeap完全相同。唯一的不同点是序列化和反序列化Spark工作流我们需要引入不同的支持类。

## 构建一个简单的Spark工作流

```python
# Imports MLeap serialization functionality for PySpark
import mleap.pyspark
from mleap.pyspark.spark_support import SimpleSparkSerializer

# Import standard PySpark Transformers and packages
from pyspark.ml.feature import VectorAssembler, StandardScaler, OneHotEncoder, StringIndexer
from pyspark.ml import Pipeline, PipelineModel
from pyspark.sql import Row

# Create a test data frame
l = [('Alice', 1), ('Bob', 2)]
rdd = sc.parallelize(l)
Person = Row('name', 'age')
person = rdd.map(lambda r: Person(*r))
df2 = spark.createDataFrame(person)
df2.collect()

# Build a very simple pipeline using two transformers
string_indexer = StringIndexer(inputCol='name', outputCol='name_string_index')

feature_assembler = VectorAssembler(inputCols=[string_indexer.getOutputCol()], outputCol="features")

feature_pipeline = [string_indexer, feature_assembler]

featurePipeline = Pipeline(stages=feature_pipeline)

featurePipeline.fit(df2)

```


## 序列化到压缩文件

为了序列化到一个压缩文件，确保URI以`jar:file`作为前缀，以`.zip`作为后缀。

例如`jar:file:/tmp/mleap-bundle.zip`.

### JSON格式

```python
featurePipeline.serializeToBundle("jar:file:/tmp/pyspark.example.zip")
```

### Protobuf格式

即将推出

## 反序列化

反序列化像序列化一样简单。你不需要知道先前MLeap Bundle的序列化格式，只需要知道bundle在哪里。

### 压缩Bundle

```python
featurePipeline = PipelineModel.deserializeFromBundle("jar:file:/tmp/pyspark.example.zip")
```
