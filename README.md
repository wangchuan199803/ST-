# ST++
出自《ST++: Make Self-training Work Better for Semi-supervised Semantic Segmentation》，用于学习笔记

self-train是进行半监督分割（大部分图像没有标签，只有少部分有标签）训练的一种典型方式，具体来说：

<img width="247" alt="image" src="https://github.com/wangchuan199803/ST-plus-for-segment/assets/39644177/ef7d925c-2ffe-40ee-bb84-42062435c6fc">


上图就是self-train的基本流程，同时也是文中的ST结构，主要可以分为以下几步：
>step1. 在有标签的数据集 $D_l$ 上训练老师网络T\
>step2. 使用网络T给无标签的数据集 $D_u$ 打标签\
>step3. 在合并后的数据集 $D_u\cup D_l$ 上训练一个学生网络 $S$ ， 模型的优化目标由两种数据的误差组成

注：一般 $S$ 和 $T$ 会采用相同的结构，因此训练 $S$ 其实就是对 $T$ 的再训练

但是这种训练方式存在两个问题：
>1. 伪标签中的错误会累积进而降低模型性能；再训练中存在耦合问题， $D_u$ 中的伪标签由 $T$ 给出，因此 $S$ 会与 $T$ 做出相似的预测，无法学习额外的信息
>2. 不同未标记样本的伪标签的可靠性存在偏差

因为针对这两个问题，文中提出了两种解决方式，分别是：
>1. 在训练阶段在未标签数据集 $D_u$ 上使用强数据增强SDA（比如裁剪旋转）以增加多样性，作为基线ST
>2. 根据不同未标记样本的伪标签可靠性，对训练方式进行改进，获得ST++。具体内容往下看

**ST++算法流程**:\
<img width="243" alt="image" src="https://github.com/wangchuan199803/ST-plus-for-segment/assets/39644177/69fab5d5-cf03-4c0e-8db5-ae4458b293da">

简单来说，ST++就是：
>step1. 在有标签的数据集 $D_l$ 上训练网络 $T$ ，但是保留多个阶段的模型checkpoint\
>step2. 对于每个未标注样本，计算每个早期阶段模型预测和最终阶段模型预测的IOU，最终获得meanIOU作为可靠性分数\
>step3. 使用有标签的数据集 $D_l$ 和可靠性分数最高的前R个样本，对网络 $S$ 进行训练\
>step4. 使用训练得到的模型 $S$ 对可靠性分数低的剩余样本进行标注\
>step5. 使用所有数据进行对 $S$ 重新训练

## 数据集
Pascal VOC 2012 + SBD\
Cityscapes\
[Cityscapes leftImg8bit](https://pan.baidu.com/share/init?surl=08_NgFheDIpnQRrwz5uhmw) ，提取码：dhr8
[Cityscapes gtFine](https://drive.google.com/file/d/1E_27g9tuHm6baBqcA7jct_jqcGA89QPm/view?usp=sharing)
## 模型结构
骨干网络有四种：\
PSPNet\
ResNet-50\
DeepLabv3+ with ResNet50/101\
DeepLabv2 with ResNet-101
## Pre-trained Model
[ResNet-50](https://download.pytorch.org/models/resnet50-0676ba61.pth)\
[ResNet-101](https://download.pytorch.org/models/resnet101-63fe2227.pth)\
[DeepLabv2-ResNet-101](https://drive.google.com/file/d/14be0R1544P5hBmpmtr8q5KeRAvGunc6i/view?usp=sharing)





