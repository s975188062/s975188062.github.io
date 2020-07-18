---
title: "E-Prime眼动编程概述"
excerpt: "用复制粘贴的方法完成E-Prime眼动编程，简单得让你无法相信。"
read_time: false
header:
  overlay_image: /assets/images/eprime_header.png
  overlay_filter: 0.5
categories:
  - Eyelink
tags:
  - E-Prime
toc: true
comments: true
author_profile: false
sidebar:
  title: "目录"
  nav: E-Prime
---

---

# 1. 简介

固然E-Prime是一个广为接受的实验设计软件，在行为实验中的表现有目共睹。其短板也很明显，当需要连接设备进行采集的时候，实验编写的难度往往会提高不少。

仍然值得开心的是，Eyelink在API方面做的非常出色，其函数逻辑极其精简高效。

用E-Prime编写Eyelink眼动实验程序，只需三步：“复制 - 粘贴 - 修改”

---

# 2. 示例程序

如果您已经安装了Eyelink的DevelopmentKit，那么您的电脑里已经有了实例代码。

![eprime_demo_script_location](/assets/images/eprime_demo_script_location.png)

依次打开“开始菜单 - SR Research - Eyelink Examples - E-Prime Examples“，我们就可以看到实例代码的文件夹了。文件结构如下：

```
E-Prime
├── E-Prime1                    # E-Prime1 示例代码
├── E-Prime2 NoCameraImg        # E-Prime2 示例代码，叫准时无法看到眼睛图像
├── E-Prime2 WithCameraImg      # E-Prime2 示例代码，叫准时可以看到眼睛图像
└── Readme.txt                  # 以上三个文件夹的介绍，包括已知的一些Bug
```

以上三个版本的示例文件夹中，每个文件夹都包括4个不同的实验程序：

```
……
├── gcwindow                 # 移动窗口范式
├── linkevent                # 变化检测的高级实例，通过Eyelink Buffer检测眼动事件
├── picture                  # 自由浏览图片的基本范式
└── simple                   # 自由浏览文本的基本范式
```

---

# 3. 示例程序概览

我们以“picture”为例，打开E-Prime程序后，我们可以看到实验结构如下图所示：

![eprime_exp_structure](/assets/images/eprime_exp_structure.png)


