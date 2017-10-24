# MLeap Runtime

MLeap运行环境是一个轻量级的机器学习引擎。它有如下特点：

1. Data Frame, leap frames支持所有通用数据类型和自定义数据类型
2. Transformers，目前支持所有Spark transformers和许多扩展transformers
3. Pipelines, 很容易从transformers创建pipelines
4. 完全集成MLeap Bundle，MLeap运行环境为希望实现自己序列化方案的客户提供了一个参考实现
5. 序列格式使得很容易发送leap frame的内容
6. [BLAS](https://github.com/scalanlp/breeze)提供运行非常快的线性代数系统

[MLeap Runtime usage section](../mleap-runtime/index.md)包含更多怎样在应用中
使用MLeap Runtime的信息
