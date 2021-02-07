---
title: "电脑不卡，从一个纯净的 Window 系统开始"
excerpt: "新买的电脑 ≠ 系统干净。所有的整机厂商都会在操作系统中增加或者阉割功能来节省成本或者创造额外收入。使用过程中的许多问题也都来源于这里。网上也有很多教程教大家如何解决各种各样的阉割系统问题，但是给电脑重做一个纯净系统才是最直接的解决办法。"
read_time: false
header:
  overlay_image: /assets/images/os-Install_windows_header.png
  overlay_filter: 0.5
  teaser: /assets/images/os-Install_windows_taster.png
categories:
  - 操作系统
tags:
  - 系统安装
  - Windows
  - 系统维护
toc: true
comments: true
author_profile: true
---
# 0. 纯净系统的利与弊

凡事皆有利弊，在决定是否要安装一个纯净的操作系统之前，我们先来看一下纯净的系统有哪些优势和劣势。

固然，经过加工处理过的系统会绑定许多流氓软件甚至阉割掉电脑厂商认为我们所不需要的功能。但是不得不承认这种非纯净的系统在易用性上远远胜出。

纯净版的系统的优劣都在“纯净”这两个字上。

因为纯净，所以没有流氓软件的骚扰与拖累。

也是因为纯净，可能会缺少软件所依靠的运行环境。

这就要求我们在安装好纯净的操作系统之后，必须首先更新操作系统、安装驱动以及安装许多语言（这里指编程语言）的运行环境和编译器。

如果你**对折腾饱含热情**或者**对自己的计算机水平略有信心**的话，自己动手重做系统将是一个非常不错的选择。

但也请注意，如果您对电脑不甚了解的话，请务必求助专业人士来帮你完成下面的工作。~~当然 Charlie 无法保证“专业人士”会给你一个什么样的结果……~~

---

# 1. 准备工作

## 1.1 备份文件

> **数据**就是**金钱**！
>
> **数据**就是**生命**！
>
> **数据**就是**一切**！

数据是你再用相同的时间也不一定换得回来的宝贵资源，因此在重做系统之前一定一定要备份数据。

需要备份的内容除了我们自己保存的各类数据和文件之外，一定要记得检查“文档”这个文件夹中的内容。

几乎所有的软件都会把用户数据储存在这个文件夹中，包括且不仅限于我们日常使用的 QQ、微信和各类游戏等软件。在我们使用电脑的过程中，这些用户文件是持续产生且储存在 C 盘的这个目录下的，这也是我们 C 盘越用越满，电脑越用越卡的原因。

打开`我的电脑`即可以看到这个文件夹，如下所示：

![os-install_windows-show_where_is_documents_folder](../assets/images/os-install_windows-show_where_is_documents_folder.png)

备份数据有两种模式：只格式化系统盘和格式化整个电脑。容我进行一下简单介绍:

* 电脑上除了 C 盘还至少有其他盘

> 如果你的电脑上电脑上除了 C 盘还有其他盘，那么你可以把数据备份到除了 C 盘以外的其他硬盘上，如到 D 盘。这里面有一个小问题，就是是否要保留原本留在D盘里面的其他数据。
>
> 其中的文件无非是分为两类，数据和软件。
>
> 数据、视频、图片、文档等文件是否需要保留大家根据需要决定就可以。但是软件部分 Charlie 强烈建议不要保留。
>
> 一方面由于重新做了操作系统之后，注册表和配置文件等关键信息可能会丢失，另一方面软件这个东西如果不是正常安装的话，鬼知道会出现什么 BUG。
>
> 这种情况下可以只格式化 C 盘，不对其他盘进行操作。

电脑只有一个 C 盘

> 如果电脑上只有这一个盘的话，就必须使用其他的 U 盘或者其他移动硬盘来备份数据了，这部分不再赘述。

## 1.2 下载操作系统

随便百度一下“Windows 10 下载”就会有很多第三方的网站结果出来，依照 Charlie 的经验，这些网站上面挂的操作系统镜像都是“豪华版”。

这种“豪华版”的镜像通常都绑定了许多的流氓软件，甚至有病毒木马或者后门。

> 官网是下载软件的最佳路径。

微软一直以来都在自己的网站上挂在了纯净系统的[下载连接](https://www.microsoft.com/zh-cn/software-download/windows10)（无法跳转请访问：https://www.microsoft.com/zh-cn/software-download/windows10）。

![os-Install_windows-download_windows](../assets/images/os-Install_windows-download_windows.png)

此处我们所下载的并不是完整的 Windows 10 操作系统，而是操作系统的下载器`MediaCreationTool20H2`。（根据下载的 Windows 版本不同，最后的4位版本号可能会有出入）

## 1.3 安装到U盘

此处 Charlie 并不准备使用 PE 去安装系统，因为某些 PE 也会在未经允许的情况下安装流氓软件。因此我们使用微软官方的安装工具制作系统安装盘。

首先，准备一个不小于 8G 的 U 盘，连接到电脑上，确认 U 盘可以被电脑识别到。

> 别担心，装完系统之后 U 盘还可以继续使用

制作系统安装盘的操作会格式化 U 盘，请注意备份重要文件。

{: .notice-warning}

打开`MediaCreationTool`，接受许可协议。

![os-install_windows-win_media_creation_tool_accept_license](../assets/images/os-install_windows-win_media_creation_tool_accept_license.png)

选择“为另一台电脑创建安装介质（U 盘、DVD 或 ISO 文件）”。

![win_media_creation_tool_install_to_flash_drive](../assets/images/os-install_windows-win_media_creation_tool_install_to_flash_drive.png)

默认，下一步。

![win_media_creation_tool_config_installation_settings](../assets\images\os-install_windows-win_media_creation_tool_config_installation_settings.png)

选择“U 盘”。

![win_media_creation_tool_install_to_flash_drive_2](../assets/images/os-install_windows-win_media_creation_tool_install_to_flash_drive_2.png)

选择你的 U 盘。如果电脑上连接了多个U盘无法确认的话，可以移除其他的 U 盘然后点击“刷新驱动器列表”。

![win_media_creation_tool_choose_flash_drive](..\assets\images\os-install_windows-win_media_creation_tool_choose_flash_drive.png)

随后，`MediaCreationTool`会自动将你的 U 盘只作为系统安装盘。此时请耐心等待，根据实际的网络情况，等待时间会从 20分钟 - 2小时 不等。

如果你看到了如下界面，那么你的装机盘已经制作完毕了，开始装系统吧！

![os-install_windows-win_media_creation_tool_done](../assets\images\os-install_windows-win_media_creation_tool_done.png)

---

# 2. 安装系统

---

# 3. 补全驱动和运行环境

## 3.1 激活系统

## 3.2 更新系统

## 3.3 安装驱动

## 3.4 安装运行环境

---

# 4. 说点其他

## 4.1 格式化 U 盘

## 4.2 分盘

## 4.3 如何正确下载软件

---

以上。