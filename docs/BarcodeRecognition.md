# 增强型条码二维码识别 (BarcodeRecognition)

基于深度学习卷积神经网络（WeChatQRCode CNN 定位超分与 YOLO-Barcode 架构）的高鲁棒性扫码套件。彻底解决传统规则扫码库（如 ZXing/rxing）在屏幕非整数缩放模糊、倾斜梯形变形、微小面积与暗光场景下无法检出定位符的致命痛点。

## 权限与平台要求
* **平台要求**：全平台通用 (Windows / macOS / Linux)

## 运行参数

| 参数名 | 类型 | 默认值 | 描述 |
| :--- | :--- | :--- | :--- |
| `image` | MixedType (FilePicker) | `""` | 包含一维码或二维码的图像文件路径 |
| `barcode_format` | String | `auto` | 码制类型：`auto`, `qr_code`, `code_128`, `ean_13`, `data_matrix` |
| `high_precision` | MixedType (Boolean) | `true` | 是否启用深度学习 CNN 定位与透视变换拉正增强 |

## 输出结构示例

```json
[
  {
    "content": "https://example.com/item/12345",
    "format": "QR_CODE",
    "confidence": 0.99,
    "rect": [120.0, 240.0, 180.0, 180.0]
  }
]
```
