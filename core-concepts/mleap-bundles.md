# MLeap Bundles

MLeap Bundles是一个基于图，可移植的序列化/反序列化文件格式:

1. 机器学习工作流 - 任何基于转换器的工作流
2. 算法（回归，树形模型，贝叶斯模型，神经网络，聚类）

Bundles使得共享训练工作流的结果变得非常容易，只需要生成一个Bundle文件，通过邮件发送给同事或者
直接浏览工作流和算法元数据。

Bundles也使部署变得简单：只需要导出bundle然后加载到Spark，Scikit-learn和MLeap应用里。

## MLeap Bundles特点

1. 序列化到一个目录或者zip文件
2. JSON和Protobuf格式
3. 可以序列化成纯JSON，纯Protobuf或者混合模式
4. 高度可扩展化，很容易和新转换器集成

## Spark, Scikit-Learn, TensorFlow通用格式

MLeap提供了Spark，Scikit和TensorFlow通用的转换器序列化格式。比如，标准向量转换器
（Tensorflow里是`tf.random_normal_initializer`）。它在三种平台上执行同样的操作，因此理论上
可以被序列化/反序列化和相互替换。

<img src="../assets/images/common-serialization.jpg" alt="Common Serialization"/>

## Bundle结构

在根目录，一个bundle有一个`bundle.json`文件，包含bundle序列化的基本元数据。另外还有一个`root/`
目录，包含机器学习工作流的根转换器。根转换器可以是MLeap支持的任何转换器，但通常情况会是一个
`Pipeline` 转换器。

下面我们看一个MLeap Bundle的例子。工作流首先是一个将类别特征转化为整数的string indexing，
紧跟着one hot encoding转换器, 然后组装结果到一个特征向量，最后是线性回归算法。bundle结构如下:

```
├── bundle.json
└── root
    ├── linReg_7a946be681a8.node
    │   ├── model.json
    │   └── node.json
    ├── model.json
    ├── node.json
    ├── oneHot_4b815730d602.node
    │   ├── model.json
    │   └── node.json
    ├── strIdx_ac9c3f9c6d3a.node
    │   ├── model.json
    │   └── node.json
    └── vecAssembler_9eb71026cd11.node
        ├── model.json
        └── node.json
```

### bundle.json

```
{
  "uid": "7b4eaab4-7d84-4f52-9351-5de98f9d5d04",
  "name": "pipeline_43ec54dff5b2",
  "timestamp": "2017-09-03T17:41:25.206",
  "format": "json",
  "version": "0.8.0"
}
```

1. `uid`是一个为bundle自动生成的唯一ID（Java UUID）
2. `name`是根转换器的`uid`
3. `format`是序列化格式
4. `version`序列化bundle所用MLeap的版本
5. `timestamp`定义了bundle的生成时间

### model.json

工作流:

```
{
  "op": "pipeline",
  "attributes": {
  "nodes": {
    "type": "list",
    "string": ["strIdx_ac9c3f9c6d3a", "oneHot_4b815730d602", "vecAssembler_9eb71026cd11", "linReg_7a946be681a8"]
  }
}

```

线性回归算法:

```
{
  "op": "linear_regression",
  "attributes": {
    "coefficients": {
      "double": [7274.194347379634, 4326.995162668048, 9341.604695180558, 1691.794448740186, 2162.2199731255423, 2342.150297286721, 0.18287261938061752],
      "shape": {
        "dimensions": [{
          "size": 7,
          "name": ""
        }]
      },
      "type": "tensor"
    },
    "intercept": {
      "double": 8085.6026142683095
    }
}
```

1. `op`指定了执行操作名字，MLeap支持的每一个转换器都有对应的名字
2. `attributes`包含执行操作所需属性值

### node.json

one hot encoder:

```
{
  "name": "oneHot_4b815730d602",
  "shape": {
    "inputs": [{
      "name": "fico_index",
      "port": "input"
    }],
    "outputs": [{
      "name": "fico",
      "port": "output"
    }]
  }
}
```

1. `name`指定了执行节点的名字
2. `shape`指定了节点输入输出，以及执行时如何被内部使用

在这个例子里，`fico_index`列被用于one hot encoder的输入，而`fico`是输出结果列。

## MLeap Bundle例子

这里给出一些序列化bundle文件的例子。他们不是真正有意义的工作流，只是用来展示bundle
文件是什么样子。这些工作流是在跑测试用例生成的，目的是验证MLeap转换器和
Spark转换器得到的结果是一样的。

[MLeap/Spark Parity Bundle例子](../assets/bundles/spark-parity.zip)

注：点击右键选择"Save As...", Gitbook会阻止直接点击链接
