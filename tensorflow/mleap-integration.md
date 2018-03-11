# Tensorflow MLeap集成

MLeap支持与预先训练的Tensorflow图的集成。MLeap使用Tensorflow提供的Swig绑定，将从MLeap数据类型转换为所需的C++张量类型之后调用Tensorflow C++库。

## MLeap Tensorflow转换器

MLeap Tensorflow转换器使用一个Tensorflow图的文件构建，并使用`freeze_graph`函数保存。该转换器在第一个请求转换为一个leap frame时懒初始化为一个Tensorflow会话。一旦会话开始，直到在一个MLeap Tensorflow转换器上调用`close`方法，否则会话将一直开启。

当操作Tensorflow转换器时，leap frame的数据要使用恰当的格式，以便正确转换为Tensorflow数据类型。参考[Type Conversions](#type-conversions)部分了解如何转化为Tensorflow类型。

转换器输出的张量列表作为Tensorflow输出字段的行，以及指定的所有输出张量作为单独的列。由于MLeap目前的工作机制，中间输出列表是必需的，但我们可能会在未来摆脱它。参考[Example](#example),一个Tensorflow图在leap frame上执行的例子。我们提前需要知道张量的输出类型，以便知道如何在转换后形成新的Leap frame。

### MLeap Tensorflow模型

模型负责维护Tensorflow会话。它是一个Java`Closeable`，当MLeap转换器或其他控制对象不再需要使用资源时要关闭。该模型将在最后关闭Tensorflow会话，以防用户忘记调用此方法。模型的输出是带有MLeap`张量`的`序列`，它包含与给定形状相同顺序的输出。

为了支持在MLeap数据类型上使用Tensorflow做转换器，下列的信息是必须的。

| 属性 | 描述 |
|---|---|
| graph | Tensorflow图定义以 __freeze_graph__ 格式保存|
| shape | 输入/输出以及非可选类型说明 |
| nodes | 保存输出的节点的字符串列表 |

## 类型转换

所有基本数据类型都隐式转换为0级的Tensorflow张量。MLeap中的`张量`数据类型几乎可以与Tensorflow张量进行一对一转换。在传递给Tensorflow之前，首先将稀疏张量的值转换为稠密张量。

以下是目前支持的Tensorflow数据类型的列表。

| Tensorflow数据类型 | MLeap数据类型 |
|---|---|
| DT_BOOLEAN | BooleanType |
| DT_STRING | StringType |
| DT_INT32 | IntegerType |
| DT_INT64 | LongType |
| DT_FLOAT | FloatType |
| DT_DOUBLE | DoubleType |
| _Tensor_ | TensorType |

由于Swig包装器不提供对所有数据类型的支持，我们建议在您的Tensorflow图中添加铸造步骤来与MLeap集成。

### 提示

1.Swig包装器不支持无符号整数类型、8位和16位整数
2. Swig包装器不支持复杂的类型

# 示例

有一个具有以下数据的输入LeapFrame。一个scala和一个一维的双张量（也被称为双打向量）。


## 输入Leap帧

### 数据


| double1 | tensor1 |
|---|---|
| 3.0 | [2.0, 1.0, 5.0] |

### 类型

| Field | Data Type |
|---|---|
| double1 | DoubleType() |
| tensor1 | TensorType(DoubleType(false)) |


如果我们通过Tensorflow转换器运行这个Leap frame，使用`double1`数据扩展为`tensor1`数据，并指定想要缩放的输出张量，接着输出Leap frame类似于：

## 输出Leap frame

### 数据

| double1 | tensor1 | raw_tf_tensors | output1 |
|---|---|---|---|
| 3.0 | [2.0, 1.0, 5.0] | Seq([6.0, 3.0, 15.0]) | [6.0, 3.0, 15.0] |

### 类型

| Field | Data Type | Notes |
|---|---|---|
| double1 | DoubleType() | |
| tensor1 | TensorType(DoubleType(false)) | |
| raw_tf_tensors | ListType(AnyType(false))  | Raw return types unknown, nulls are not allowed |
| output1 | TensorType(DoubleType(false)) | Underlying type known at this point, and reflected in the LeapFrame |

## 提示

1. `raw_tf_tensors`字段包含一个`AnyType`,这表示它不可序列化。如果使用Combust API服务，在检索结果时必须过滤掉这个值。
