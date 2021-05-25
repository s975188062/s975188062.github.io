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
>如果有想要看到的内容，可以通过[首页表单](/)或者邮件与我联系：Charlie-TechBlog@outlook.com
>
>欢迎[订阅我的博客](/blog%20usage/add_rss_feed/)，在我更新文章时会在第一时间通知您。

# 0. Intro

* 认识眼动研究
* 认识眼动仪

---

# 1. Experiment Builder

## 1.1 Basic

* [认识Experiment Builder](/eyelink/EB_Intro/)
* [编程前的实验设计准备](/eyelink/Experiment_Design/)
* [Experiment层](/eyelink/Experiment_Level/)（DisplayScreen、Keyboard、Timer和Sequence）
* [Block层](/eyelink/Block_Level/)（CameraSetup）
* [Trial层](/eyelink/Trial_Level/)（PrepareSequence和DriftCorrection）
* [Recording层](/eyelink/Recording_Level/)（Datasource、引用和Test Run）
* [设置随机](/eyelink/set_trial_random/)（Datasource）
* [计算反应时和正确率](/eyelink/calcuate_rt_n_acc/#1-计算反应时)（Variable、UpdateAttribute和Conditional）
* [记录行为信息](/eyelink/calcuate_rt_n_acc/#23-保存到result-file)（ResultFile和AddToResultFile）
* [实验的保存、编译、压缩与迁移](/eyelink/exp_package_save_n_transfer/)

## 1.2 Master

* [拉丁方设计](/eyelink/latin_square_random/)
* [文字自动化分兴趣区-Auot_Word_Segment](/eyelink/eb_auto_word_segment/)
* 播放声音材料（DisplayScreen、PlayAudio和PlauAudioControl）
* 播放视频材料（DisplayScreen、PlayAudio和PlauAudioControl）
* 录音（RecordSound、RecordSoundControl和VoiceKey）
* 边界触发（InvisibleBoundary）
* GC-Window（DisplayScreen）
* Visual-World（DisplayScreen和Mouse）
* 多模态（SetTTL、BiometricTTL等）
* 真随机（Variable和UpdateAttribute）

## 1.3 单控件精讲

* [DriftCorrection Action](/eyelink/Drift/)

---

# 2. 主试机及操作

## 2.1 Basic

* [实验室环境配置](/eyelink/LabSetup/)
* [Eyelink软件安装](/eyelink/install_software/)
* [数据采集操作](/eyelink/data_collection/)
* 常见问题汇总

## 2.2 Master

* [主试机系统更新教程](/eyelink/host-system-update/)

---

# 3. Data Viewer

## 3.1 Basic

* [认识Data Viewer](/eyelink/DV_Intro/)
* [数据导入](/eyelink/dv_import_data/)
* [TrialGrouping](/eyelink/Trial_Grouping/)
* [设置InterestPeriod](/eyelink/dv_set_IP/)
* [绘制InterestArea](/eyelink/dv_set_ia/)
* [Report Data](/eyelink/dv_report_data/)

## 3.2 Master

* [数据清洗](/eyelink/dv-4_stage_fixation_cleaning/)
* 绘制动态兴趣区DynamicInterestArea

---

# 4. Third-Party Programing Tools

* [第三方编程概述](/eyelink/3rd-intro/)
* [第三方编程 Command 函数可用参数参考文档](/eyelink/3rd-programing/3rd_comand_ini/)
* 第三方编程 Message 函数可用参数参考文档

## 4.1 E-Prime

* [总体概述](/eyelink/e-prime/eprime_overview/)
* [User Script](/eyelink/e-prime/eprime-user_script/)
* [elConnect脚本](/eyelink/e-prime/eprime_elconnect/)
* [elCameraSetup脚本](/eyelink/e-prime/eprime_elCameraSetup/)
* [startRecording脚本](/eyelink/e-prime/eprime_startRecording/)
* [stopRecording脚本](/eyelink/e-prime/eprime_stopRecording/)
* [elClose脚本](/eyelink/e-prime/eprime_elClose/)

## 4.2 PsychToolBox

* [概述](/eyelink/3rd-matlab_overview/)
* Picture.m

## 4.3 PsychoPy

SR-Research公司的王治国博士已经就PsychoPy的内容创作了系统的工具书，此处直接引用王博士的劳动成果。

> This book will first introduce the building blocks and syntax of Python, then discuss libraries that we can use to program psychology experiments, i.e., PsychoPy and Pygame. For eye-tracking, this book will feature the Pylink library, a Python interface to the EyeLink Developer’s Kit. The example scripts accompanying this book are freely available on GitHub, https://github.com/zhiguo-eyelab/Pylink_book/tree/master/example_scripts. This book is a useful reference for eye-tracking researchers, but you can also use it in graduate or undergraduate level programming courses. ——王治国

**传送门**：[Zhiguo-Eyelab](https://github.com/zhiguo-eyelab/Pylink_book)

感谢王博士的无私分享。

## 4.4 Translog II

* [概述](/eyelink/translog-ii/translog-overview/)
* Project的制作
* 数据采集

## 4.5 辅助工具

* [![vac](https://img.shields.io/badge/Visual_Angle_Calculator-v1.0-green)](/tools/tools_vac/)

## 4.6 数据读取

* [使用 Matlab/Python 读取 .edf 数据](/eyelink/3rd-read_edf/)

---

# 5. Weblink

## 准备中……
