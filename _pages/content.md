---
permalink: /content/
title: "Eyelink指北"
toc: true
---

---

>欢迎，这里是Charlie的Eyelink指北。内容在逐步更新中……
>
>您可以点击右侧的导航栏来快速跳转到您感兴趣的内容。
>
>如果有想要看到的内容，请通过邮件与我联系：Charlie-TechBlog@outlook.com
>
>您也可以发送一封邮件给我，这样我会把您加到邮件列表中，等我更新文章时会在第一时间通知您。

# 0. Intro

* 认识眼动研究
* 认识眼动仪

---

# 1. Experiment Builder

## 1.1 Basic

* [认识Experiment Builder](/eyelink/EB_Intro/)
* [编程前的实验设计准备](/eyelink/Experiment_Design/)
* Experiment层（DisplayScreen、Keyboard和Timer）
* Block层（Sequence和CameraSetup）
* Trial层（PrepareSequence和DriftCorrection）
* Recording层（Datasource和引用）
* 设置随机（Datasource）
* 计算反应时和正确率（Variable、UpdateAttribute和Conditional）
* 记录行为信息（ResultFile和AddToResultFile）
* 实验的保存、编译、压缩与迁移

## 1.2 Master

* 播放声音材料（DisplayScreen、PlayAudio和PlauAudioControl）
* 播放视频材料（DisplayScreen、PlayAudio和PlauAudioControl）
* 录音（RecordSound、RecordSoundControl和VoiceKey）
* 边界触发（InvisibleBoundary）
* GC-Window（DisplayScreen）
* Visual-World（DisplayScreen和Mouse）
* 多模态（SetTTL、BiometricTTL等）
* 真随机（Variable和UpdateAttribute）
* 绘制兴趣区InterestArea（DisplayScreen）

## 1.3 Tips

* [DriftCorrection Action](/eyelink/Drift/)

---

# 2. 主试机及操作

## 2.1 Basic

* [实验室环境配置](/eyelink/LabSetup/)
* 数据采集操作
* 常见问题汇总

## 2.2 Master

* [主试机系统更新教程](/eyelink/host-system-update/)

---

# 3. Data Viewer

## 3.1 Basic

* 认识Data Viewer
* 数据导入
* TrialGrouping
* 设置InterestPeriod
* 绘制InterestArea
* Report Data

## 3.2 Master

* 数据清洗
* 绘制动态兴趣区DynamicInterestArea
* 数据补救

---

# 4. Third-Party Programing Tools

## 4.1 E-Prime

* 概述
* elConnect
* elCameraSetup
* startRecording
* stopRecording
* elClose

## 4.2 PsychoToolBox

* 概述

## 4.3 PsychoPy

SR-Research公司的王治国博士已经就PsychoPy的内容创作了系统的工具书，此处直接引用王博士的劳动成果。

> This book will first introduce the building blocks and syntax of Python, then discuss libraries that we can use to program psychology experiments, i.e., PsychoPy and Pygame. For eye-tracking, this book will feature the Pylink library, a Python interface to the EyeLink Developer’s Kit. The example scripts accompanying this book are freely available on GitHub, https://github.com/zhiguo-eyelab/Pylink_book/tree/master/example_scripts. This book is a useful reference for eye-tracking researchers, but you can also use it in graduate or undergraduate level programming courses. ——王治国

**传送门**：[Zhiguo-Eyelab](https://github.com/zhiguo-eyelab/Pylink_book)

感谢王博士的无私分享。

## 4.4 Translog II

* 概述
* Project的制作
* 数据采集
* TPR_DB

## 4.5 辅助工具

* Visual-Angle Calculator

---

# 5. Weblink

## 准备中……
