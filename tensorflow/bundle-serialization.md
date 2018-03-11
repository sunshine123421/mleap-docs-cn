# Tensorflow Bundle序列化

当将Tensorflow转换器序列化为一个MLeap Bundle，我们会将Tensorflow图存储为Protobuf格式的文件，对于Tensorflow图，你需要首先使用[freeze_graph](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/python/tools/freeze_graph.py)冻结你的Tensorflow图。这将确保执行图所需的一切都在图的定义文件中。

## 简单的MLeap Tensorflow Bundle

下载一个使用Tensorflow添加两个浮点数示例的MLeap Bundle: [MLeap Tensorflow Bundle](../assets/bundles/tensorflow-model.zip).

注意：点击右键并点击“另存为...”，Gitbook会阻止直接点击在链接上。
