# 本地模型服务与推理 (OpenAiService)

管理本地轻量 Shimmy (GGUF) 大模型服务，对外暴露标准 OpenAI 兼容 HTTP 接口，并支持在自动化工作流内直接执行 Prompt 提问与原数据输出。

## 权限与平台要求
* **平台要求**：全平台通用 (Windows / macOS / Linux)
* **权限要求**：无需管理员权限

## 核心工作模式

* **流程推理模式 (`action="chat"`)**：在工作流中直接向指定模型（本地或外部 OpenAI 兼容端点）发送提问，将回答文本或结构化数据直接赋给流程变量。
* **服务管理模式 (`action="start_server" / "stop_server" / "status"`)**：以 Sidecar 伴生守护进程在后台拉起轻量 Shimmy 推理服务（内存占用 <50MB），对外监听指定端口（如 11434），使外部工具（VS Code Continue, Cursor, 外部脚本）可将其当做本地私有 Ollama / vLLM 节点使用。

## 运行参数

| 参数名 | 类型 | 默认值 | 描述 |
| :--- | :--- | :--- | :--- |
| `action` | String | `chat` | 执行模式：`chat`(直接推理), `start_server`(启动服务), `stop_server`(停止服务), `status`(探活状态) |
| `model_name` | MixedType | `qwen2.5:7b` | 模型名称标识，如 `qwen2.5:7b`, `deepseek-r1:1.5b`, `llama-3.2:3b` |
| `model_path` | MixedType (FilePicker) | `""` | 本地 `.gguf` 权重文件路径（用于本地 Shimmy 加载） |
| `port` | MixedType | `11434` | 对外暴露的 OpenAI 兼容 HTTP 服务监听端口 |
| `prompt` | MixedType | `""` | 用户输入提问词或消息模板 |
| `system_prompt` | MixedType | `You are a helpful assistant.` | 大模型预设角色与行为指令 |
| `temperature` | MixedType | `0.7` | 采样温度 (0.0~2.0) |
| `json_schema` | MixedType | `""` | 可选 JSON Schema 字符串，强制模型严格以结构化 JSON 输出 |

## 输出说明

* **输出变量**：模型生成的回答文本（若指定了 `json_schema`，可直接解析为强类型 JSON 对象）。

## 行业对标
* 对标 UiPath GenAI Activities / 影刀大模型动作流 / Ollama 独立服务。
