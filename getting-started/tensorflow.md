# 开始Tensrflow

MLeap Tensorflow集成支持将Tensorflow图用作机器学习工作流中的转换器步骤。将来我们会
提供更多兼容。Tensorflow集成目前还处于实验阶段，因为Tensorflow和MLeap集成都还在
逐渐稳定的路上。

## 构建 MLeap-Tensorflow

MLeap Tensorflow模块还不在Maven中心，而必须在Tensorflow JNI的支持下通过源码
编译构建。参考[instructions](building.md#build-tensorflow-mleap-module)如何
构建Tensorflow模块

## 使用 MLeap-Tensorflow

编译构建成功后，将Tensorflow嵌入MLeap工作流就很容易了。

首先，在项目依赖中添加：

```sbt
libraryDependencies += "ml.combust.mleap" %% "mleap-tensorflow" % "0.8.0"
```

然后我们就可以使用Tensorflow图了，让我们建一个简单的图做两个张量相乘：

```scala
import ml.combust.bundle.dsl.Shape
import ml.combust.mleap.runtime.{LeapFrame, LocalDataset, Row}
import ml.combust.mleap.runtime.types.{FloatType, StructField, StructType}
import org.tensorflow

// Initialize our Tensorflow demo graph
val graph = new tensorflow.Graph

// Build placeholders for our input values
val inputA = graph.opBuilder("Placeholder", "InputA").
  setAttr("dtype", tensorflow.DataType.FLOAT).
  build()
val inputB = graph.opBuilder("Placeholder", "InputB").
  setAttr("dtype", tensorflow.DataType.FLOAT).
  build()

// Multiply the two placeholders and put the result in
// The "MyResult" tensor
graph.opBuilder("Mul", "MyResult").
  setAttr("T", tensorflow.DataType.FLOAT).
  addInput(inputA.output(0)).
  addInput(inputB.output(0)).
  build()

// Build the MLeap model wrapper around the Tensorflow graph
val model = TensorflowModel(graph,
  // Must specify inputs and input types for converting to TF tensors
  inputs = Seq(("InputA", FloatType(false)), ("InputB", FloatType(false))),
  // Likewise, specify the output values so we can convert back to MLeap
  // Types properly
  outputs = Seq(("MyResult", FloatType(false))))

// Connect our Leap Frame values to the Tensorflow graph
// Inputs and outputs
val shape = Shape().
  // Column "input_a" gets sent to the TF graph as the input "InputA"
  withInput("input_a", "InputA").
  // Column "input_b" gets sent to the TF graph as the input "InputB"
  withInput("input_b", "InputB").
  // TF graph output "MyResult" gets placed in the leap frame as col
  // "my_result"
  withOutput("my_result", "MyResult")

// Create the MLeap transformer that executes the TF model against
// A leap frame
val transformer = TensorflowTransformer(inputs = shape.inputs,
  outputs = shape.outputs ,
  rawOutputCol = Some("raw_result"),
  model = model)

// Create a sample leap frame to transform with the Tensorflow graph
val schema = StructType(StructField("input_a", FloatType()), StructField("input_b", FloatType())).get
val dataset = LocalDataset(Seq(Row(5.6f, 7.9f),
  Row(3.4f, 6.7f),
  Row(1.2f, 9.7f)))
val frame = LeapFrame(schema, dataset)

// Transform the leap frame and make sure it behaves as expected
val data = transformer.transform(frame).get.dataset
assert(data(0)(3) == 5.6f * 7.9f)
assert(data(1)(3) == 3.4f * 6.7f)
assert(data(2)(3) == 1.2f * 9.7f)

// Cleanup the transformer
// This closes the TF session and graph resources
transformer.close()
```

更多Tensorflow集成如何工作的资料：

1. 数据转换和集成细节[这里](../tensorflow/mleap-integration.md)
2. 如何将Tensorflow图序列化为MLeap Bundles[这里](../tensorflow/bundle-serialization.md)
