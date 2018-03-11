# MLeap Scikit-Learn集成

MLeap为Scikit工作流提供序列化功能，特征空间和转换器Bundle.Ml可以以保证Scikit和Spark的转换器功能平衡的方式。
MLeap-Scikit两个主要的用例：
1. 序列化Scikit工作流和MLeap运行时执行
2. 序列化Scikit工作流和Spark反序列化

前面提到过现在MLeap运行时是一个仅有scala语言的库，未来我们计划添加对Python的支持。然而即使没有Scikit和Numpy的依赖，也能足够执行工作流和模型。

## MLeap扩展Scikit

scikit转换器和Spark转换器的工作原理有下列重要的不同点：
1. Spark转换器带有`name`, `op`, `inputCol`, and `outputCol`的属性，scikit没有
2. Spark转换器可以在向量上运行，scikit是运行在n维数组和矩阵上
3. Spark是用Scala语言编写，所以对Spark添加隐式函数和属性相对容易，scikit稍微棘手，需要使用setattr()方法

由于这些额外的复杂性，当用MLeap扩展scikit转换器时，我们必须遵循下列格式。
首先，需要初始化每个转换器并包括：
* Op: 唯一的`op`名字 - 把它当作一个基于Spark转换器的链接使用（例如scikit里的标准缩放与Spark是相同的，所以我们有一个名为`standard_scaler`的操作来表示它）
* 名字：每一个转换器有一个唯一的命名。例如，如果有多个标准缩放对象，每一个都需要一个唯一的命名
* 输入列：为了序列化，我们要设置输入列
* 输出列: 为了反序列化，需要设置输出列

### MLeap的Scikit转换器和工作流

首先，初始化所有需要的库
```python
# Initialize MLeap libraries before Scikit/Pandas
import mleap.sklearn.preprocessing.data
import mleap.sklearn.pipeline
from mleap.sklearn.preprocessing.data import FeatureExtractor

# Import Scikit Transformer(s)
import pandas as pd
import numpy as np
from sklearn.preprocessing import StandardScaler
from sklearn.pipeline import Pipeline
```

接着，在Pandas中创建一个测试的数据帧

```python
# Create a pandas DataFrame
df = pd.DataFrame(np.random.randn(10, 5), columns=['a', 'b', 'c', 'd', 'e'])
```

我们定义了两个转换器，一个特征提取器将只提取我们想要缩放的特征；另一个标准缩放器将执行标准的正常缩放操作。

```python
# Initialize a FeatureExtractor, which subselects only the features we want
# to run the Standard Scaler against
input_features = ['a', 'c', 'd']
output_vector_name = 'unscaled_continuous_features' # Used only for serialization purposes
output_features = ["{}_scaled".format(x) for x in input_features]

feature_extractor_tf = FeatureExtractor(input_scalars=input_features,
                                        output_vector=output_vector_name,
                                        output_vector_items=output_features)


# Define the Standard Scaler as we normally would
standard_scaler_tf = StandardScaler(with_mean=True,
                                    with_std=True)

# Execute ML-Init to add the require attributes to the transformer object
# Op and Name will be initialized automatically
standard_scaler_tf.mlinit(prior_tf=feature_extractor_tf,
                          output_features='scaled_continuous_features')
```

现在，已经有我们定义的转换器，我们将它们合并为一个工作流并在数据帧中执行

```python
# Now let's create a small pipeline using the Feature Extractor and the Standard Scaler
standard_scaler_pipeline = Pipeline([(feature_extractor_tf.name, feature_extractor_tf),
                                     (standard_scaler_tf.name, standard_scaler_tf)])
standard_scaler_pipeline.mlinit()

# Now let's run it on our test DataFrame
standard_scaler_pipeline.fit_transform(df)

# Printed output
array([[ 0.2070446 ,  0.30612846, -0.91620529],
       [ 0.81463009, -0.26668287,  1.95663995],
       [-0.94079041, -0.18882131, -0.0462197 ],
       [-0.43931405,  0.13214763, -0.10700743],
       [ 0.43992551, -0.2985418 , -0.89093752],
       [-0.15391539, -2.20828471,  0.5361159 ],
       [-1.07689244,  1.61019861,  1.42868885],
       [ 0.87874789,  1.43146482, -0.44362038],
       [-1.60105094, -0.40130005, -0.10754886],
       [ 1.87161513, -0.11630878, -1.40990552]])
```

## 合并转换器

我们演示了如何在一系列特征上做转换器，但是这些操作的输出只是一个n维数组，如果我们想在回归模型中使用它，那么我们需要加入到原始数据中。接下来演示如何使用特征空间从多个转换器中合并数据。

首先，继续并创建另一个转换器，在数据帧上对其余两个特征做归一化：

```python
from sklearn.preprocessing import MinMaxScaler

input_features_min_max = ['b', 'e']
output_vector_name_min_max = 'unscaled_continuous_features_min_max' # Used only for serialization purposes
output_features_min_max = ["{}_min_maxscaled".format(x) for x in input_features_min_max]

feature_extractor_min_max_tf = FeatureExtractor(input_scalars=input_features_min_max,
                                                output_vector=output_vector_name_min_max,
                                                output_vector_items=output_features_min_max)


# Define the MinMaxScaler as we normally would
min_maxscaler_tf = MinMaxScaler()

# Execute ML-Init to add the require attributes to the transformer object
# Op and Name will be initialized automatically
min_maxscaler_tf.mlinit(prior_tf=feature_extractor_min_max_tf,
                          output_features='min_max_scaled_continuous_features')

# Assemble our MinMaxScaler Pipeline
min_max_scaler_pipeline = Pipeline([(feature_extractor_min_max_tf.name, feature_extractor_min_max_tf),
                                    (min_maxscaler_tf.name, min_maxscaler_tf)])
min_max_scaler_pipeline.mlinit()

# Now let's run it on our test DataFrame
min_max_scaler_pipeline.fit_transform(df)

array([[ 0.58433367,  0.72234095],
       [ 0.21145259,  0.72993807],
       [ 0.52661493,  0.59771784],
       [ 0.29403088,  0.19431993],
       [ 0.48838789,  1.        ],
       [ 1.        ,  0.46456522],
       [ 0.36402459,  0.43669119],
       [ 0.        ,  0.74182958],
       [ 0.60312285,  0.        ],
       [ 0.33707035,  0.39792128]])
```

最后，使用特征空间合并两个工作流。提示，在集合特征空间前不需要在工作流上运行`fit`或者`fit_transform`方法

```python
# Import MLeap extension to Feature Unions
import mleap.sklearn.feature_union

# Import Feature Union
from sklearn.pipeline import FeatureUnion

feature_union = FeatureUnion([
        (standard_scaler_pipeline.name, standard_scaler_pipeline),
        (min_max_scaler_pipeline.name, min_max_scaler_pipeline)
        ])
feature_union.mlinit()

# Create pipeline out of the Feature Union
feature_union_pipeline = Pipeline([(feature_union.name, feature_union)])
feature_union_pipeline.mlinit()

# Execute it on our data frame
feature_union_pipeline.fit_transform(df)

array([[ 0.2070446 ,  0.30612846, -0.91620529,  0.58433367,  0.72234095],
       [ 0.81463009, -0.26668287,  1.95663995,  0.21145259,  0.72993807],
       [-0.94079041, -0.18882131, -0.0462197 ,  0.52661493,  0.59771784],
       [-0.43931405,  0.13214763, -0.10700743,  0.29403088,  0.19431993],
       [ 0.43992551, -0.2985418 , -0.89093752,  0.48838789,  1.        ],
       [-0.15391539, -2.20828471,  0.5361159 ,  1.        ,  0.46456522],
       [-1.07689244,  1.61019861,  1.42868885,  0.36402459,  0.43669119],
       [ 0.87874789,  1.43146482, -0.44362038,  0.        ,  0.74182958],
       [-1.60105094, -0.40130005, -0.10754886,  0.60312285,  0.        ],
       [ 1.87161513, -0.11630878, -1.40990552,  0.33707035,  0.39792128]])
```

## 序列化到压缩文件

为了序列化到压缩文件，确保URI以`jar:file`作为前缀，以`.zip`作为后缀。

例如：`jar:file:/tmp/mleap-bundle.zip`.

提示，在序列化前必须适配您的工作流。

### JSON格式

设置`init=True`表示创建一个bundle来代替序列化转换器。

```python
feature_union_pipeline.serialize_to_bundle('/tmp', 'jar:file:/tmp/mleap-bundle.zip', init=True)
```

### Protobuf格式

即将推出

### 反序列化

即将推出

## 演示

github上对转换器、工作流、特征空间和序列化的所有步骤提供了完整的演示。
