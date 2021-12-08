---
title: "在 EB 中播放声音刺激"
excerpt: "听觉是我们第二大的信息来源。在言语加工领域，越来越多的研究者将语音信息作为刺激手段呈现在自己的实验中。本次我们来看一看如何在 EB 中播放声音。"
read_time: false
header:
  overlay_image: /assets/images/eb-play_sound_header.png
  overlay_filter: 0.5
  teaser: /assets/images/eb-play_sound_taster.png
categories:
  - Eyelink
tags:
  - Experiment Builder
  - Play Sound
toc: true
author_profile: false
sidebar:
  title: "目录"
  nav: EB
---

---

# 0. 声卡的抉择

在学习如何播放声音之前，我们需要先了解一下播放声音的硬件——声卡。

声卡是用来将声音文件播放出来的硬件，根据性能不同，分为 集成声卡 和 ASIO声卡 两种。

**本节太长不看**：有条件的话尽量用 ASIO 声卡保证声音刺激呈现的时间精度，成本千元左右。实在不行用集成声卡也凑合。请跳到[<u>1. 播放声音</u>](/eyelink/eb-asio_sound_card/)继续阅读。
{: .notice--info}

## 0.1 集成声卡

简单来说，集成声卡在我们每个人的电脑里面都有，是和计算机的主板集成在一起的，故名集成声卡。

根据集成声卡的型号不同，其性能表现也会有所差异，这种差异主要体现在声音的播放延迟上。

对于计算机而言，声音文件的播放流程是这样的：

    1. 读取声音文件；
    2. 将声音文件发给声卡播放；
    3. 声卡播放出声音。

对于我们写的实验程序而言，我们所记录的声音开始播放的时间是第 2 步，将声音文件交给声卡播放的时候。而声音真的播放出来的时间则是第 3 步。第 2 步和第 3 步之间的误差就是我们所说的声音播放延迟。

这种延迟在所有电脑中普遍存在，细心的同学可能早就感受过自己电脑的音画不同步。具体的延迟数值根据电脑情况不同，在 10 ～ 500 ms 之间浮动。

众所周知，实验求稳。抖动的延迟对于数据的影响不可预知，所以集成声卡并不是实验的最佳选择。

## 0.2 ASIO 声卡

> ASIO 是 Audio Stream Input Output 的缩写。是由 Steinberg 公司开发的专业声卡驱动模式的一种简称。
> 
> 简单来说，ASIO 的目的是降低音频延迟；同时 ASIO 作为系统中独立的音频通道可以避开 DirectSound（或其他通道）的干扰，从而使得 ASIO 应用程序（如我们的实验刺激程序）可以不受系统中正在运行的其他程序干扰，达到降低声音播放和录制延迟的目的。同样一块声卡，假设使用 MME 驱动时的延迟时间为 750 毫秒，那么当换成 ASIO 驱动后延迟量就有可能会降低到 10 毫秒以下。

关于 ASIO 声卡的内容，Charlie 已经在[<u>**播放声音或录音？整块声卡！**</u>](/eyelink/eb-asio_sound_card/)中进行了介绍，此处不再赘述。

---

# 1. 播放声音

## 1.1 准备声音文件

在 EB 中，

## 1.2 设置声卡驱动

## 1.3 设置播放声音

### 1.3.1 Play Sound Action

### 1.3.2 Synchronize Audio

---

以上。