# 开始Scikit-Learn

MLeap Scikit-Learn集成能够将Scikit-Learn训练的机器学习工作流序列化到[MLeap Bundles](../mleap-bundle/)。
同时MLeap也支持对Scikit的几种扩展，包括[Spark Extensions transformers.](../core-concepts/transformers/support.md#extensions)。

MLeap Scikit集成在转换器，工作流和复合特征空间(Feature Unions)上加入了Bundle.ML序列化。
必须指出的是，因为核心执行引擎是Scala写的而且在Spark转换器上建模，MLeap Scikit仅仅支持在Spark
可执行或者基于Spark扩展库的转换器。完整的Scikit转换器支持列表在
[supported transformers page](../core-concepts/transformers/support.md)，
或者你可以参考[custom transformers]()章节支持定制化转换器。

## 将MLeap Scikit加入到你的项目

想将MLeap加入你的Scikit项目，只需要pip安装MLeap。

```bash
pip install mleap
```

然后在python环境中针对你想要序列化的Scikit转换器导入MLeap扩展。

```python
# Extends Bundle.ML Serialization for Pipelines
import mleap.sklearn.pipeline

# Extends Bundle.ML Serialization for Feature Unions
import mleap.sklearn.feature_union

# Extends Bundle.ML Serialization for Base Transformers (i.e. LabelEncoder, Standard Scaler)
import mleap.sklearn.preprocessing.data

# Extends Bundle.ML Serialization for Linear Regression Models
import mleap.sklearn.base

# Extends Bundle.ML Serialization for Logistic Regression
import mleap.sklearn.logistic

# Extends Bundle.ML Serialization for Random Forest Regressor
from mleap.sklearn.ensemble import forest
```

更多如何在Scikit中使用MLeap扩展的信息

1. 参考[build instructions](./building.html)从源码构建MLeap
2. 详细指南[MLeap and Scikit-Learn](../scikit-learn/index.md)
3. 参考[Scikit-learn documentation](http://scikit-learn.org/stable/modules/generated/sklearn.pipeline.Pipeline.html) 如何在Python中训练机器学习工作流
4. 参考Scikit-learn文档[Feature Unions](http://scikit-learn.org/stable/modules/generated/sklearn.pipeline.FeatureUnion.html)如何在工作流中使用复合特征空间
5. 参考[Demo notebook](https://github.com/combust/mleap-demo/)如何用Scikit和MLeap序列化你的工作流到Bundle.ml
6. [Learn](../basic/transofrm-leap-frame.md) 如何用MLeap转换数据帧[DataFrame](../core-concepts/data-frames/index.md)
