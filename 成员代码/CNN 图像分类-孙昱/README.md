# MNIST 简单 CNN 图像分类

孙昱 2023211471

本项目使用一个简单的 CNN 模型完成 MNIST 手写数字分类，包含以下完整流程：

- 自动下载 MNIST 数据集
- 训练模型
- 测试模型
- 输出测试集上的样例分类结果

## 1. 文件说明

- `main.py`：数据下载、训练、测试、输出结果

## 2. 环境要求

- Python 3.8 及以上
- PyTorch
- torchvision

## 3. 安装依赖

```bash
pip install torch torchvision
```

## 4. 运行方式

```bash
python mnist_cnn.py
```

默认参数：

- epochs=3
- batch_size=64
- lr=0.001
- seed=42
- show_count=10

也可以自定义参数，如：

```bash
python mnist_cnn.py --epochs 5 --batch_size 128 --lr 0.001 --show_count 10
```

## 5. 输出内容说明

脚本运行后会输出：

- 当前设备（CPU 或 CUDA）
- 每个 epoch 的训练损失、训练准确率、测试损失、测试准确率
- 最后输出若干条测试样例的预测结果，格式如下：

```text
index=0, pred=7, label=7
index=1, pred=2, label=2
...
```

其中：

- pred：模型预测类别
- label：真实类别

## 6. 运行示例

![image](img\sample_run.png)

## 7. 流程总结

1. 设置随机种子
2. 下载并加载 MNIST 数据集
3. 构建简单 CNN 模型
4. 按 epoch 执行训练和测试
5. 输出测试样例预测结果