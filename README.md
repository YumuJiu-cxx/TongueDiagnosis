# 舌象诊断项目

这个项目实现了使用舌头图像进行诊断，它包括两个主要部分：

## 功能

- **Tongue Recognition**：使用 U-Net 进行舌头识别，并将舌头分割出来。
- **Tongue Coating Recognition**：使用 U-Net 进行舌苔识别，并将舌苔分割出来。

## 功能介绍

### 1. Tongue Recognition

舌头分割模型。

#### 功能

- 从JSON文件中创建分割掩膜（Mask）
- 加载和标准化图像数据
- 构建U-Net模型
- 训练模型并进行验证
- 保存训练好的模型

示例代码：

```python
hubModel = HubModel()
hubModel.style_conver('result_path', 'content_image_path', 'style_image_path')
```

### 2. SelfModel 使用

- 创建 `SelfModel` 实例。
- 使用 `style_conver` 方法进行风格迁移。此方法需要内容图像路径、风格图像路径和结果保存路径。

示例代码：

```python
selfModel = SelfModel()
selfModel.style_conver('result_path', 'content_image_path', 'style_image_path')
```

## 文件结构

项目包含以下文件和文件夹：

- `image/`：存放原始图像和风格图像的文件夹。
- `result/`：存放风格转换结果的文件夹。
