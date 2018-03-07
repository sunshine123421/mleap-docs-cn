# MLeap服务

MLeap服务提供了一个轻量级的docker映像, 用于搭建MLeap模型的RESTful API服务。
该服务的操作非常简单, 你可以用它快速地搭建运行原型。

## 安装

MLeap服务是一个docker镜像,该镜像位于[Docker Hub](https://hub.docker.com/r/combustml/mleap-serving/).
开始安装之前, 请拉取该镜像到你的本地机器上:

```
docker pull combustml/mleap-serving:0.8.0
```

## 用法

为了将你的模型作为一个REST API来使用, 我们需要做以下事情:

1. 启动Docker里的服务器
2. 加载我们的模型到内存中
3. 变换成一个leap帧

### 启动服务器

为了开始变换数据, 请先启动Docker镜像。请确保将宿主机上包含你的模型的目录映射到容器里。
在这个例子里, 我们的模型被存储于`/tmp/models`目录里, 并且将这个目录映射到容器
里的`/models`目录。

```
mkdir /tmp/models
docker run -v /tmp/models:/models combustml/mleap-serving:0.8.0
```

这样, 你就可以通过本地的`65327`端口来访问模型服务器。

### 加载模型

使用curl命令将模型加载到内存中。如果你没有自己的模型，可以在我们的样例模型里进行下载。
当你启动服务器时, 请务必将这个模型放进你用于映射的存放模型的目录中。

1. [AirBnB Linear Regression](https://s3-us-west-2.amazonaws.com/mleap-demo/airbnb.model.lr-0.6.0-SNAPSHOT.zip)
2. [AirBnB Random Forest](https://s3-us-west-2.amazonaws.com/mleap-demo/airbnb.model.rf-0.6.0-SNAPSHOT.zip)

```
curl -XPUT -H "content-type: application/json" \
  -d '{"path":"/models/<my model>.zip"}' \
  http://localhost:65327/model
```

### Transform

接下来, 我们将利用我们的模型去变换一个JSON-encoded leap帧。
如果你正在使用我们的AirBnB样例模型, 你可以从下面的链接下载leap帧:

1. [AirBnB Leap Frame](https://s3-us-west-2.amazonaws.com/mleap-demo/frame.airbnb.json)

Save the frame to `/tmp/frame.airbnb.json` and then let's transform it
using our server.
存储帧于`/tmp/frame.airbnb.json`, 然后通过我们的服务器变换此帧。

```
curl -XPOST -H "accept: application/json" \
  -H "content-type: application/json" \
  -d @/tmp/frame.airbnb.json \
  http://localhost:65327/transform
```

你应该收到一个leap响应帧, 此帧是以JSON格式存储的, 你可以从中提取响应结果。
如果你当时用的是我们的AirBnB样例模型，那么在这个leap帧中的最后一个域是prediction。

### 卸载模型

如果因为某些原因，你希望保持服务器的运行状态，但是不想将任何模型加载到内存中,
只要删除`model`的资源:

```
curl -XDELETE http://localhost:65327/model
```
