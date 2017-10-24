# 支持的Transformers

下面是MLeap支持的各种机器学习平台的Transformer

注：Tensorflow没有Transformer支持列表，但是可以参考此链接在MLeap中引入Tensorflow graph
[include Tensorflow graphs in an MLeap transformer pipeline](../../integration/tensorflow/usage.html).

## 特征提取

| Transformer| Spark | MLeap | Scikit-Learn | TensorFlow |
|---|:---:|:---:|:---:|:---:|
| Binarizer | x | x | x | |
| BucketedRandomProjectionLSH | x | x | | |
| Bucketizer       | x | x |  | |
| ChiSqSelector | x | x | | |
| CountVectorizer       | x | x | | |
| DCT | x | x | | |
| ElementwiseProduct | x | x | x | |
| HashingTermFrequency | x | x | x | |
| IDF | x | x | | |
| Imputer   | x | x | x | |
| Interaction | x | x | x | |
| MaxAbsScaler | x | x | | |
| MinHashLSH | x | x | | |
| MinMaxScaler | x | x | x |  |
| Ngram | x | x | | |
| Normalizer | x | x | | |
| OneHotEncoder | x | x | |
| PCA | x | x | x | |
| QuantileDiscretizer | x | x | | |
| PolynomialExpansion | x | x | x | |
| ReverseStringIndexer | x | x | x | |
| StandardScaler | x | x | x | |
| StopWordsRemover | x | x | | |
| StringIndexer | x | x | x | |
| Tokenizer | x | x | x | |
| VectorAssembler | x | x | x | |
| VectorIndexer | x | x | | |
| VectorSlicer | x | x | | |
| WordToVector | x | x | | | |

## 分类

| Transformer | Spark| MLeap | Scikit-Learn  | TensorFlow |
|---|:---:|:---:|:---:|:---:|
| DecisionTreeClassifier | x | x | x | |
| GradientBoostedTreeClassifier | x | x | | |
| LogisticRegression | x | x | x | |
| LogisticRegressionCv | x | x | x | |
| NaiveBayesClassifier | x | x | | |
| OneVsRest | x | x | | |
| RandomForestClassifier | x | x | x | |
| SupportVectorMachines | x | x | x | |
| MultiLayerPerceptron | x | x | | | |

## 回归

| Transformer | Spark | MLeap | Scikit-Learn | TensorFlow |
|---|:---:|:---:|:---:|:---:|
| AFTSurvivalRegression | x | x | | |
| DecisionTreeRegression | x | x | x | |
| GeneralizedLinearRegression | x | x | | |
| GradientBoostedTreeRegression | x | x | | |
| IsotonicRegression | x | x | | |
| LinearRegression | x | x | x | |
| RandomForestRegression | x | x | x | | |


## 聚类

| Transformer | Spark | MLeap | Scikit-Learn | TensorFlow |
|---|:---:|:---:|:---:|:---:|
| BisectingKMeans | x | x | | |
| GaussianMixtureModel | x | x | | |
| KMeans | x | x | | |
| LDA | x | | | | |

## 扩展
| Transformer | Spark | MLeap | Scikit-Learn | TensorFlow | Description |
|---|:---:|:---:|:---:|:---:|:---|
| MathUnary | x | x | x | | Simple set of unary mathematical operations |
| MathBinary | x | x | x | | Simple set of binary mathematical operations |
| StringMap | x | x | x | | Maps a string to a double |

## 推荐算法
| Transformer | Spark | MLeap | Scikit-Learn | TensorFlow |
|---|:---:|:---:|:---:|:---:|
| ALS | x | | | | |
