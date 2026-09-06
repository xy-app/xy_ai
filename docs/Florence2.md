# 多模态视觉定位与解析 (Florence2)

微软开源的超紧凑多模态视觉大模型 (0.23B / 0.77B)，支持通过自然语言短语定位屏幕 UI 元素 (`<CAPTION_TO_PHRASE_GROUNDING>`)、开放目标检测、密集区域描述与带坐标的高精度文字提取，专门解决无 DOM 句柄环境下的自适应元素交互痛点。

## 权限与平台要求
* **平台要求**：全平台通用 (Windows / macOS / Linux)
* **硬件加速**：支持 DirectML / CoreML / CUDA / CPU

## 运行参数

| 参数名 | 类型 | 默认值 | 描述 |
| :--- | :--- | :--- | :--- |
| `image` | MixedType (FilePicker) | `""` | 待分析的图像路径或当前截屏变量 |
| `task_type` | String | `caption_to_phrase_grounding` | 任务类型：`caption_to_phrase_grounding`(短语定位), `object_detection`(目标检测), `detailed_caption`(密集描述), `ocr_with_region`(区域OCR) |
| `prompt` | MixedType (String) | `""` | 意图提示短语（例如：“蓝色登录按钮”、“关闭对话框图标”） |
| `model_path` | MixedType (FilePicker) | `""` | 可选自定义 Florence-2 ONNX 目录，留空使用官方默认权重 |

## 输出结构示例

```json
[
  {
    "label": "蓝色登录按钮",
    "box": [320.0, 450.0, 480.0, 490.0]
  }
]
```
