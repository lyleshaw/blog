---
title: "PyTorch使用指南——数据引入"
date: 2020-08-17T22:36:36+08:00
toc: false
images:
tags:
- PyTorch
- 机器学习
---

> ***本文首发于[https://lyleshaw.com/tech/wechat-bot/](https://lyleshaw.com/tech/wechat-bot/)***

## 导入包

首先来一段正常的导入包代码，我将每个包的功能注释到代码后

```python
import torch # 引入Torch，没啥可说的
import torch.nn as nn # 引入神经网络模块，模型的定义需要用到nn.Model模块
import torch.nn.functional as F # 函数模块，包括机器学习里常用的一些计算函数
import pandas as pd #pandas，用来导入导出数据文件并做一定程度上的清洗
import numpy as np # numpy，python最好的科学计算库，常用在矩阵计算等
from torch.utils.data import * # 这个模块是数据相关的一些操作
from torch.autograd import Variable # variable函数是将tensor转化为可微分的variable类型
import torchvision # 这里面是一些计算机视觉常用的包和函数，一些
import matplotlib.pyplot as plt # plt画图用
import torch.optim as optim # 优化模块，有很多优化器，Adam、SGD等
from torch.utils.data import Dataset # 数据集操作
from torchvision import transforms, datasets, models # transforms是对数据进行totensor，归一化等等操作的包
```

> 可以根据不同任务的实际情况选择调用哪些包

## 数据导入

大部分网上关于pytorch的入门教程都是从mnist开始的，通常数据引入部分就是如下的代码

```python
train_loader = torch.utils.data.DataLoader(
    datasets.MNIST('../data', train=True, download=True,
                   transform=transforms.Compose([
                       transforms.ToTensor(),
                       transforms.Normalize((0.1307,), (0.3081,))
                   ])),
    batch_size=32, shuffle=True)
```

仅仅通过datasets里的MNIST就直接引入了手写字体的数据，虽然说用起来很方便，但是我想**大家应该不会只希望于用pytorch固定好的数据**，而更希望可以自定义数据进行训练和预测。

这时就需要用到如下代码（截取自某次任务）：

```python
tr_data = pd.read_csv("./data/train/outfile5.csv",encoding='gbk')
train = pd.read_csv('./data/train/outfile5.csv',usecols=[1,2],encoding='gbk')
train_label = pd.read_csv('./data/train/outfile5.csv',usecols=[29],encoding='gbk')

te_data = pd.read_csv("./data/val/lval.csv",encoding='gbk')
test = pd.read_csv('./data/val/lval.csv',usecols=[1,2],encoding='gbk')
test_label = pd.read_csv('./data/val/lval.csv',usecols=[29],encoding='gbk')

train_tensor = torch.Tensor(train.values)
train_label_tensor = torch.Tensor(train_label.values)
test_tensor = torch.Tensor(test.values)
test_label_tensor = torch.Tensor(test_label.values)

train_set = TensorDataset(train_tensor,train_label_tensor)
train_data = torch.utils.data.DataLoader(train_set, batch_size=batchsize,shuffle=True)
test_set = TensorDataset(test_tensor,test_label_tensor)
test_data = torch.utils.data.DataLoader(test_set, batch_size=batchsize,shuffle=True)
```

介绍一下，首先通过pandas里的read_csv函数将csv表格文件转换为dataframe（pandas的数据格式），存储在tr_data等变量中。

而后利用Tensor函数将其转换为tensor类型的数据，然后通过TensorDataset将数据和标签一起转换为dataset类型，最终使用DataLoader函数（经过各种变化）成为可以被训练的数据。

而对于图像类的数据，可以直接使用ImageFolder函数通过文件夹的方式分类读取，只要满足如下目录格式

```
train_data/
··········/label_1
··················/1.jpg
··················/2.jpg
··········/label_2
··················/3.jpg
··················/4.jpg
```

则读取的1.jpg 2.jpg的标签为label_1，读取的3.jpg 4.jpg的标签为label_2

以下为代码示例

```python
data_transform = transforms.Compose([transforms.RandomResizedCrop(224),transforms.RandomHorizontalFlip(),transforms.ToTensor(),transforms.Normalize((0.5,0.5,0.5),(0.5,0.5,0.5))])

train_img = torchvision.datasets.ImageFolder('data/train',
                                            transform=data_transform
                                            )
test_img = torchvision.datasets.ImageFolder('data/val',
                                            transform=data_transform
                                            )

train_data = torch.utils.data.DataLoader(train_img, batch_size=4,shuffle=True)
test_data = torch.utils.data.DataLoader(test_img, batch_size=4,shuffle=True)
```

当然，也可以通过继承ImageFolder类自定义图片读取方式。

总之，数据格式经历了如下变化：

> csv → dataframe → tensor → dataset → dataloader → (variable)[以后会讲]