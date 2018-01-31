# 数据类型

Spark, MLeap, Scikit-learn和Tensorflow支持多种数据类型。幸运的是，所有这些技术都基于通用
的数学数据结构，因此它们很大程度上是相互兼容的。

数据帧把列数据类型存储在一个模式对象中。此模式决定了哪些操作可以在哪些列上执行，以及转换如何处理。

## 支持数据类型

| 数据类型 | 注释 |
|---|---|
| Byte | 8位整数, MLeap和Spark仅支持带符号整数 |
| Short | 16位整数, MLeap和Spark仅支持带符号整数 |
| Integer | 32位整数, MLeap和Spark仅支持带符号整数 |
| Long | 64位整数, MLeap和Spark仅支持带符号整数 |
| Float | 32位浮点数 |
| Double | 64位浮点数 |
| Boolean | 8位布尔值表示真或假，如果需要可以压缩到1个bit |
| String | 字符串，根据平台不同可以是预先定义长度或者不定长以NULL结尾 |
| Array | 以上基本类型的数组 |
| Tensor | MLeap和Tensorflow支持, 以上基本类型之一的n维存储空间 |
