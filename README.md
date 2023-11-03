# ST++
出自《ST++: Make Self-training Work Better for Semi-supervised Semantic Segmentation》，用于学习笔记

self-train是进行半监督分割（大部分图像没有标签，只有少部分有标签）训练的一种典型方式，具体来说：

<img width="247" alt="image" src="https://github.com/wangchuan199803/ST-plus-for-segment/assets/39644177/ef7d925c-2ffe-40ee-bb84-42062435c6fc">

上图就是self-train的基本流程，主要可以分为以下几步：

step1. 在有标签的数据集$D_l$上训练老师网络T
step2. 使用网络T给无标签的数据集$D_u$打标签
step3. 在合并后的数据集$D_u\cup D_l$上训练一个学生网络S
