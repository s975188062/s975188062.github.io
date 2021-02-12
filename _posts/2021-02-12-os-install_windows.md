---
title: "万字长文，手把手教你如何获取一个纯净的 Window 系统（实际上只有不到 5000 字）"
excerpt: "新买的电脑 ≠ 系统干净。所有的整机厂商都会在操作系统中增加或者阉割功能来节省成本或者创造额外收入。使用过程中的许多问题也都来源于这里。网上也有很多教程教大家如何解决各种各样的阉割系统问题，但是给电脑重做一个纯净系统才是最直接的解决办法。"
read_time: false
header:
  overlay_image: /assets/images/os-Install_windows_header.png
  overlay_filter: 0.5
  teaser: /assets/images/os-Install_windows_taster.png
categories:
  - OS
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



注意！Charlie 推荐将数据和文件被分到可移动硬盘或者 U 盘。这样最省心！

{: .notice--info}



> **数据**就是**金钱**！
>
> **数据**就是**生命**！
>
> **数据**就是**一切**！

数据是你再用相同的时间也不一定换得回来的宝贵资源，因此在重做系统之前一定一定要备份数据。

需要备份的内容除了我们自己保存的各类数据和文件之外，一定要记得检查“文档”这个文件夹中的内容。

几乎所有的软件都会把用户数据储存在这个文件夹中，包括且不仅限于我们日常使用的 QQ、微信和各类游戏等软件。在我们使用电脑的过程中，这些用户文件是持续产生且储存在 C 盘的这个目录下的，这也是我们 C 盘越用越满，电脑越用越卡的原因。

打开`我的电脑`即可以看到这个文件夹，如下所示：

![os-install_windows-show_where_is_documents_folder](/assets/images/os-install_windows-show_where_is_documents_folder.png)

备份数据有两种模式：只格式化系统盘 或者 格式化整个电脑。容我进行一下简单介绍:

* 电脑上除了 C 盘还至少有其他盘（这样操作其实很不省心）

> 如果你的电脑上电脑上除了 C 盘还有其他盘，那么你可以把数据备份到除了 C 盘以外的其他硬盘上，如到 D 盘。这里面有一个小问题，就是是否要保留原本留在D盘里面的其他数据。
>
> 其中的文件无非是分为两类，数据和软件。
>
> 数据、视频、图片、文档等文件是否需要保留大家根据需要决定就可以。但是软件部分 Charlie 强烈建议不要保留。
>
> 一方面由于重新做了操作系统之后，注册表和配置文件等关键信息可能会丢失，另一方面软件这个东西如果不是正常安装的话，鬼知道会出现什么 BUG。
>
> 这种情况下可以只格式化 C 盘，不对其他盘进行操作。

* 电脑只有一个 C 盘（省心）

> 如果电脑上只有这一个盘的话，就必须使用其他的 U 盘或者其他移动硬盘来备份数据了，这部分不再赘述。

## 1.2 下载操作系统

随便百度一下“Windows 10 下载”就会有很多第三方的网站结果出来，依照 Charlie 的经验，这些网站上面挂的操作系统镜像都是“豪华版”。

这种“豪华版”的镜像通常都绑定了许多的流氓软件，甚至有病毒木马或者后门。

> 官网是下载软件的最佳路径。

微软一直以来都在自己的网站上挂在了纯净系统的[下载连接](https://www.microsoft.com/zh-cn/software-download/windows10)（无法跳转请访问：https://www.microsoft.com/zh-cn/software-download/windows10）。

![os-Install_windows-download_windows](/assets/images/os-Install_windows-download_windows.png)

此处我们所下载的并不是完整的 Windows 10 操作系统，而是操作系统的下载器`MediaCreationTool20H2`。（根据下载的 Windows 版本不同，最后的4位版本号可能会有出入）

## 1.3 安装到U盘

此处 Charlie 并不准备使用 PE 去安装系统，因为某些 PE 也会在未经允许的情况下安装流氓软件。因此我们使用微软官方的安装工具制作系统安装盘。

首先，准备一个不小于 8G 的 U 盘，连接到电脑上，确认 U 盘可以被电脑识别到。

> 别担心，装完系统之后 U 盘还可以继续使用

{: .notice--warning}制作系统安装盘的操作会格式化 U 盘，请注意备份重要文件。



打开`MediaCreationTool`，接受许可协议。

![os-install_windows-win_media_creation_tool_accept_license](/assets/images/os-install_windows-win_media_creation_tool_accept_license.png)

选择“为另一台电脑创建安装介质（U 盘、DVD 或 ISO 文件）”。

![win_media_creation_tool_install_to_flash_drive](/assets/images/os-install_windows-win_media_creation_tool_install_to_flash_drive.png)

默认，下一步。

![win_media_creation_tool_config_installation_settings](/assets\images\os-install_windows-win_media_creation_tool_config_installation_settings.png)

选择“U 盘”。

![win_media_creation_tool_install_to_flash_drive_2](/assets/images/os-install_windows-win_media_creation_tool_install_to_flash_drive_2.png)

选择你的 U 盘。如果电脑上连接了多个U盘无法确认的话，可以移除其他的 U 盘然后点击“刷新驱动器列表”。

![win_media_creation_tool_choose_flash_drive](..\assets\images\os-install_windows-win_media_creation_tool_choose_flash_drive.png)

随后，`MediaCreationTool`会自动将你的 U 盘只作为系统安装盘。此时请耐心等待，根据实际的网络情况，等待时间会从 20分钟 - 2小时 不等。

如果你看到了如下界面，那么你的装机盘已经制作完毕了，开始装系统吧！

![os-install_windows-win_media_creation_tool_done](/assets\images\os-install_windows-win_media_creation_tool_done.png)

---

# 2. 安装系统

## 2.1 安装

将 U 盘插到电脑上，重启电脑。顺利的话，电脑将自动重启进入如下界面：

![os-install_windows-install_step_1_select_language](/assets/images/os-install_windows-install_step_1_select_language.png)

> 如果没能自动进入到这个界面的话，重新启动电脑。在电脑启动的过程中注意屏幕的提示。
>
> 如 Charlie 手边的是一台 Intel NUC5i7RYH。在启动时会显示如下界面：
>
> ![os-install_windows-install_step_x_start](/assets/images/os-install_windows-install_step_x_start.png)
>
> 其中最后一行提示`F10 to Enter Boot Menu`，所以应该在此时尽快按`F10`启动 BootManager。
>
> 因为电脑的主板型号不同，具体按键也不尽相同，基本的逻辑就是找一找看通过什么按键可以进入到`Boot Menu`或者`Boot Manager`。常见的按键包括但不仅限于`F12`、`F1`、`F2`和`F10`，按照概率排序。
>
> 随后在 Boot Manager 中选择以“USB”开头的选项：
>
> ![os-install_windows-install_step_x_boot_manager](/assets/images/os-install_windows-install_step_x_boot_manager.png)
>
> 按回车即可进入到开始介绍的界面。

一切默认选择，直接点击下一步：

![os-install_windows-install_step_1_select_language_2](/assets/images/os-install_windows-install_step_1_select_language_2.png)

现在安装：

![os-install_windows-install_step_2_install_now](/assets/images/os-install_windows-install_step_2_install_now.png)

选择“我没有产品密钥”：

![os-install_windows-install_step_3_no_license](/assets/images/os-install_windows-install_step_3_no_license.png)

选择“Windows 10 专业版”，点击“确定”：

![os-install_windows-install_step_4_stlect_pro_version](/assets/images/os-install_windows-install_step_4_stlect_pro_version.png)

接受许可条款，点击“下一步”：

![os-install_windows-install_step_5_accept_license](/assets/images/os-install_windows-install_step_5_accept_license.png)

选择“自定义：仅安装 Windows (高级) (C)”：

![os-install_windows-install_step_6_install_only](/assets/images/os-install_windows-install_step_6_install_only.png)

选择要安装的驱动器，这一步比较复杂。

![os-install_windows-install_step_7_which_disk](/assets/images/os-install_windows-install_step_7_which_disk.png)

首先，我们需要选择正确的驱动器分区，即我们平时所说的 `C 盘`、`D 盘`、`E 盘`。

> 蓝色：代表物理硬盘，指电脑中实际安装了几个硬盘。
>
> 绿色：代表硬盘分区，除了`C 盘`、`D 盘`、`E 盘`外还会有系统引导分区和恢复分区等。
>
> 紫色：分区盘的容量。

对于初学者来说貌似很难去判断应该选择哪一个，那么我们使用一个简单一点的方法。

* 电脑上除了 C 盘还至少有其他盘

> 注意上图中紫色标记的数值，根据硬盘容量找到哪个是你的 C 盘分区。只删除这一个分区。

* 电脑只有一个 C 盘

> 重复下图的步骤，依次删除全部的分区。

注意！如果您将数据备份到了除了 C 盘的其他硬盘上，一定不要删除错误，否则全部数据都将丢失。

{: .notice--warning}

![os-install_windows-install_step_7_select_disk](/assets/images/os-install_windows-install_step_7_delete_disk.png)

随后在列表中选中“驱动器 x 未分配的空间”，点击下一步（列表中也可能有其他分区）：

![os-install_windows-install_step_7_select_disk](/assets/images/os-install_windows-install_step_7_select_disk.png)

随后，系统将会自动安装，安装完毕后系统会自动重启：

![os-install_windows-install_step_8_installing](/assets/images/os-install_windows-install_step_8_installing.png)

电脑重启后，拔下 U 盘即可。重启的过程实际上是第一次启动系统，过程会有些漫长，我们耐心等待。

## 2.2 初始化

重启后我们就可以开始初始化我们的新操作系统了。

选择“中国”，点击“是”：

![os-install_windows-install_init_1_language](/assets/images/os-install_windows-install_init_1_language.png)

选择“微软拼音”，点击“是”：

![os-install_windows-install_init_2_keyboard](/assets/images/os-install_windows-install_init_2_keyboard.png)

点击“跳过”：

![os-install_windows-install_init_3_skip_second_keyboard](/assets/images/os-install_windows-install_init_3_skip_second_keyboard.png)

**一定不要联网！！！**点击“我没有 Internet 连接”：

![os-install_windows-install_init_4_network](/assets/images/os-install_windows-install_init_4_network.png)

点击“继续执行有限设置”：

![os-install_windows-install_init_5_limited_setting](/assets/images/os-install_windows-install_init_5_limited_setting.png)

设定用户名，尽量不要使用中文，并且使用下划线代替空格，如“Changlin_Song”：

![os-install_windows-install_init_6_user_name](/assets/images/os-install_windows-install_init_6_user_name.png)

设置密码：

![os-install_windows-install_init_7_password](/assets/images/os-install_windows-install_init_7_password.png)

隐私设置，点击“接受”即可：

![os-install_windows-install_init_8_private_setting](/assets/images/os-install_windows-install_init_8_private_setting.png)

设置语音助手 Cotana ，点击“以后再说”：

![os-install_windows-install_init_9_cortana](/assets/images/os-install_windows-install_init_9_cortana.png)

随后系统会自动完成一些配置，显示界面如下：

![os-install_windows-install_init_10_need_minutes](/assets/images/os-install_windows-install_init_10_need_minutes.png)

几分钟后，就进入了新的系统，至此完成了一半。

![os-install_windows-install_init_done](/assets/images/os-install_windows-install_init_done.png)

---

# 3. 补全驱动和运行环境

Charlie 在前面已经提到过，“纯净”也代表着“要啥啥没有”，所以我们需要自力更生做一些事情。

Charlie 在工作中积攒了一套系统环境补全工具，分享给大家。其中包含如下几个部分：

- KMS 系统激活工具 [【下载连接(8fn0)】](https://pan.baidu.com/s/16Vof2bRM6dLz6vTXloxojw)  [【原项目地址】](https://github.com/zbezj)
- DrictX 修复工具 [【下载连接(5tdt)】](https://pan.baidu.com/s/1A4Wtl6DXf6m8WFT78oOBGQ)  [【原项目地址】](https://blog.csdn.net/vbcom/article/details/6962388)

在下面的使用中将进行详细介绍。

## 3.1 激活系统

> 激活系统的正确姿势一定是到微软指定代理商处购买~~死贵的~~系统激活码。
>
> “官方正版”和“国产正版”之间其实在使用上并没有区别。两者之间的唯一不同就是是否是微软合法授权的。大家可以放心食用。

当然 Charlie 也有让你使用”国产正版“的方法。

首先我们来看一下你的系统是否需要激活。有一些整机厂商会对自己的产品进行硬件授权，即无论如何重新做系统，都不需要额外激活，Windows 系统会自动识别并激活。

但是大多数电脑都是家庭版系统，安装专业版的话由于硬件授权版本不符，还是需要额外激活。你可以在“开始菜单 - 控制面板 - 更新和安全 - 激活”中查看。这不重要。

大多数时候都是需要额外激活的，在这个过程中不要联网。

### 3.1.1 关闭 Windows Defender 

打开“开始菜单 - Windows 安全中心 - 病毒和威胁防护 - 管理设置”。

![os-install_windows-windows_defender](/assets/images/os-install_windows-windows_defender.png)

依次关闭“实时保护”、“云提供的保护”、“自动提交样本”和“篡改防护”。

![os-install_windows-disable_windows_defender](/assets/images/os-install_windows-disable_windows_defender.png)

### 3.1.2 使用 KMS Activator 激活 Windows 

这里我们要使用的是[HEU KMS Activator](https://blog.csdn.net/vbcom/article/details/6962388)

右键`HEU_KMS_Activator_v22.1.0`，点击“以管理员身份运行”。

> 如果没有任何反应或者没能成功打开，那么应该是没有完全关闭 Windows Defender ，请重新执行 [3.1.1 关闭 Windows Defender](/操作系统/os-install_windows/#311-关闭-windows-defender) 中的步骤。

点击软件中间的 Windows Logo 即可自动激活 Windows。过程中您可以看到软件窗口下部有一个显示进度的百分比提示。

当激活完成时，软件将自动提示：

![os-install_windows-KMS_activate_success](/assets/images/os-install_windows-KMS_activate_success.png)

如果您看到了如上的窗口，那么您的 Windows 系统已经激活成功。

那么您可以反向执行 [3.1.1 关闭 Windows Defender](/操作系统/os-install_windows/#311-关闭-windows-defender) 中的步骤，重新打开 Windows Defender 。

## 3.2 更新系统

首先，连接网络。

打开“开始菜单 - 控制面板 - 更新和安全 - Windows 更新”。

一般情况下，系统会自动开始检查更新。如果没能正常更新的话，点击“检查更新”的按钮手动开始检查更新。

![os-install_windows-update_windows](/assets/images/os-install_windows-update_windows.png)

随后，系统会自动更新，如下图所示：

![os-install_windows-update_windows_working](/assets/images/os-install_windows-update_windows_working.png)

点击“查看可选更新”：

![os-install_windows-update_windows_check_for_more](/assets/images/os-install_windows-update_windows_check_for_more.png)

选中所有可选条目，点击“下载并安装”：

![os-install_windows-update_all](/assets/images/os-install_windows-update_all.png)

随后，等待所有更新内容下载并安装完成。这个过程的时间取决于你的网络情况，泡杯茶吧 :P

## 3.3 安装驱动

如果你的电脑中有一些特殊硬件，包括且不仅限于：独立显卡、独立声卡、TTL并口卡等硬件设备，那么你需要为他们安装额外的驱动程序。

“驱动精灵”不失为一个不错的选择，但是请注意在安装过程中不要直接默认的“下一步”到底，仔细检查下有哪些默认勾选的对勾是在绑定流氓软件。

但是如果你的电脑中并没有安装上述硬件的话，恭喜你！可以跳过这一步。

## 3.4 安装运行环境

此处我们使用的工具是`DrictX Repair`。

右键，点击“以管理员身份运行”打开`DrictX Repair`。

![os-install_windows-achive_DrctXR](/assets/images/os-install_windows-achive_DrctXR.png)

点击“检测并修复”开始运行。

![os-install_windows-repair_DrctX](/assets/images/os-install_windows-repair_DrctX.png)

随后软件会自动进行工作。

![os-install_windows-repairing](/assets/images/os-install_windows-repairing.png)

完成后点击“关闭”并退出即可。

至此完成系统安装，之后就请安装自己需要的各种软件吧~

---

# 4. 说点其他

## 4.1 格式化 U 盘

刚刚我们安装系统用过的 U盘 现在已经可以格式化并用作正常 U盘 使用了。

![os-install_windows-rebuild_flash](/assets/images/os-install_windows-rebuild_flash.png)

## 4.2 如何正确下载软件

任何软件，都到**官网**去下载！

一方面官网的资源是纯净无担忧的；

另一方面，每个软件的官网都会有很多 Q&A 来介绍这个软件如何正确安装以及常见的安装问题。

---

以上。
