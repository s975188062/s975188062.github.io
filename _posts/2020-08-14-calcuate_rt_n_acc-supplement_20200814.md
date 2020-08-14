---
title: "反应时和正确率的计算与记录 - 补充更新 1"
excerpt: "近日收到多位老师反应，刺激屏幕的按键反应存在延迟，按键后不会立即显示下一屏的内容。借此机会，向大家解释一下问题产生的原因，顺便加深大家对 DisplayScreen 这个控件的理解。"
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
  - Result File
  - Add to Result File
toc: true
comments: true
author_profile: false
sidebar:
  title: "目录"
  nav: EB
---

---

近日来，有几位老师在跟随 Charlie 一起学习 Experiment Buidler 的时候发现了一个问题：

> 被试做完按键反应之后，屏幕反应很慢，要很久才回跳动。

这个问题很有意思，与计算机系统的逻辑有关。

# 1. 为什么会有这个问题

在讨论这个问题之前，不如我们先讨论一下电脑显示器是如何工作的。

首先，仍然，电脑很“**笨**”。

电脑是收到我们程序命令来显示刺激的，我们命令他显示什么，他就显示什么。

除了要命令他开始显示以外，还要命令他结束显示。但实际上，结束显示就是显示空屏的意思。

> 真是个拿起来就不知道放下的铁憨憨。

在我们的实验设计中，如下图所示，`Display_Question`后进行 `Keyboard_of_Question` 按键反应。且在按键反应后没有新的 DisplayScreen 控件来控制计算机显示新的内容。

![eb-rt_n_acc-supplyment_2-hole_sequence](/assets/images/eb-rt_n_acc-supplyment_2-hole_sequence.png)

此时计算机就会 “盼君来” 一般地乖乖等待下一个显示命令。

而下一个显示命令 `Display_TextPage` （根据设置可能是 `DriftCorrection`）又如 “渣男” 一般，在下一个试次的开始才会出现。

整个等待过程包括上图中绿色的计算过程和试次间的一些准备操作，相对耗时。

被试在按键后，屏幕没有立即反馈，虽然只等了 100～200ms ，但依然很不舒服。

---

# 2. 解决办法

解决的方法其实非常简单粗暴。

我们在 `Keyboard_of_Question` 这个键盘控件之后增加一个 `DisplayScreen` 控件即可。

将这个 `DisplayScreen` 控件命名为 “Blank_Screen”，且无需显示任何内容。

![eb-rt_n_acc-supplyment_2-insert_blank_screen](/assets/images/eb-rt_n_acc-supplyment_2-insert_blank_screen.png)

那么在按键反应之后，会立即呈现空屏，不再有延迟感出现。

---

以上。