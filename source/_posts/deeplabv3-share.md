---
title: DeepLabV3神经网络实战-苹果图片的区域分割
date: 2023-07-02 16:00
summary: 这是之前所做项目的一部分，使用了DeepLabV3模型对苹果区域进行可分割
categories: 图像处理
tags:
  - 深度学习
  - 语义分割
  - Python
---
## DeepLabV3神经网络介绍

该网络是于2018年提出的语义分割模型，以deeplabv3为encoder为架构，在此基础上加入了Decoder模块细化分割结果。并将Depthwise separable convolution在ASPP和Decoder上的应用。

![](http://rx4hz3911.hd-bkt.clouddn.com/e3896d4867794435be4c037a46d2b152.png)

**Encoder：**
首先图片进入Encoder里面进行特征提取，经过DCNN（深度卷积神经网络）生成两个有效特征层，分别为浅层特征层和深层特征层，浅层特征层的高和宽会大一些，而深层特征层的下采样会多一些，所以高和宽会小一些。在Encoder中，我们会使用不同膨胀率的膨胀卷积进行特征提取，其中有膨胀率分别为6,12,18的3x3卷积，用来提高网络的感受野，使得网络有不同的特征感受情况，之后将特征层进行堆叠，再经过1x1卷积进行通道数调整，获得绿色特征层。

**Decoder:**
 由DCNN生成的浅层特征层进入到Decoder解码器中，由编码器生成的具有高语义信息的绿色特征层进入到Decoder中进行上采样，之后与较浅的特征经过1x1卷积得到的结果进行特征融合，之后经过3x3的卷积进行特征提取，最终经过上采样将输出图片与输入图片大小一致，得到预测结果。

## 项目实践

{% blockquote %}
之前所做的项目需要分割出苹果健康的区域，损坏的区域，以及背景，以便后续的图像处理操作，采用了该模型进行语义分割处理。
本项目数据集来自kaggle https://www.kaggle.com/datasets/sergeynesteruk/apple-rotting-segmentation-problem-in-the-wild
{% endblockquote %}

### 模型训练
#### 训练参数
|            | 描述                                   |
| ---------- | -------------------------------------- |
| 所选模型   | deeplabv3_resnet101模型                |
| epoch      | 60                                     |
| batch      | 8                                      |
| 图像增强   | 0.5概率随机剪裁，0.5概率双线性插值缩放 |
| learn_rate | 0.0003                                 |

#### 训练曲线
![](http://rx4hz3911.hd-bkt.clouddn.com/img.png)
![](http://rx4hz3911.hd-bkt.clouddn.com/img_1.png)

### 结果分析
采用平均交并比评价训练结果，其中miou:86.11584745762715,结果较好
{% blockquote %}
黑色部分为背景，绿色部分为健康区域，红部分为腐败破损区域
第一行为原图，第二行为目标图，第三行为预测图
{% endblockquote %}
![](http://rx4hz3911.hd-bkt.clouddn.com/img_3.png)
![](http://rx4hz3911.hd-bkt.clouddn.com/img_3.png)