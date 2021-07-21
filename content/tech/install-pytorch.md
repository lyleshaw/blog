---
title: "PyTorch使用指南——pytorch安装"
date: 2020-09-24T18:42:36+08:00
toc: false
images:
tags:
- PyTorch
- 机器学习
---

> ***本文首发于[https://lyleshaw.com/tech/wechat-bot/](https://lyleshaw.com/tech/wechat-bot/)***
>
> 最近好几个朋友在问pytorch安装的问题，总结了一下，大家好像都是【超时】和【路径】问题，因此在整个系列前些个0篇，介绍pytorch安装中的各种坑

## Q0 一些小问题

![1](https://s1.ax1x.com/2020/09/24/0pP0N4.png)

有朋友首先问了我这个问题...这个需要明确一个前提，即pytorch在pip中实际上并不叫pytorch，直接pip install pytorch是会报错的，应该使用[官网安装命令](https://pytorch.org/get-started/locally/)完成安装。

## Q1 超时——如何使用pip/conda镜像

其次常见的问题应该是time out，即超时错误。这是由于Torch仓库在海外，访问外网的速度会有所限制，而连接超过设定时间后即会自动终止安装，这时候就需要通过国内镜像来完成安装。

**以下提供部分常用镜像地址**
```
清华：https://pypi.tuna.tsinghua.edu.cn/simple
阿里云：http://mirrors.aliyun.com/pypi/simple/
中国科技大学 https://pypi.mirrors.ustc.edu.cn/simple/
豆瓣：http://pypi.douban.com/simple/
```

通过我们会使用清华源或阿里源

如果您不经常使用python，仅仅为了任务而使用pytorch，那么我建议您在该次安装中使用暂时的镜像，命令为

```
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple {Your Pack's Name}
```

对于Torch安装而言，则为

```
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple torch==1.6.0+cpu torchvision==0.7.0+cpu -f https://download.pytorch.org/whl/torch_stable.html
```

倘若您会经常使用python，那么我建议您直接将pip安装来源永久修改为清华源，方法如下：

首先找到如下路径，{Your User Name}为您的用户名：

```
C:\User\{Your User Name}\pip\pip.ini
```

其次修改pip.ini的内容为如下内容：

```
[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
[install]
trusted-host=mirrors.aliyun.com
```

一般而言，通过如上方法即可成功。

## Q2 安装失败？试试CPU版

如果上述安装过程出现如下错误

![2](https://s1.ax1x.com/2020/09/24/0pFrOx.png)

那么应该是您默认安装了CUDA版本，即GPU版本

这种情况下建议您安装CPU版本，即把下图中CUDA版本选择为“None”后的命令进行安装

![3](https://s1.ax1x.com/2020/09/24/0pFIXt.png)

## Q3 安装成功却无法import？路径错误

如果您出现![4](https://s1.ax1x.com/2020/09/24/0pFv1s.png)内容，您可以直接在CMD中输入python后打出import torch内容后没有报错，那么恭喜您，pytorch已经成功安装到了您的电脑。

但是不要着急，这并不意味着您可以将pytorch在生产环境中使用。

如图：![5](https://s1.ax1x.com/2020/09/24/0pkFNF.png)

这可能是由于您安装了多个python环境（包括但不限于原生python，anaconda等），就可能会产生将Torch安装到自己也不知道哪个环境的情况，结果即如上图。

此时有三个解决方案：

### 1. 卸载所有环境，只保留一个环境（建议anaconda）【简单】
### 2. 在所有环境中全部安装一遍Torch【中等】
### 3. 手动调整系统环境变量中的路径，将Torch安装到自己需要的环境中【较难】

> 以上即是安装pytorch中的常见问题，如果有其他问题可以向我发邮件，我会在看到后回复~