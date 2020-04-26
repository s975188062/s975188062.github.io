---
title: "计算反应时和正确率"
excerpt: "EB Basic系列的行为信息记录，主要涉及Variable、Update Attribute和Conditional三个控件的用法。"
read_time: false
header:
  overlay_image: /assets/images/eb_calcuate_rt_n_acc_header.png
  overlay_filter: 0.5
categories:
  - Eyelink
tags:
  - Experiment Builder
  - Variable
  - Update Attribute
  - Conditional Trigger
toc: true
comments: true
author_profile: false
sidebar:
  title: "目录"
  nav: EB
---

---

# 1. 计算反应时

有机体对刺激的反应并不能在受到刺激的同时就发生，从刺激的呈现到反应的开始之间会有一段时间间距。反应时间（reaction time, RT）就是指从刺激呈现到有机体做出明显的反应所需要的时间。

## 1.1 执行逻辑

我们会通过一个非常简单的算式来计算各种各样的行为指标，以`TextPage`的阅读用时为例：

假设`Text Page`在第3秒时呈现，被试在第5秒时按下“空格”结束阅读，那么`TextPage`的阅读的阅读时间为 5 - 3 = 2秒，即“阅读用时 = 被试按键的时间 - 刺激出现的时间”。

熟悉编程的人会了解如何通过代码定义和使用变量，大概的过程是：

`定义一个名为“XX”的对象` -> `声明“XX”的类型` -> `对”XX“进行赋值`

## 1.2 计算Text Page阅读用时



## 1.3 计算Question反应时



---

# 2. 记录Question回答结果
