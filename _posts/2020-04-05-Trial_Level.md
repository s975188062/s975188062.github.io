---
title: "Trial层"
categories:
  - Eyelink
tags:
  - Experiment Builder
  - Prepare Sequence
  - Drift Action
  - Sequence
toc: true
comments: true
author_profile: false
sidebar:
  title: "目录"
  nav: EB
---

---

>实验设计回顾
>>![eb_exp_design_trial_level](/assets/images/eb_exp_design_trial_level.png)
>
>根据实验设计，我们除了需要在`Trial层`编辑`DriftCorrection`和`Recording层级`两个部分之外，还需要增加一个`PrepareSequence Action`控件。
>
>即：`PrepareSequence` -> `DriftCorrection` -> `Trial层`
>
>在`Block层`中双击编辑好的`Trial`进入`Trial层`内部开始编辑。

---

# 1. PrepareSequence Action

`PrepareSequence Action`控件放到此处有两个功能：**“预加载实验刺激材料”**和**“将实验材料等信息发送到主试机上以便监控实验”**。

## 1.1 预加载实验刺激材料

这部分工作`Experiment Builder`会自动识别需要加载的素材，我们不需要额外操作，只要将`PrepareSequence Action`从`控件库`中拉入工作区，连接箭头即刻。

![eb_add_preparesequence](/assets/images/eb_add_preparesequence.gif)

`PrepareSequence Action`的存在即可完成预加载实验刺激材料的工作，不需要额外设置。

## 1.2 将实验材料等信息发送到主试机上以便监控实验

此处实际上是两个控件的配合功能

* `PrepareSequence Action`设置将监控的页面发送到主试机；
* 在需要发送页面的`DisplayScreen Action`属性中设置发送该屏内容。

![eb_use_host_display_logic](/assets/images/eb_use_host_display_logic.png)

细心的同学可能已经发现了，当`Recoding层`中有多个`DisplayScreen Action`时，只能在其中选择一个投影到主试机上进行监测。

那么此处我们在`PrepareSequence Action`控件上需要进行的设置只有将`Draw to Eyelink Host`设置成`Image`即可。

![eb_set_prepare_sequence](/assets/images/eb_set_prepare_sequence.png)

---

# 2. DriftCorrection Action

将`DriftCorrection Action`从`控件库`中拖入工作区，连接箭头。

![eb_add_drift](/assets/images/eb_add_drift.gif)

根据实验设计，我需要将`DriftCorrection`注视点呈现的位置放到刺激文本段的段首第一个字上，从[指导语](/eyelink/Experiment_Level/#1-指导语)的部分我们已经学习到如何使用`Margin`功能来设置页边距了，那么在后面的刺激文本呈现环节将继续沿用相同的设置方法，即`上边距 - 80`，`左边距 - 100`。因此考虑将`DriftCorrection`注视点放到`100, 80`。

![eb_set_drift_location](/assets/images/eb_set_drift_location.png)

然后我们设置`DriftCorrection`的`Background Color`为中性灰`128, 128, 128, 255`。

>关于`DriftCorrection Action`的详细技巧，您可以查阅[关于Drift Correction 你应该知道更多](/eyelink/Drift/)

---

# 3. Recording层

向工作区添加一个新的`Sequence`，连接箭头，更名为“Recording”。

再将`Record`属性更改为`True`。设置在运行`Recording层`内部的内容时，同步记录眼动数据。`Record层`以外的部分不会记录眼动数据。

![eb_add_record_level](/assets/images/eb_add_record_level.gif)

---

以上。