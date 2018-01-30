# 开始PySpark

MLeap PySpark集成能够将PySpark训练的机器学习工作流序列化到[MLeap Bundles](../mleap-bundle/)。
MLeap还提供了对Spark的几种扩展，包括加强的独热编码（one hot encoding）和one-vs-rest模型算法。
不同于MLeap Spark集成，MLeap还不支持PySpark和[Spark Extensions transformers.](../core-concepts/transformers/support.md#extensions)
的集成。

## 将MLeap Spark加入到你的项目

在将PySpark加入到你项目之前，首先需要编译并加入[MLeap Spark](./spark.md)。

MLeap PySpark在[combust/mleap](https://github.com/combust/mleap)github仓库中作为python包存在。

要将MLeap加入到你的PySpark项目，克隆git仓库，加入`mleap/python`路径，然后导入`mleap.pyspark`。

```bash
git clone git@github.com:combust/mleap.git
```

然后在python环境中加入：

```python
import sys
sys.path.append('<git directory>/mleap/python')

import mleap.pyspark
```

提示：导入`mleap.pyspark`需要在导入其他PySpark库之前。

提示：如果你使用notebook作为工作环境，参考下面链接安装MLeap PySpark:
* [Jupyter](../integration/jupyter-notebooks.md)
* [Zeppelin](../integration/zeppelin-notebooks.md)
* [Databricks](../integration/databricks-notebooks.md)

## 使用PIP

PySpark将在不久后支持PIP安装

在PySpark中使用MLeap扩展：

1. 参考[build instructions](./building.html)从源码构建MLeap
2. 参考[core concepts](../core-concepts/)机器学习工作流综述
3. 参考[Spark documentation](http://spark.apache.org/docs/latest/ml-guide.html) 学习如何在Spark中训练机器学习工作流
4. 参考[Demo notebook](https://github.com/combust/mleap-demo/blob/master/notebooks/PySpark%20-%20AirBnb.ipynb) 如何用PySpark和MLeap序列化你的工作流到Bundle.ml
