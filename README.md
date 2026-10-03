# 🤖 官方 AI 视觉与智能推理套件 (AI Vision & Inference Suite)

[![Plugin Version](https://img.shields.io/badge/version-0.50.5-blue.svg)](manifest.json)
[![Group](https://img.shields.io/badge/group-AI-purple.svg)](#)
[![Platform](https://img.shields.io/badge/platform-All-green.svg)](#)
[![License](https://img.shields.io/badge/license-MIT-lightgrey.svg)](#)

专为桌面自动化、RPA 智能体（Agent）及工业质检场景打造的本地 AI 推理旗舰套件。深度融合 GGUF 本地端侧大模型运行时与 ONNX 工业级视觉引擎，提供零云端依赖的端到端文本与多模态智能：涵盖端侧 LLM 对话服务、端到端 OCR 文字检测与条件查找、异步视觉等待守卫、复杂结构化表格解析、深度学习增强型条形码/二维码双向生成识别、基于微软 Florence-2 的意图驱动 UI 定位，以及 YOLOv8/v11 实时目标检测与图像分类。

---

## 📖 简介 (Overview)

`xy_ai` 为流程自动化赋予了新一代“眼脑协同”的本地多模态感知能力：
- **端侧大语言模型服务 (`OpenAiService`)**：
  - 基于高效轻量的 Shimmy 引擎直连运行 `.gguf` 权重（如 Qwen 2.5、DeepSeek-R1、Llama 3.2）。
  - 内置 HTTP API 网关，一键向外暴露标准 **OpenAI 兼容接口**（默认 11434 端口），无缝对齐 Ollama/vLLM 生态。
  - 支持 System Prompt 角色设定、采样温度控制与 `json_schema` 格式化强制约束输出，保障下游业务流程稳定性。
- **全链路 OCR 与视觉定位体系**：
  - **通用文字识别 (`OcrGeneral`)**：内置 PaddleOCR (PP-OCRv4/v5) ONNX 检测 + 角度分类 + 识别完整流水线，输出带置信度的行级文字与包围框。
  - **条件文字查找 (`OcrFindText`)**：在图像或全屏中依据包含、完全、正则或模糊匹配检索关键字，直接回传布尔判断与中心点击坐标。
  - **视觉条件等待 (`VisualWait`)**：周期性轮询屏幕指定 ROI 区域，等待文字或视觉元素动态出现/消失，彻底解决现代动态界面异步渲染的竞态问题。
  - **结构化表格还原 (`OcrTable`)**：基于 PP-Structurev2 表格识别引擎，自动解析行列拓扑并导出为 Markdown、JSON 或 HTML 数据。
- **意图驱动的多模态视觉定位 (`Florence2`)**：
  - 引入微软先进的 Florence-2 视觉大模型，支持自然语言短语定位（Phrase Grounding，例如：“点击右上角关闭按钮”或“确认支付”）。
  - 支持密集目标检测、图像详细描述（Captioning）及基于区域的精确 OCR 识别。
- **深度增强条码系统 (`BarcodeRecognition`, `BarcodeGenerate`)**：
  - 扫码端基于 CNN/ONNX 预定位与超分辨率算法，极大改善微小条码、倾斜畸变、弱光和模糊条件下的检出率。
  - 生成端支持根据文本实时渲染高容错率二维码与主流一维码（Code 128 / EAN-13）。
- **通用检测与分类 (`ObjectDetection`, `ImageClassification`)**：
  - 集成 YOLOv8 / YOLOv11 ONNX 内置 NMS 模型，执行工业零件、瑕疵及屏幕特定元素的高速定位。
  - 支持 MobileNet / ResNet 等骨干网络图像场景辨识，内置 Softmax 概率归一化。

---

## ✨ 核心特性 (Features)

- **100% 本地端侧运行，隐私零外泄**：无论是 LLM 文本生成还是视觉模型推理，全部依靠本机 CPU/GPU 算力驱动，满足政企内外网隔离环境的严格合规要求。
- **OpenAI 兼容本地网关**：内置标准端点（`/v1/chat/completions`），既可在流程节点内即席 Prompt 调用，也可充当本地推理 Microservice。
- **意图驱动自动化 (Natural Language Grounding)**：告别脆弱的绝对像素坐标与选择器绑定，直接用自然语言短语指挥流程点击目标。
- **工业级 OCR 抗畸变校正**：内置行方向分类器（Angle Classifier），支持文字 $90^\circ$ / $180^\circ$ 倒置自适应摆正识别。
- **抗网络抖动的视觉轮询守卫**：支持限定 `search_region` 局部检测以压低 CPU 占用并提升轮询刷新率。

---

## 🛠️ 动作指令全景速查 (Action Catalog)

| 动作标识 (Tag) | 功能名称 | 引擎底座 | 描述 | 输出类型 |
| :--- | :--- | :--- | :--- | :--- |
| `OpenAiService` | 本地模型服务与推理 | Shimmy (GGUF) | 启停本地大模型服务或发起 Prompt 推理，支持 OpenAI 接口与 JSON Schema | `string` |
| `OcrGeneral` | 通用文字识别 | ONNX / Native | 端到端文本检测、方向校正与字符识别，输出文本行与外接坐标框 | `string` |
| `OcrFindText` | 文字查找定位 | PaddleOCR ONNX | 查找指定文本关键字，输出是否存在 (bool)、中心坐标点与包围框 | `boolean` |
| `VisualWait` | 视觉条件等待 | 视觉/OCR 轮询 | 周期性等待屏幕/区域中文字或元素的出现与消失，带超时控制 | `boolean` |
| `OcrTable` | 表格结构提取 | PP-Structurev2 | 提取图片中的复杂表格并结构化输出为 Markdown、JSON 或 HTML | `string` |
| `BarcodeRecognition`| 增强型条码二维码识别 | CNN + ONNX 超分 | 针对小面积、倾斜形变与低画质模糊场景的高鲁棒性扫码引擎 | `string` |
| `BarcodeGenerate` | 条码与二维码生成 | 原生绘图引擎 | 根据文本输入生成标准一维条码 (Code 128/EAN-13) 或二维码图片 | `string` |
| `Florence2` | 多模态视觉定位与解析 | Florence-2 ONNX | 自然语言短语意图定位屏幕 UI 元素、密集目标检测与界面描述 | `string` |
| `ObjectDetection` | 通用目标检测 | YOLOv8/v11 ONNX | 基于内置 NMS 的深度学习模型进行全画面实时目标定位与分类 | `string` |
| `ImageClassification`| 图像通用分类 | MobileNet/ResNet | 通用深度神经网络图像类别预测，支持 Softmax 概率分布输出 | `string` |

---

## 📚 详细参数说明与使用参考 (Detailed Reference)

### 1. 本地大模型服务与推理 (Local LLM & OpenAI Gateway)

#### `OpenAiService` - 本地模型服务与推理
- **输入参数：**
  | 参数名 (Field) | 显示名称 | 类型 | 默认值 | 预设推荐值 | 说明 |
  | :--- | :--- | :--- | :--- | :--- | :--- |
  | `action` | 操作模式 | `string` | `chat` | `chat`, `start_server`, `stop_server`, `status` | `chat`: 单次对话推理；`start_server`: 启动常驻后台 HTTP 服务 |
  | `model_name` | 模型标识 | `string` | `qwen2.5:7b` | `qwen2.5:7b`, `deepseek-r1:1.5b`, `llama-3.2:3b` | 模型名称标识 |
  | `model_path` | 本地 GGUF 模型路径 | `FilePicker` | `""` | - | 本地 `.gguf` 格式的权重物理绝对路径 |
  | `port` | 服务监听端口 | `number` | `11434` | `11434`, `8000`, `11888` | 对外暴露的标准 OpenAI 兼容 HTTP 服务端口 |
  | `prompt` | 用户提示词 (Prompt) | `string` | `""` | - | 用户输入的主提示词或任务指令 |
  | `system_prompt` | 系统提示词 (System) | `string` | `You are a helpful assistant.` | - | 约束大模型角色行为准则的 System 提示词 |
  | `temperature` | 采样温度 | `real` | `0.7` | `0.2`, `0.7`, `1.0` | 控制模型输出的发散度与确定性 |
  | `json_schema` | 结构化输出模式 (Schema)| `string` | `""` | - | JSON 模式规范，强制输出严格遵循的结构化对象 |
- **输出类型：** `string`（大模型生成的回答内容或服务状态描述）

---

### 2. OCR 智能文字识别与定位 (OCR & Visual Finding)

#### `OcrGeneral` - 通用文字识别
- **输入参数：**
  | 参数名 (Field) | 显示名称 | 类型 | 默认值 | 预设推荐值 | 说明 |
  | :--- | :--- | :--- | :--- | :--- | :--- |
  | `image` | 待识别图像 | `FilePicker` | `""` | - | 本地图像文件路径或内存图像变量引用 |
  | `engine` | 推理引擎 | `string` | `auto` | `auto`, `ppocr_v5`, `ppocr_v4`, `windows_native` | ONNX 模型版本或系统原生 OCR 后端 |
  | `enable_angle_cls` | 自适应方向校正 | `boolean` | `true` | `true`, `false` | 自动矫正旋转或倒置文本行方向 |
  | `score_threshold` | 置信度阈值 | `real` | `0.5` | `0.3`, `0.5`, `0.7`, `0.9` | 过滤低置信度字符结果的门限值 |
- **输出类型：** `string`（包含文本内容、各行多边形包围盒及置信度的 JSON 结构串）

#### `OcrFindText` - 文字查找定位
常用于界面条件分支判断或找字点击交互（Find & Click）。
- **输入参数：**
  - `image` (*FilePicker*): 待查找图像路径，**留空默认对当前主屏幕全屏查找**。
  - `target_text` (*string*): 目标关键字或正则表达式。
  - `match_mode` (*string*, 默认 `contains`): `contains` (包含匹配), `exact` (完全匹配), `regex` (正则匹配), `fuzzy` (模糊相似度)。
  - `score_threshold` (*real*, 默认 `0.6`): 相似度或 OCR 识别置信度阈值。
- **输出类型：** `boolean`（找到返回 `true`，否则 `false`；上下文附带注入中心坐标 `[x, y]` 与包围框）

#### `VisualWait` - 视觉条件等待
- **输入参数：**
  | 参数名 (Field) | 显示名称 | 类型 | 默认值 | 预设推荐值 | 说明 |
  | :--- | :--- | :--- | :--- | :--- | :--- |
  | `wait_type` | 等待类型 | `string` | `text_appear` | `text_appear`, `text_disappear`, `element_appear`, `element_disappear` | 等待文字或视觉元素的出现/消失状态切换 |
  | `target_pattern` | 目标匹配规则 | `string` | `""` | - | 待匹配的目标关键字或分类目标标签 |
  | `timeout_seconds`| 最大超时(秒) | `number` | `10` | `5`, `10`, `30`, `60` | 超时未达成状态则抛出或返回失败 |
  | `interval_ms` | 轮询间隔(毫秒) | `number` | `500` | `200`, `500`, `1000` | 两次屏幕截取检测之间的等待间隙 |
  | `search_region` | 限定搜索区域 | `string` | `""` | `[100, 100, 400, 300]` | `[x, y, w, h]` 局部区域，缩小检测面大幅提高 FPS |
- **输出类型：** `boolean`

#### `OcrTable` - 表格结构提取
- **输入参数：**
  - `image` (*FilePicker*): 输入表格截图或扫描件。
  - `output_format` (*string*, 默认 `markdown`): `markdown`, `json`, `html`。
  - `score_threshold` (*real*, 默认 `0.5`): 单元格预测置信度门限。
- **输出类型：** `string`（格式化表格文本）

---

### 3. 多模态视觉大模型定位 (Florence-2 Multimodal Grounding)

#### `Florence2` - 多模态视觉定位与解析
利用自然语言短语直接定位屏幕目标或理解复杂画面的内容。
- **输入参数：**
  | 参数名 (Field) | 显示名称 | 类型 | 默认值 | 预设推荐值 | 说明 |
  | :--- | :--- | :--- | :--- | :--- | :--- |
  | `image` | 输入图像 | `FilePicker` | `""` | - | 待分析图像文件路径或截屏内存数据 |
  | `task_type` | 视觉任务类型 | `string` | `caption_to_phrase_grounding` | `caption_to_phrase_grounding`, `object_detection`, `detailed_caption`, `ocr_with_region` | `caption_to_phrase_grounding`: 自然语言短语意图定位；`detailed_caption`: 详细描述画面；`object_detection`: 密集目标检测 |
  | `prompt` | 意图短语 / 提示词 | `string` | `""` | `确认支付按钮`, `右上角关闭图标` | 定位目标时使用的自然语言描述短语 |
  | `model_path` | Florence-2 ONNX 路径 | `FilePicker` | `""` | - | 自定义模型物理路径，留空使用套件内置权重 |
- **输出类型：** `string`（匹配区域坐标包围框四元组与解析结果）

---

### 4. 深度学习条形码与二维码系统 (Barcode Engine)

#### `BarcodeRecognition` - 增强型条码二维码识别
- **输入参数：**
  - `image` (*FilePicker*): 输入图像文件路径。
  - `barcode_format` (*string*, 默认 `auto`): `auto`, `qr_code`, `code_128`, `ean_13`, `data_matrix`。
  - `high_precision` (*boolean*, 默认 `true`): 开启深度学习 CNN 粗定位与超分重建算法，攻克反光、磨损与倾斜扫码。
- **输出类型：** `string`（解码文本与码块边界坐标）

#### `BarcodeGenerate` - 条码与二维码生成
- **输入参数：**
  - `content` (*string*): 待编码的字符串内容或链接。
  - `barcode_format` (*string*, 默认 `qr_code`): `qr_code`, `code_128`, `ean_13`。
  - `output_path` (*FilePicker*): 保存图片的目标文件路径。
  - `width` / `height` (*number*, 默认 `300x300`): 生成图像的像素宽高尺寸。
- **输出类型：** `string`（落盘绝对路径）

---

### 5. 目标检测与图像分类 (YOLO & Classification)

#### `ObjectDetection` - 通用目标检测
- **输入参数：**
  - `image` (*FilePicker*): 待检测的输入画面。
  - `model` (*FilePicker*): ONNX 模型权重文件（如 `yolov8n.onnx`，集成原生 NMS）。
  - `confidence` (*real*, 默认 `0.5`): 置信度过滤门限。
  - `framework` (*string*, 默认 `onnx`): 底层推理执行后端。
  - `labels` (*FilePicker*): 类别标签映射文本（每行对应一个类名）。
- **输出类型：** `string`（检测到的目标列表，包含类别标签、置信度分数及 `[x, y, w, h]` 坐标）

#### `ImageClassification` - 图像通用分类
- **输入参数：**
  - `image` (*FilePicker*): 待分类图像路径。
  - `model` (*FilePicker*): 分类网络 ONNX 模型文件。
  - `labels` (*FilePicker*): 类别清单文本。
  - `framework` (*string*, 默认 `onnx`): 推理后端。
  - `softmax` (*boolean*, 默认 `true`): 是否对输出 Logits 向量进行指数归一化概率计算。
- **输出类型：** `string`（Top-K 类别及归一化概率分数）

---

## 📦 插件清单定义 (Manifest Reference)

```json
{
  "plugin_id": "xy_ai",
  "name": "官方 AI 视觉与智能推理套件 (AI Vision & Inference Suite)",
  "version": "0.50.5",
  "group": "AI",
  "group_icon": "🤖",
  "description": "提供基于 GGUF 的本地模型服务与 OpenAI 接口，以及基于 ONNX 视觉引擎的通用文字识别、条件文字查找、视觉等待、深度增强扫码、Florence-2 意图定位与目标检测等能力",
  "actions": [...]
}
```

---

## 📄 许可证 (License)

本项目遵循 MIT 开源协议。
