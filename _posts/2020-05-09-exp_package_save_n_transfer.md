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

**⚠️注意⚠️**：在`Deploy`前务必取消`Dummy Mode`的勾选，否则编译出来的实验程序也会是Dummy Mode的实验程序，无法链接眼动仪。
{: .notice--warning}

---

# 3. 实验程序打包与迁移

> “无数老师和同学在向我求助的时候，都直接把程序打包成了普通的压缩包发给我。虽然绝大多数时候我都可以正常打开，但其实这是不规范的。”

Eyelink有自己的`.ebz`格式，全称“**E**xperiment **B**uilder **Z**ip”。这是一种压缩率特别高的压缩格式，以我们在前面几章编写的实验为例，整个实验文件夹的大小为401Kb，压缩后仅34Kb，压缩率高达90%。

除此之外，这种EB自带的压缩方式可以兼顾不同电脑软件环境和系统环境的变化。例如在Windows上面编写的实验直接放到MacOS上面运行就很容易报错，如果使用这种Package的方法就完全不需要担心兼容性的问题。

首先我们点击工具栏中的`Package`按钮。

![eb_show_package](/assets/images/eb_show_package.png)

在随后出现的窗口中输入保存的`.ebz`压缩包的文件名和储存路径，点击`OK`即可。

![eb_package_window](/assets/images/eb_package_window.png)

如图所示我压缩到了桌面：

<figure class="align-center">
  <img src="/assets/images/eb_show_packaged_ebz.png" alt="">
</figure> 


双击即可再次打开次压缩包，输入解压的路径和新的Project名称即可。默认在相同路径下生成相同名字的Project。

![eb_unpack_window](/assets/images/eb_unpack_window.png)

点击OK，解压完毕后将自动打开实验。

---

以上。