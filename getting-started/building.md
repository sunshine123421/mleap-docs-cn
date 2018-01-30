# 从源码构建MLeap

MLeap is hosted on [Github](https://github.com/combust/mleap) as a
public project. Building MLeap is very straightforward and has very few
dependencies. Here are instructions for building from source.

MLeap作为一个公共项目放在[Github](https://github.com/combust/mleap)。构建
MLeap非常简单而没有过多的依赖。下面是从源码构建的指南。

## 安装SBT

[安装Simple Build Tool](http://www.scala-sbt.org/0.13/docs/Setup.html),
SBT被用于构建很多Scala项目。

## 编译MLeap核心

MLeap核心包括除了Tensorflow集成之外的所有子模块。我们将Tensorflow排除在核心构建之外
是因为Tensorflow的依赖安装比较困难。

### 克隆Github仓库

```bash
git clone https://github.com/combust/mleap.git
cd mleap
```

### 初始化Git Submodules

MLeap依赖于git submodule管理所有的protobuf定义，所以我们需要初始化并更新。

```bash
git submodule init
git submodule update
```

### 编译MLeap

MLeap项目由许多子模块组成，所有的子模块都聚合在SBT根项目下。这意味着，SBT发出的
命令会在所有子项目中执行。

```bash
sbt compile
```

### 运行测试用例

MLeap有大量的测试，包括完备的MLeap和Spark转换器之间的等同测试。

```bash
sbt test
```

## 编译Tensorflow支持

编译`mleap-tensorflow`子模块并不会自动运行。我们需要先编译Tensorflow，然后
安装Tensorflow Java jar包到本地maven2仓库。

### 编译/安装 Tensorflow

Tensorflow有很多编译和安装操作指南。

1. [Tensorflow](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/g3doc/get_started/os_setup.md)
2. [Tensorflow Java Bindings](https://github.com/tensorflow/tensorflow/tree/master/tensorflow/java).

### 构建Tensorflow MLeap模块

```bash
sbt mleap-tensorflow/compile
```

### 运行Tensorflow集成测试

```bash
TENSORFLOW_JNI=/path/to/tensorflow/library/folder/java sbt mleap-tensorflow/test
```
