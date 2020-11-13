---
title: "翻译研究工具 Translog-II 简介"
excerpt: "Translog 2000/2006/II 自从诞生以来一直被全世界范围内的翻译研究学者所广泛使用。其相较于传统的量化研究相比，融入了眼动数据的整合，从视觉信息提取的角度为翻译过程的量化研究提供了新的机会。"
read_time: false
header:
  overlay_image: /assets/images/translog-header.png
  overlay_filter: 0.5
  actions:
    - label: "Translog-II官网(需要翻墙)"
      url: "https://sites.google.com/site/centretranslationinnovation/translog-ii"
categories:
  - Eyelink
  - Translog-II
tags:
  - Translog-II
toc: true
comments: true
author_profile: false
sidebar:
  title: "目录"
  nav: EL_3rd
---

---

本文仅对 Translog-II 于 Eyelink 的联合数据采集进行介绍，关于 Translog-II 的详细信息请移步 Translog-II 官网询问作者。感谢 Arnt 与 Michael 等人对 Translog-II 作出的贡献。
{: .notice--info}

> Translog-II is a program to record and study human reading and writing processes on a computer. It is an instrument to acquire objective, digital data of human translation processes. As their predecessors, Translog 2000 and Translog 2006, also Translog-II consists of two main components: Translog-II Supervisor is used to create a project file and to replay recorded sessions. Translog-II User is used to run a text production experiments (a user reads, writes or translates a text). Translog-II produces a log files which contains user activity data of the reading, writing and translation processes, and which can be evaluated by external tools (see TPR-DB). （摘自[https://sites.google.com/site/centretranslationinnovation/translog-ii](https://sites.google.com/site/centretranslationinnovation/translog-ii)）
>
> Translog-II 是一种用于记录和研究读写过程的程序。它可以获取人类翻译过程并量化为客观的数字。同 Translog 2000 和 Translog 2006 一样，Translog-II 也包含 2 个主要组件：Translog-II Supervisor 用于创建项目并重播记录的数据。 Translog-II User 用于运行文实验项目（阅读，书写或翻译）。 Translog-II 将数据保存为一个日志文件，其中包含阅读，写入和翻译过程的被试活动数据。同时，可以通过外部工具进行评估（请参阅TPR-DB）。

# 1. Translog-II 的 Supervisor & User

## 1.1 Supervisor 

![translog-overview-supersivor_window](/assets/images/translog-overview-supersivor_window.png)

Supervisor 用于 Project 的创建与数据的回放及初步处理。

整个软件窗口也是 Windows 的**“标题栏 - 菜单栏 - 工具栏 - 工作区 - 状态栏”**的传统布局。

您可以点击工具栏中的 Create Project 来创建一个实验程序，也可以点击 Start Experiment 来进行实验。在实验结束后，点击 Open Log File 来回放和处理数据。

## 1.2 User

![translog-overview-user_window](/assets/images/translog-overview-user_window.png)

User 负责实验的刺激呈现和数据采集。采集过程示意如下：

![translog-overview-data_loging_window](/assets/images/translog-overview-data_loging_window.png)

## 1.3 CRITT TPR-DB

CRITT TPR-DB 全称为 CRITT Translation Process Database。

这是一个 Translog 研究数据处理和共享的数据库。您可以在上面看到世界范围内使用 Translog 进行研究的学者所收集到的数据。

同时 TPR-DB 也可以将 Translog 记录的数据转化为可以用作统计分析的 10 种报表。

## 1.4 Work Flow

1. 在 Supervisor 中设计实验的 Project ；
2. 在 User 中采集实验数据；
3. 在 Supervisor 中对数据进行预处理；
4. 在 TPR-DB 中导出报表。

---

# 2. Eyelink 与 Translog-II 的眼动数据整合

Eyelink 与 Translog-II 的作者 Michael Carl 先生于 2019 年底开始了深度合作。使 Translog-II 开始支持 Eyelink 全线的眼动仪型号。

Eyelink 在与 Translog-II 进行不同步数据采集时，秉承了其一贯的高采样和高精度特点。为翻译研究领域提供了真正意义上的高质量眼动数据。

采样率方面支持最高 2000Hz 的眼动数据采集。在提高了时间精确度的同时，也大幅降低了随机误差，在时间和空间两个维度提供最优质的数据。

与此同时，Eyelink 凭借其在在阅读相关研究中的积累多年的产品优势和 0.25～0.5 度的典型误差值，得以精确测量每一个单词的注视和眼跳行为。

**One More Thing!**

使用 Eyelink 与 Translog-II 进行联合数据采集时，可以同时保留 Eyelink 的 .edf 文件与 Translog-II 的 .xml 文件。

一个实验，两份数据。

美滋滋 ;P 

---

以上
