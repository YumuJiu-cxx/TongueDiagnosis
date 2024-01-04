# 舌象诊断项目

这个项目实现了使用舌头图像进行诊断，它包括两个主要部分：Tongue Recognition和Tongue Coating Recognition。

## Tongue Recognition

使用 U-Net 进行舌头识别，并将舌头分割出来。

### 功能

- 从JSON文件中创建分割掩膜（Mask）
- 加载和标准化图像数据
- 构建U-Net模型
- 训练模型并进行验证
- 保存训练好的模型

### 数据预处理

- **创建掩膜**：从JSON标注文件中读取多边形标注，并创建对应的掩膜图像。
- **图像加载**：加载图像文件，并将它们转换为数组格式。
- **标准化**：将图像数据标准化到0-1范围内。

### 模型结构

U-Net模型包含编码器（下采样）和解码器（上采样）部分，以及在最底层的桥接层。

**输入层**
- 输入尺寸为 `512 × 512 × 3`（高度，宽度，颜色通道）。

示例代码：

```python
selfModel = SelfModel()
selfModel.style_conver('result_path', 'content_image_path', 'style_image_path')
```

## 文件结构

项目包含以下文件和文件夹：

- `image/`：存放原始图像和风格图像的文件夹。
- `result/`：存放风格转换结果的文件夹。
