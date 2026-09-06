# 通用文字识别 (OcrGeneral)

基于百度 PaddleOCR ONNX (PP-OCRv4 / PP-OCRv5) 工业级模型的端到端高精度文字识别套件，在底层自动串联文本检测 (DBNet)、角度分类 (ClsNet) 与文字识别 (SVTR/CRNN) 闭环流水线，并在 Windows 平台支持原生 `Windows.Media.Ocr` 免模型降级兜底。

## 权限与平台要求
* **平台要求**：全平台通用 (Windows / macOS / Linux)
* **硬件加速**：自适应适配 DirectML (Windows) / CoreML (macOS) / CPU

## 运行参数

| 参数名 | 类型 | 默认值 | 描述 |
| :--- | :--- | :--- | :--- |
| `image` | MixedType (FilePicker) | `""` | 待识别图像路径或当前屏幕截屏变量 |
| `engine` | String | `auto` | 推理引擎：`auto`(自适应优先), `ppocr_v5`, `ppocr_v4`, `windows_native`(原生系统OCR) |
| `enable_angle_cls` | MixedType (Boolean) | `true` | 是否自动检测并纠正倒置、旋转 90°/180°/270° 的文字行 |
| `score_threshold` | MixedType (Real) | `0.5` | 置信度过滤门限 (0.0~1.0) |

## 输出结构示例

输出友好型结构化业务数据：
```json
{
  "full_text": "订单编号: 20260904001\n合计金额: ￥1,280.00",
  "line_count": 2,
  "lines": [
    {
      "text": "订单编号: 20260904001",
      "score": 0.985,
      "rect": [50.0, 120.0, 260.0, 30.0],
      "center": [180.0, 135.0]
    }
  ]
}
```

## 行业对标
* 对标 UiPath Computer Vision OCR / 影刀通用文字识别。
