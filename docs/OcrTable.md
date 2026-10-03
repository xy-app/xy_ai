# 表格结构提取 (OcrTable)

基于百度 PP-Structurev2 表格分析与结构还原模型，自动定位图片中的表格区域、识别内部网格拓扑、对齐跨行跨列单元格并提取文字，直接还原为标准 Markdown 表格、结构化 JSON 数组或 HTML。

## 权限与平台要求
* **平台要求**：全平台通用 (Windows / macOS / Linux)

## 运行参数

| 参数名 | 类型 | 默认值 | 描述 |
| :--- | :--- | :--- | :--- |
| `image` | MixedType (FilePicker) | `""` | 包含表格的图像文件路径 |
| `output_format` | String | `markdown` | 输出格式：`markdown`, `json`, `html` |
| `score_threshold` | MixedType (Real) | `0.5` | 表格结构与文字置信度阈值 |

## 输出说明

* **输出变量**：格式化后的表格字符串，可直接联动 `xy_analysis` 转换为 DataFrame，或直接写入 Excel / Markdown 报表。
