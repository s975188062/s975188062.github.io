---
title: "实验的保存、编译、压缩与迁移"
excerpt: "EB Basic系列的最终章，涉及Save、Package、UnPack等功能。"
read_time: false
header:
  overlay_image: /assets/images/eb_package_n_deploy_header.png
  overlay_filter: 0.5
categories:
  - Eyelink
tags:
  - Experiment Builder
  - Package
  - Unpack
toc: true
comments: true
author_profile: false
sidebar:
  title: "目录"
  nav: EB
---

---

# 1. 保存实验

EB软件不会自动保存实验，因此我们要养成良好的保存习惯。经常使用`Ctrl+S`或者`Command⌘+S`来保存实验。

---

# 2. 编译导出

在实验完全编辑完成后，经`Test Run`测试符合设计要求，就可以编译导出实验程序了。

![eb_show_deploy](/assets/images/eb_show_deploy.png)

在工具栏中点击`Deploy`按钮，首先会跳出如下图所示的Confirm窗口，告知当前操作会清空`results文件夹`下的所有数据。正常情况下的`results文件夹`内是不会有正式实验数据的，所以点击`是(Y)`。

![eb_deploy_confirm_window](/assets/images/eb_deploy_confirm_window.png)

随后跳出窗口让我们指定保存的路径和文件夹名称，自行酌情调整。

![eb_deploy_window](/assets/images/eb_deploy_window.png)

我们打开编译导出后的文件夹，双击`.exe文件`即可运行实验。

![eb_deployed_folder](/assets/images/eb_deployed_folder.png)

> **⚠️注意⚠️**：在`Deploy`之前务必取消`Dummy Mode`的勾选，否则编译出来的实验程序也会是Dummy Mode的实验程序，无法链接眼动仪。

---

# 3. 实验程序打包与迁移



---
