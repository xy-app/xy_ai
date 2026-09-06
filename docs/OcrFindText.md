# 文字查找定位 (OcrFindText)

在指定图像或当前屏幕中查找目标关键字，输出布尔存在性结果、目标中心坐标点与区域包围框。作为流程中最重要的视觉条件判断节点，可直接驱动后续的鼠标点击或分支判断。

## 权限与平台要求
* **平台要求**：全平台通用 (Windows / macOS / Linux)

## 运行参数

| 参数名 | 类型 | 默认值 | 描述 |
| :--- | :--- | :--- | :--- |
| `image` | MixedType (FilePicker) | `""` | 待查找图像路径（留空则自动对当前屏幕全屏查找） |
| `target_text` | MixedType (String) | `""` | 需要匹配的目标文字、短语或正则表达式 |
| `match_mode` | String | `contains` | 匹配准则：`contains`(包含), `exact`(完全一致), `regex`(正则), `fuzzy`(模糊相似) |
| `score_threshold` | MixedType (Real) | `0.6` | 匹配判定门限或 OCR 置信度 |

## 输出说明

* `found` (Boolean)：是否找到目标，可直接作为 `xy_control` 中 `If` 动作的判断依据；
* `center` (Point)：匹配文字块的中心像素坐标 `(x, y)`，可直接连接鼠标移动与点击；
* `rect` (Rect)：文字块的外接矩形 `(x, y, w, h)`。

## 最佳实践
```mermaid
flowchart LR
    A[OcrFindText "查找'提交订单'"] --> B{If found == true}
    B -- 是 --> C[点击 center 坐标]
    B -- 否 --> D[抛出异常或重试]
```
