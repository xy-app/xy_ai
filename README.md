# 官方 AI 视觉与智能推理套件 (AI Vision & Inference Suite) 🤖

[![Version](https://img.shields.io/badge/version-0.49.1-blue.svg)](manifest.json)
[![Plugin ID](https://img.shields.io/badge/plugin__id-xy__ai-green.svg)](manifest.json)
[![Group](https://img.shields.io/badge/group-AI-orange.svg)](manifest.json)
[![Platform](https://img.shields.io/badge/platform-All-lightgrey.svg)](manifest.json)

`xy_ai` 是官方 AI 视觉与智能推理套件，为自动化与业务流程提供高可用、多模态的端侧智能推理与计算机视觉识别能力。套件基于 **Shimmy (GGUF)** 引擎提供本地大语言模型服务与标准 OpenAI 接口，并基于 **ONNX Runtime (ort)** 深度整合了 **PaddleOCR**、**PP-Structurev2**、**Florence-2**、**YOLOv8/v11**、**CNN 超分扫码** 及 **通用图像分类** 等多模态视觉能力。


## 📑 目录

- [核心特性](#-核心特性)
- [动作功能概览](#-动作功能概览)
  - [1. 本地大模型服务与推理 (LLM)](#1-本地大模型服务与推理-llm)
  - [2. 文字识别与定位 (OCR & Detection)](#2-文字识别与定位-ocr--detection)
  - [3. 智能视觉等待与表格分析 (UI & Structure)](#3-智能视觉等待与表格分析-ui--structure)
  - [4. 条码识别与生成 (Barcode & QR)](#4-条码识别与生成-barcode--qr)
  - [5. 多模态视觉与深度学习模型 (VLM & CV)](#5-多模态视觉与深度学习模型-vlm--cv)
- [动作详细参数参考](#-动作详细参数参考)
- [典型实战场景](#-典型实战场景)
- [版本历史](#-版本历史)



## ✨ 核心特性

- **本地轻量大模型驱动**：内置基于 Shimmy (GGUF) 的模型服务引擎，一键启停并对外暴露兼容 OpenAI 的 HTTP 接口，支持结构化 JSON Schema 约束输出。
- **全流程高精 OCR 体系**：基于 PaddleOCR ONNX 算子流水线，集成方向分类与检测识别，并提供支持坐标返回的条件文字查找，可直接充当自动化点击/分支路由依据。
- **异步防抖视觉等待**：提供支持屏幕局部 ROI 区域的文字与元素轮询等待机制，彻底消除界面异步加载带来的竞态异常。
- **多模态 VLM 意图定位**：集成微软 Florence-2 视觉大模型，支持自然语言短语（Phrase Grounding）定位目标 UI 元素与屏幕密集描述。
- **工业级扫码鲁棒性**：借助 CNN 目标检测与超分辨率图像校正，大幅提升模糊、小面积与倾斜扭曲场景下的条码/二维码识别率。
- **通用 CV 模型灵活拓展**：支持自定义加载 YOLOv8/v11 目标检测模型与 MobileNet/ResNet 分类模型。


## 🛠 动作功能概览

插件共提供 **9 个核心 AI 与视觉动作**：

| 动作标签 (Tag) | 显示名称 | 核心能力与适用场景 | 输出类型 |
| :--- | :--- | :--- | :--- |
| `OpenAiService` | 本地模型服务与推理 | 本地 GGUF 启停管理、OpenAI 兼容接口暴露、Prompt 对话与 JSON Schema 结构化输出 | `string` |
| `OcrGeneral` | 通用文字识别 | PaddleOCR ONNX 高精度识别，支持自动角度矫正与坐标框原数据输出 | `string` |
| `OcrFindText` | 文字查找定位 | 在图像或全屏检索指定文字，返回命中状态 (`found`) 与中心坐标，供条件判断或点击使用 | `boolean` |
| `VisualWait` | 视觉条件等待 | 周期轮询等待目标文字/元素在屏幕出现或消失，支持设定超时与 ROI 区域加速 | `boolean` |
| `OcrTable` | 表格结构提取 | 基于 PP-Structurev2 识别复杂表格，还原为 Markdown、JSON 或 HTML | `string` |
| `BarcodeRecognition` | 增强型条码二维码识别 | CNN 目标定位 + 超分校正扫码，解决倾斜、形变与模糊场景下的识别痛点 | `string` |
| `BarcodeGenerate` | 条码与二维码生成 | 依据文本编码生成条形码（Code 128、EAN-13 等）或二维码图片文件 | `string` |
| `Florence2` | 多模态视觉定位与解析 | 微软 Florence-2 模型，支持自然语言提示词意图定位 UI 元素、密集目标检测与区域 OCR | `string` |
| `ObjectDetection` | 通用目标检测 | YOLOv8/v11 ONNX (内置 NMS) 通用目标定位、分类与置信度过滤 | `string` |
| `ImageClassification` | 图像通用分类 | MobileNet/ResNet ONNX 通用图像分类预测与场景辨识 | `string` |


## 📖 动作详细参数参考

### 1. 本地大模型服务与推理 (LLM)

#### `OpenAiService` (本地模型服务与推理)
- **描述**：管理本地轻量 Shimmy (GGUF) 模型服务，对外暴露标准 OpenAI 兼容接口，并支持在流程内执行 Prompt 推理与原数据输出。
- **参数列表**：
  - `action` (*String*): 操作模式。可选 `chat`（执行对话）、`start_server`（启动服务）、`stop_server`（停止服务）、`status`（获取状态）。默认 `chat`。
  - `model_name` (*String*): 模型名称或远程模型标识。预设：`qwen2.5:7b`、`deepseek-r1:1.5b`、`llama-3.2:3b`。
  - `model_path` (*FilePicker*): 本地 `.gguf` 权重文件路径。
  - `port` (*Number*): 对外暴露的 HTTP 服务端口。预设：`11434`、`8000`、`11888`。默认 `11434`。
  - `prompt` (*String*): 输入给大模型的提示词或提问内容。
  - `system_prompt` (*String*): 系统预设角色与行为指令。默认 `You are a helpful assistant.`。
  - `temperature` (*Real*): 采样温度（生成随机性）。预设：`0.2`、`0.7`、`1.0`。默认 `0.7`。
  - `json_schema` (*String*): 可选 JSON Schema 规范，强制模型输出合规结构化数据。
- **输出**：`string`（模型生成的文本或结构化数据）


### 2. 文字识别与定位 (OCR & Detection)

#### `OcrGeneral` (通用文字识别)
- **描述**：基于 PaddleOCR ONNX (内置检测+角度分类+识别流水线) 的端到端文字提取，输出完整文本与包围框。
- **参数列表**：
  - `image` (*FilePicker*): 待识别图像路径或截屏内存变量。
  - `engine` (*String*): 推理引擎。预设：`auto`、`ppocr_v5`、`ppocr_v4`、`windows_native`。默认 `auto`。
  - `enable_angle_cls` (*Boolean*): 是否对倒置或旋转的文本行进行自动矫正。默认 `true`。
  - `score_threshold` (*Real*): 置信度阈值过滤。默认 `0.5`。
- **输出**：`string`（JSON 格式的文本块与坐标矩阵）

#### `OcrFindText` (文字查找定位)
- **描述**：在图像或当前全屏检索目标文字，输出命中布尔值 (`found`) 与中心坐标，可无缝对接点击动作。
- **参数列表**：
  - `image` (*FilePicker*): 待查找图像路径。**留空表示直接在当前全屏查找**。
  - `target_text` (*String*): 需要查找的目标关键字或正则表达式。
  - `match_mode` (*String*): 匹配模式。可选 `contains`（包含）、`exact`（全等）、`regex`（正则）、`fuzzy`（模糊相似度）。默认 `contains`。
  - `score_threshold` (*Real*): 相似度/置信度门限。默认 `0.6`。
- **输出**：`boolean`（是否命中目标文本）

### 3. 智能视觉等待与表格分析 (UI & Structure)

#### `VisualWait` (视觉条件等待)
- **描述**：周期性轮询等待目标文字或视觉元素在屏幕中出现或消失，支持超时控制，彻底解决界面异步渲染竞态。
- **参数列表**：
  - `wait_type` (*String*): 等待类型。可选 `text_appear`、`text_disappear`、`element_appear`、`element_disappear`。默认 `text_appear`。
  - `target_pattern` (*String*): 等待比对的目标文本或目标分类标签。
  - `timeout_seconds` (*Number*): 最大超时时间（秒）。默认 `10`。
  - `interval_ms` (*Number*): 轮询间隔（毫秒）。默认 `500`。
  - `search_region` (*String*): 限定搜索区域 `[x, y, w, h]`，设置局部 ROI 可大幅提升检测帧率。
- **输出**：`boolean`（等待是否成功）

#### `OcrTable` (表格结构提取)
- **描述**：基于 PP-Structurev2 模型识别图像中的复杂表格，并自动解析为排版数据。
- **参数列表**：
  - `image` (*FilePicker*): 输入表格图像路径。
  - `output_format` (*String*): 输出格式。可选 `markdown`、`json`、`html`。默认 `markdown`。
  - `score_threshold` (*Real*): 单元格与边框置信度阈值。默认 `0.5`。
- **输出**：`string`（指定格式的表格文本内容）

### 4. 条码识别与生成 (Barcode & QR)

#### `BarcodeRecognition` (增强型条码二维码识别)
- **描述**：基于 CNN/ONNX 目标定位与超分校正技术的高鲁棒性扫码引擎，攻克模糊、反光、破损、倾斜形变下的漏读难题。
- **参数列表**：
  - `image` (*FilePicker*): 包含条码/二维码的图像文件路径。
  - `barcode_format` (*String*): 目标码制。可选 `auto`、`qr_code`、`code_128`、`ean_13`、`data_matrix`。默认 `auto`。
  - `high_precision` (*Boolean*): 是否开启深度学习超分高精度模式。默认 `true`。
- **输出**：`string`（解码后的字符内容与元数据）

#### `BarcodeGenerate` (条码与二维码生成)
- **描述**：根据文本内容生成自定义规格的条码或二维码图像文件。
- **参数列表**：
  - `content` (*String*): 要写入的文本或链接内容。
  - `barcode_format` (*String*): 目标格式。可选 `qr_code`、`code_128`、`ean_13`。默认 `qr_code`。
  - `output_path` (*FilePicker*): 生成图片保存路径（PNG/JPG）。
  - `width` (*Number*): 图像宽度（像素）。默认 `300`。
  - `height` (*Number*): 图像高度（像素）。默认 `300`。
- **输出**：`string`（保存成功的本地路径）


### 5. 多模态视觉与深度学习模型 (VLM & CV)

#### `Florence2` (多模态视觉定位与解析)
- **描述**：微软 Florence-2 多模态视觉模型，支持通过自然语言短语语义定位界面元素、物体边界框及复杂场景解析。
- **参数列表**：
  - `image` (*FilePicker*): 待分析图片路径或屏幕截屏。
  - `task_type` (*String*): 视觉任务类型。可选：
    - `caption_to_phrase_grounding` (短语定位元素坐标)
    - `object_detection` (通用目标检测)
    - `detailed_caption` (图像详细描述)
    - `ocr_with_region` (带区域检测的 OCR)
    默认 `caption_to_phrase_grounding`。
  - `prompt` (*String*): 意图短语 / 自然语言提示（如：`"确认支付按钮"`、`"右上角关闭"`）。
  - `model_path` (*FilePicker*): 可选自定义 Florence-2 ONNX 路径，留空使用内置默认模型。
- **输出**：`string`（JSON 格式的目标包围框与语义标签）

#### `ObjectDetection` (通用目标检测)
- **描述**：基于 YOLOv8 / YOLOv11 ONNX 的通用实时目标定位与类别预测，内置高效 NMS 算子。
- **参数列表**：
  - `image` (*FilePicker*): 待检测图像路径。
  - `model` (*FilePicker*): YOLO ONNX 权重文件路径。
  - `confidence` (*Real*): 候选目标置信度阈值。默认 `0.5`。
  - `framework` (*String*): 底层推理引擎，默认 `onnx`。
  - `labels` (*FilePicker*): 类别映射文本文件。
- **输出**：`string`（检测结果目标列表与检测框）

#### `ImageClassification` (图像通用分类)
- **描述**：加载通用分类网络（MobileNet / ResNet 等）对图像进行全局特征提取与分类。
- **参数列表**：
  - `image` (*FilePicker*): 待分类图像路径。
  - `model` (*FilePicker*): 分类模型 ONNX 路径。
  - `labels` (*FilePicker*): 标签列表文件路径。
  - `framework` (*String*): 推理引擎，默认 `onnx`。
  - `softmax` (*Boolean*): 是否对输出 Logits 执行 Softmax 归一化。默认 `true`。
- **输出**：`string`（分类概率排名结果）

## 💡 典型实战场景

### 场景 1：无界面原生控件时的自然语言意图定位与点击
在无法通过 DOM 或 UI 自动化树获取句柄的复杂客户端/远程桌面上：

```text
1. [VisualWait] 等待结算页面加载
   - wait_type: "text_appear"
   - target_pattern: "订单结算"
   - timeout_seconds: 15

2. [Florence2] 通过语义意图查找动态按钮位置
   - task_type: "caption_to_phrase_grounding"
   - prompt: "红色提交订单按钮"
   
3. 提取返回坐标并驱动系统鼠标完成点击
```

### 场景 2：离线文档批量结构化提取与报表生成

对扫描件与票据进行自动结构化归档：

```text
1. [BarcodeRecognition] 优先扫描发票上的追溯二维码
   - barcode_format: "qr_code"
   - high_precision: true

2. [OcrTable] 提取单据中的费用明细表
   - output_format: "markdown"
   - score_threshold: 0.6

3. [OpenAiService] 传入本地 LLM 进行信息归纳与字段标准化
   - model_name: "qwen2.5:7b"
   - prompt: "将以上 Markdown 表格提取为符合会计系统的标准 JSON"
   - json_schema: "{\"type\":\"object\",\"properties\":{...}}"

```
