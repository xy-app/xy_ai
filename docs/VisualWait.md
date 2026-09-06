# 视觉条件等待 (VisualWait)

周期性轮询等待目标文字或视觉元素在屏幕中出现或消失，支持超时控制与限定区域检索，彻底解决界面异步加载、弹窗延迟出现的竞态问题，杜绝低效脆弱的固定睡眠延时 (`Sleep`)。

## 权限与平台要求
* **平台要求**：全平台通用 (Windows / macOS / Linux)

## 运行参数

| 参数名 | 类型 | 默认值 | 描述 |
| :--- | :--- | :--- | :--- |
| `wait_type` | String | `text_appear` | 等待类型：`text_appear`(等待文字出现), `text_disappear`(等待文字消失), `element_appear`(等待元素出现), `element_disappear`(等待元素消失) |
| `target_pattern` | MixedType (String) | `""` | 待匹配的关键字、正则规则或 YOLO 分类标签 |
| `timeout_seconds` | MixedType (Number) | `10` | 最大等待超时秒数（超出则判定失败） |
| `interval_ms` | MixedType (Number) | `500` | 轮询步进间隔毫秒数 |
| `search_region` | MixedType (String) | `""` | 可选限制区域 `[x, y, w, h]` (ROI)，大幅提升抓屏与推理帧率 |

## 输出说明

* `found` (Boolean)：在超时时间内是否成功满足等待条件；
* `elapsed_ms` (Number)：自开始等待至条件达成所花费的实际毫秒数；
* `center` (Point)：目标达成时的中心坐标（仅在 `appear` 模式且成功时有效）。
