# 图像分类 (ImageClassification)

基于深度学习分类网络的高性能图像场景与目标类别识别动作。支持 ResNet、MobileNet 等常见视觉分类模型，用于图像状态判别与场景分类。

## 权限要求
> 无要求

## 子流程
> 不支持

## 运行参数

* `image` (Image)：待分类图像对象或图像文件路径。
* `model` (Path)：分类网络 ONNX 模型文件路径。
* `labels` (Path)：类别清单文本文件路径（每行对应一个类别名称）。
* `framework` (String)：推理执行后端（默认：`onnx`）。
* `softmax` (Boolean)：是否对模型输出的 Logits 向量进行 Softmax 指数归一化概率计算（默认：`true`）。

## 输出
> 图像分类结果 JSON 字符串（包含各候选类别的标签名称 `label`、置信度概率分数 `score` 以及排序列表）。

## 注意事项
* 若模型在导出时内部已包含 Softmax 操作，可将 `softmax` 参数设为 `false` 以避免重复归一化。
* 类别清单文件需与模型输出的维度顺序严格一致。
