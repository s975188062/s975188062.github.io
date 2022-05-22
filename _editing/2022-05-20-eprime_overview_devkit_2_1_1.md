---
title: "E-Prime眼动编程概述(E-Prime with DevKit v2.1.1)"
excerpt: "用复制粘贴的方法完成E-Prime眼动编程，简单得让你无法相信。"
read_time: false
header:
  overlay_image: /assets/images/eprime_header.png
  overlay_filter: 0.5
  actions:
    - label: "下载示例文件(提取码：vo0m)"
      url: "链接: https://pan.baidu.com/s/1A8QKilCfR1CNSBa_xsqKeA
categories:
  - Eyelink
  - E-Prime
tags:
  - E-Prime
toc: true
comments: true
author_profile: false
sidebar:
  title: "目录"
  nav: EL_3rd_Eprime
---

---

> 本篇是基于安装了 `2.1.1` 版本 Development Kit 的用户，如果您安装的是 `1.11.5` 版本的 Development Kit ，请点击 [>>>传送门<<<](/eyelink/e-prime/eprime_overview/)。您可以在 “开始菜单 - SR Research - SR Version” 中查看 `eyelink_core.dll` 的版本来确定您的 Development Kit 版本。
{: .notice--warning}

# 1. 简介

路径依赖，亦或是进度紧迫来不及学习 Experiment Builder，总会有很多需要用 E-Prime 进行眼动实验的时候。

虽然如此，Charlie 还是推荐大家使用 Eyelink 自家的 Experiment Builder 运行含眼动的实验。不是说 E-Prime 不好，只是每个软件都有各自的专长和限制。

不过不用担心，得益于 Eyelink 精简高效的 API，只要你了解了眼动实验的基本流程，在 E-Prime 中完成一个眼动实验也不是一个特别复杂的事儿。

只需要 “复制 - 粘贴 - 修改”。

---

# 2. 示例程序

安装好 Eyelink Development Kit 后，您可以在 “开始菜单 - SR Research - Eyelink Sample Experiments” 中找到示例程序文件夹。

![eprime-overview_v2-sample_exp_folder](/assets/images/eprime-overview_v2-sample_exp_folder.png)

E-Prime 示例就在 E-Prime 文件夹中。其内部文件结构如下：

```
E-Prime
├── E-Prime 1               # E-Prime1 示例代码
    ├── gcwindow            # 移动窗口范式
    ├── linkevent           # 变化检测的高级实例，通过Eyelink Buffer检测眼动事件
    ├── picture             # 自由浏览图片的基本范式
    └── simple              # 自由浏览文本的基本范式
├── E-Prime 2 and 3         # E-Prime2 示例代码，叫准时无法看到眼睛图像
    ├── EyeLink_FixWindowFastSamples    # 
    ├── EyeLink_GCFastSamples           # 
    ├── EyeLink_Picture                 #
    ├── EyeLink_PictureMRI              #
    ├── EyeLink_SaccadeEvent            #
    ├── EyeLink_SimpleCanvasSave        #
    └── EyeLink_Video                   #
└── Readme.txt                  # 示例介绍
```

---

# 3. 示例程序概览

---

以上。