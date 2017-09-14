# Core Concepts

MLeap以简单易行的方式使用几个核心构建块(Building Block)部署管道。

| 概念                 | 描述                         |
| ------------------- | -----------------------------|
| Data Frames  | 用于存储待转换的数据，类似于SQL表 |
| Transformers | 从DataFrame中提取数据进行计算，然后将结果放入新的DataFrame |
| Pipelines     |  用于对DataFrame执行一系列的转换操作 |
| Feature Unions - 仅限Scikit | 用于并行执行Pipeline操作，最后将结果合并 |
| MLeap Bundles | 以通用的JSON/Protobuf格式序列化存储机器学习Pipeline |
| MLeap Runtime | 在Java虚拟机中以轻量级的数据结构执行机器学习Pipeline |

这一章节的目的是向不太熟悉机器学习的人介绍管道的概念以及如何使用数据帧。即便如此，MLeap Bundle和Mleap Runtime的部分也应该对所有人都有所帮助。
