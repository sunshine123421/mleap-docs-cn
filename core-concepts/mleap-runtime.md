# MLeap运行时

MLeap运行时是一个轻量级的机器学习引擎。它有如下特点：

1. 数据帧， leap frames支持所有通用数据类型和自定义数据类型
2. 转换器，目前支持所有Spark转换器和许多扩展转换器
3. 工作流, 很容易从转换器创建工作流
4. 完全集成MLeap Bundle，MLeap运行时为希望实现自己序列化方案的客户提供了一个参考实现
5. 序列化格式使得很容易发送leap frame的内容
6. [BLAS](https://github.com/scalanlp/breeze)提供运行非常快的线性代数系统

[MLeap运行使用章节](../mleap-runtime/index.md)包含更多怎样在应用中使用MLeap运行时的信息
