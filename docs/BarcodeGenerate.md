# 条码与二维码生成 (BarcodeGenerate)

根据指定的文本内容与格式规范，生成高保真的一维条形码 (Code128, EAN-13) 或二维码 (QR Code) 图像文件，广泛应用于电子面单、自动化测试与标识打印等业务流程。

## 权限与平台要求
* **平台要求**：全平台通用 (Windows / macOS / Linux)

## 运行参数

| 参数名 | 类型 | 默认值 | 描述 |
| :--- | :--- | :--- | :--- |
| `content` | MixedType (String) | `""` | 待编码的字符串内容或链接 URL |
| `barcode_format` | String | `qr_code` | 生成格式：`qr_code`, `code_128`, `ean_13` |
| `output_path` | MixedType (FilePicker) | `""` | 生成图片的目标存储路径 (.png / .jpg) |
| `width` | MixedType (Number) | `300` | 图像宽度（像素） |
| `height` | MixedType (Number) | `300` | 图像高度（像素） |

## 输出说明

* **输出变量**：已保存的图像文件完整绝对路径。
