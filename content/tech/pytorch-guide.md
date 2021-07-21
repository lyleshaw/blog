---
title: "PyTorch使用指南"
date: 2020-08-17T22:16:47+08:00
toc: false
images:
tags:
- PyTorch
- 机器学习
---

> ***本文首发于[https://lyleshaw.com/tech/wechat-bot/](https://lyleshaw.com/tech/wechat-bot/)***

## 为什么我要写这些？

这将是**一个系列**的长文，我希望通过撰写这些文章使得一个*对深度学习原理有一定理解*的人快速上手pytorch。

在深度学习建模方面的框架主要为tensorflow和pytorch，前者虽然在前些年占据优势，但是更适合工业级的部署任务，其计算图和*仿佛在python里重构了一门新语言*的设计非常劝退新手。相比之下，pytorch就更适合入门了。

尽管pytorch相较于tensorflow较为容易，但是因其功能的复杂性，还是很容易让新手踩坑。（是的，说的是我本人，debug de到自闭，甚至跑起来时候都不敢相信）。

## 这个系列会有哪些模块？

我会分以下模块分别写文章来解释及列出常见错误原因：

+ [Torch安装](https://lyleshaw.com/tech/install-pytorch/)
+ [数据引入](https://lyleshaw.com/tech/import-data/)
+ 数据清洗
+ 模型设计
+ 模型运行
+ 模型保存&调用
+ 模型优化

## 参考

[1.] 常用代码片段. Lyle. [https://github.com/lyleshaw/Frequently-used-Python-code](https://github.com/lyleshaw/Frequently-used-Python-code).

[2.] PyTorch 模型训练实用教程. 余霆嵩.