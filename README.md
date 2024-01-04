# 舌象诊断项目

这个项目实现了使用舌头图像进行诊断，它包括两个主要部分： Tongue Recognition 和 Tongue Coating Recognition。

## Tongue Recognition

使用 U-Net 进行舌头识别，并将舌头分割出来。

### 功能

- 从 JSON 文件中创建分割掩膜（Mask）
- 加载和标准化图像数据
- 构建 U-Net 模型
- 训练模型并进行验证
- 保存训练好的模型

### 数据预处理

- **创建掩膜**：从 JSON 标注文件中读取多边形标注，并创建对应的掩膜图像。
- **图像加载**：加载图像文件，并将它们转换为数组格式。
- **标准化**：将图像数据标准化到 0-1 范围内。

### 模型结构

U-Net 模型包含编码器（下采样）和解码器（上采样）部分，以及在最底层的桥接层。

**输入层**
- 输入尺寸为 `512 × 512 × 3`（高度，宽度，颜色通道）。

**编码器部分（下采样）**
- 包含 4 个编码器块（`encoder_block`）。
- 每个编码器块由一个 `conv_block` 和一个 `MaxPooling2D` 层构成。
- `conv_block` 包括两层卷积，每层卷积后跟着一个批量归一化和 ReLU 激活函数。
- 卷积核大小均为 `3 × 3` ，填充方式为 'same'。
- 第一个编码器块使用 32 个卷积核，之后每个块的卷积核数量翻倍（64, 128, 256）。

**桥接部分**
- 一个 `conv_block`，使用 512 个卷积核。
- 后面跟着一个 `Dropout` 层，dropout 率为 0.5。

**解码器部分（上采样）**
- 包含 4 个解码器块 `decoder_block`。
- 每个解码器块开始于一个 `Conv2DTranspose` 层，卷积核大小 `2 × 2`，步长为 (2, 2)，用于上采样。
- `Conv2DTranspose` 的输出与相应编码器的输出进行连接 `concatenate`。
- 连接后，跟着一个 `conv_block`，该 `conv_block` 与编码器中的类似，但没有池化层。
- 解码器块的卷积核数量依次为 256, 128, 64, 32。

**输出层**
- 一个 `Conv2D` 层，卷积核大小为 `1 × 1`，使用 1 个卷积核，激活函数为 `sigmoid`。

**编译模型**
- 使用 `Adam` 优化器。
- 损失函数为 `binary_crossentropy`。
- 性能指标为 `accuracy`。

**架构总结**
- U-Net 模型由 4 层编码器和 4 层解码器构成，每个编码器和解码器块包含两层卷积。
- 卷积核数量在编码器中逐渐增加，而在解码器中逐渐减少。最终通过一个 1 × 1 卷积核的输出层产生模型预测。

### 使用说明

1. 将图像和 JSON 标注文件放在指定文件夹中。
2. 运行数据预处理脚本来准备训练数据。
3. 运行模型构建和训练脚本。
4. 训练完成后，模型会被保存。
