# 模型压缩 — PaddleSlim

PaddleSlim 是一个模型压缩工具库，包含模型剪裁、定点量化、知识蒸馏、超参搜索和模型结构搜索等一系列模型压缩策略。

对于业务用户，PaddleSlim 提供完整的模型压缩解决方案，可用于图像分类、检测、分割等各种类型的视觉场景。
同时也在持续探索 NLP 领域模型的压缩方案。另外，PaddleSlim 提供且在不断完善各种压缩策略在经典开源任务的 benchmark,
以便业务用户参考。

对于模型压缩算法研究者或开发者，PaddleSlim 提供各种压缩策略的底层辅助接口，方便用户复现、调研和使用最新论文方法。
PaddleSlim 会从底层能力、技术咨询合作和业务场景等角度支持开发者进行模型压缩策略相关的创新工作。

## 功能

- 模型剪裁
  - 卷积通道均匀剪裁
  - 基于敏感度的卷积通道剪裁
  - 基于进化算法的自动剪裁

- 定点量化
  - 在线量化训练（training aware）
  - 离线量化（post training）

- 知识蒸馏
  - 支持单进程知识蒸馏
  - 支持多进程分布式知识蒸馏

- 神经网络结构自动搜索（NAS）
  - 支持基于进化算法的轻量神经网络结构自动搜索
  - 支持 One-Shot 网络结构自动搜索
  - 支持 FLOPS / 硬件延时约束
  - 支持多平台模型延时评估
  - 支持用户自定义搜索算法和搜索空间

## 安装

依赖：

Paddle >= 1.7.0

```bash
pip install paddleslim -i https://pypi.org/simple
```

## 使用

- [快速开始](https://paddleslim.readthedocs.io/zh_CN/develop/quick_start/index.html)：通过简单示例介绍如何快速使用 PaddleSlim。
- [进阶教程](https://paddleslim.readthedocs.io/zh_CN/develop/tutorials/index.html)：PaddleSlim 高阶教程。
- [模型库](https://paddleslim.readthedocs.io/zh_CN/develop/model_zoo.html)：各个压缩策略在图像分类、目标检测和图像语义分割模型上的实验结论，包括模型精度、预测速度和可供下载的预训练模型。
- [API 文档](https://paddleslim.readthedocs.io/zh_CN/develop/api_cn/index.html)
- [Paddle 检测库](https://github.com/PaddlePaddle/PaddleDetection/tree/master/slim)：介绍如何在检测库中使用 PaddleSlim。
- [Paddle 分割库](https://github.com/PaddlePaddle/PaddleSlim/tree/develop)：介绍如何在分割库中使用 PaddleSlim。
- [PaddleLite](https://paddlepaddle.github.io/Paddle-Lite/)：介绍如何使用预测库 PaddleLite 部署 PaddleSlim 产出的模型。

## 部分压缩策略效果

### 分类模型

数据: ImageNet2012; 模型: MobileNetV1;

|压缩策略 |精度收益(baseline: 70.91%) |模型大小(baseline: 17.0M)|
|:---:|:---:|:---:|
| 知识蒸馏(ResNet50)| **+1.06%** |-|
| 知识蒸馏(ResNet50) + int8 量化训练 |**+1.10%**| **-71.76%**|
| 剪裁(FLOPs-50%) + int8 量化训练|**-1.71%**|**-86.47%**|


### 图像检测模型

#### 数据：Pascal VOC；模型：MobileNet-V1-YOLOv3

|        压缩方法           | mAP(baseline: 76.2%)         | 模型大小(baseline: 94MB)      |
| :---------------------:   | :------------: | :------------:|
| 知识蒸馏(ResNet34-YOLOv3) | **+2.8%**      |       -       |
| 剪裁 FLOPs -52.88%        | **+1.4%**      | **-67.76%**   |
|知识蒸馏(ResNet34-YOLOv3)+剪裁(FLOPs-69.57%)| **+2.6%**|**-67.00%**|


#### 数据：COCO；模型：MobileNet-V1-YOLOv3

|        压缩方法           | mAP(baseline: 29.3%) | 模型大小|
| :---------------------:   | :------------: | :------:|
| 知识蒸馏(ResNet34-YOLOv3) |  **+2.1%**     |-|
| 知识蒸馏(ResNet34-YOLOv3)+剪裁(FLOPs-67.56%) | **-0.3%** | **-66.90%**|

### 搜索

数据：ImageNet2012; 模型：MobileNetV2

|硬件环境           | 推理耗时 | Top1 准确率(baseline:71.90%) |
|:---------------:|:---------:|:--------------------:|
| RK3288  | **-23%**    | +0.07%    |
| Android cellphone  | **-20%**    | +0.16% |
| iPhone 6s   | **-17%**    | +0.32%  |
