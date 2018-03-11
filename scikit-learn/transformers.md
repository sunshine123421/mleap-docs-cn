# Scikit转换器示例

这里我们概述一些更复杂的转换器和一些需要额外的处理才能与工作流和特征空间更好配合的转换器。

### 标签编码
`标签编码`与Spark中的`StringIndexer`是同义词，然而在使用中我们需要考虑scikit转换器的一些独特功的特征：
1. `标签编码`一次只能在一个特征上运行
2. `标签编码`的输出是一个(1,n)而不是(n,1)的numpy数组，因此需要进一步处理，像独热编码处理。

下面是一个`标签编码`的工作流示例

```python
# Create a dataframe with some a categorical and a continuous feature
df = pd.DataFrame(np.array([ ['Alice', 32], ['Jack', 18], ['Bob',34]]), columns=['name', 'age'])

# Define our feature extractor
feature_extractor_tf = FeatureExtractor(input_scalars=['name'], 
                                        output_vector='name_continuous_feature', 
                                        output_vector_items=['name_label_encoded'])

# Label Encoder for x1 Label 
label_encoder_tf = LabelEncoder()
label_encoder_tf.mlinit(input_features = feature_extractor_tf.output_vector_items, output_features='name_label_le')

# Reshape the output of the LabelEncoder to N-by-1 array
reshape_le_tf = ReshapeArrayToN1()

# Create our pipeline object and initialize MLeap Serialization
le_pipeline = Pipeline([(feature_extractor_tf.name, feature_extractor_tf),
                        (label_encoder_tf.name, label_encoder_tf),
                        (reshape_le_tf.name, reshape_le_tf)
                        ])
le_pipeline.mlinit()

# Transform our DataFrame
le_pipeline.fit_transform(df)

# output
array([[0],
       [2],
       [1]])
```

下一步是将标签索引与一个`独热编码`结合在一起

### Scikit独热编码

我们会继续上面的示例来演示开箱即用的Scikit独热编码是如何工作的。

```python
## Vector Assembler for x1 One Hot Encoder
one_hot_encoder_tf = OneHotEncoder(sparse=False) # Make sure to set sparse=False
one_hot_encoder_tf.mlinit(prior_tf=label_encoder_tf, output_features = '{}_one_hot_encoded'.format(label_encoder_tf.output_features))
#

# Construct our pipeline
one_hot_encoder_pipeline_x0 = Pipeline([
                                         (feature_extractor_tf.name, feature_extractor_tf),
                                         (label_encoder_tf.name, label_encoder_tf),
                                         (reshape_le_tf.name, reshape_le_tf),
                                         (one_hot_encoder_tf.name, one_hot_encoder_tf)
                                        ])

one_hot_encoder_pipeline_x0.mlinit()

# Execute our LabelEncoder + OneHotEncoder pipeline on our dataframe
one_hot_encoder_pipeline_x0.fit_transform(df)
matrix([[ 1.,  0.,  0.],
        [ 0.,  0.,  1.],
        [ 0.,  1.,  0.]])
```

Scikit独热编码的缺点之一是它缺少ML工作流所需的`drop_last`功能。MLeap具有它自己的独热编码来启用该功能。

### MLeap独热编码

和Scikit独热编码非常类似，除了需要设置一个额外的`drop_last`属性。

```python
from mleap.sklearn.extensions.data import OneHotEncoder

## Vector Assembler for x1 One Hot Encoder
one_hot_encoder_tf = OneHotEncoder(sparse=False, drop_last=True) # Make sure to set sparse=False
one_hot_encoder_tf.mlinit(prior_tf=label_encoder_tf, output_features = '{}_one_hot_encoded'.format(label_encoder_tf.output_features))
#

# Construct our pipeline
one_hot_encoder_pipeline_x0 = Pipeline([
                                         (feature_extractor_tf.name, feature_extractor_tf),
                                         (label_encoder_tf.name, label_encoder_tf),
                                         (reshape_le_tf.name, reshape_le_tf),
                                         (one_hot_encoder_tf.name, one_hot_encoder_tf)
                                        ])

one_hot_encoder_pipeline_x0.mlinit()

# Execute our LabelEncoder + OneHotEncoder pipeline on our dataframe
one_hot_encoder_pipeline_x0.fit_transform(df)
matrix([[ 1.,  0.],
        [ 0.,  0.],
        [ 0.,  1.]])
```
