# 目标检测 (ObjectDetection)

基于深度学习视觉推理引擎的高性能目标检测动作。支持 YOLOv8 / YOLOv11 等集成 NMS 的模型，实现工业零件、特定目标、瑕疵以及屏幕特定 UI 元素的高速定位。

## 权限要求
> 无要求

## 子流程
> 不支持

## 运行参数

* `image` (Image)：待检测的输入画面对象或图像文件路径。
* `model` (Path)：ONNX 模型权重文件路径（如 `yolov8n.onnx`，集成原生 NMS）。
* `confidence` (Number)：目标检测置信度阈值（默认：`0.5`，过滤低于该置信度的候选框）。
* `framework` (String)：推理执行后端（默认：`onnx`）。
* `labels` (Path)：类别标签映射文件路径或标签列表文本（每行对应一个类别名称）。

## 输出
> 检测到的目标列表 JSON 字符串（包含各目标的类别标签 `label`、置信度分数 `confidence` 以及包围框坐标 `box: [x, y, width, height]`）。

## 注意事项
* 模型建议使用带有内建 Non-Maximum Suppression (NMS) 的标准 ONNX 导出权重，可大幅简化前后处理并提升推理帧率。
* 若置信度设置过低可能引入误检，建议根据场景在 `0.4` 至 `0.7` 区间内微调。
