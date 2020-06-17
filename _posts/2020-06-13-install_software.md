---
title: "Eyelink软件安装教程"
excerpt: "Windows和MacOS的Eyelink软件安装教程。"
read_time: false
categories:
  - Eyelink
tags:
  - Experiment Builder
  - Data Viewer
  - Development Kit
toc: true
comments: true
author_profile: false
sidebar:
  title: "目录"
  nav: host
---

---

# 1. 安装前准备

## 1.1 卸载杀毒软件

如果您使用的是Win10系统，那么我建议您卸载电脑中全部的杀毒软件。Win10系统自带的安全工具已经足够日常保护您电脑的安全。

杀毒软件包括且不仅限于：360系（包括安全卫士、杀毒和浏览器等一系列软件）、Mcafee、金山毒霸等。
{: .notice--warning}

## 1.2 下载安装包

* 您可以关注<Eyelink博润视动>公众号，在后台回复“EB”、“DV”和“DevKit”获取软件安装包；
* 也可以点击下方的链接下载：


| 软件                 | Windows下载链接                                                                                                                               | Mac下载链接                                                                                                                                 |
|--------------------|-------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------|
| Experiment Builder | [![win_2.2.299](https://img.shields.io/badge/Windows_v2.2.299-c10g-green?&style=social)](https://pan.baidu.com/s/1rzwMpOJeR9PK6D6Oo3n01g) | [![Mac_2.2.299](https://img.shields.io/badge/MacOS_v2.2.299-7cfa-green?&style=social)](https://pan.baidu.com/s/14IqXism-ktX_qae_gzSUBQ) |
| Data Viewer        | [![win_4.1.1](https://img.shields.io/badge/Windows_v4.1.1-wmzv-green?&style=social)](https://pan.baidu.com/s/1XVAxAzoMquc51H1V2VuxEQ)     | [![Mac_2.2.299](https://img.shields.io/badge/MacOS_v4.1.1-n17d-green?&style=social)](https://pan.baidu.com/s/1ruGbGM8ZJq5oZBjIkiPczw)   |
| Development Kit    | [![win_1.1.15](https://img.shields.io/badge/Windows_v1.1.15-kygv-green?&style=social)](https://pan.baidu.com/s/1jSqjRoSd9DKps9_9Aj9qpw)   | [![Mac_2.2.299](https://img.shields.io/badge/MacOS_v1.1.11-zied-green?&style=social)](https://pan.baidu.com/s/1LyIpUTbiKygrVL0mJjKBIQ)  |


---

# 2. Windows 安装教程

注意：三个软件需要一个一个安装，确定装完成再装下一个。同时安装极易引发bug。
{: .notice--info}

## 2.1 安装Experiment Builder

双击打开软件安装包，点击允许设备进行更改。

![host_install_software_1](/assets/images/host_install_software_1.png)

随后安装包会自动加载，耐心等待一会。

![host_install_software_2](/assets/images/host_install_software_2.png)

安装包自动加载完成后，会跳出新的窗口，在此处点击`Next >`。

![host_install_software_3](/assets/images/host_install_software_3.png)

后面的除了填写用户和组织名称以外，全部按默认设置安装。

![host_install_software_4](/assets/images/host_install_software_4.png)

![host_install_software_5](/assets/images/host_install_software_5.png)

![host_install_software_6](/assets/images/host_install_software_6.png)

![host_install_software_7](/assets/images/host_install_software_7.png)

![host_install_software_8](/assets/images/host_install_software_8.png)

随后如下图所示，进入安装阶段。

![host_install_software_9](/assets/images/host_install_software_9.png)

安装完成后，系统会提示是否信任SR Research Ltd安装的软件，点击`安装`。

![host_install_software_10](/assets/images/host_install_software_10.png)

至此完成了Experiment Builder的安装。桌面上不会生成软件的快捷方式，但是您可以到“开始 - SR Research”中找到Experiment Builder。

## 2.2 安装Data Viewer

Data Viewer的安装和Experiment Builder基本相同，唯一有区别的地方是您需要选择安装32位或64位版本。

您可以“右键`我的电脑` - 属性”中查看您的操作系统版本，来选择安装对应的Data Viewer版本。

![host_install_software_64_base_os](/assets/images/host_install_software_64_base_os.png)

例如，我的电脑是64位操作系统，则在安装的过程中选择64位版本，其余设置和EB相同。

![host_install_software_11](/assets/images/host_install_software_11.png)

## 2.3 安装Development Kit

Development Kit的安装过程同Experiment Builder完全相同，此处省略。

## 2.4 安装加密狗驱动

打开“开始菜单 - SR Research - Install HASP Driver”。

随后全部按照默认设置安装即可。

---

# 3. Mac OS X

注意：Mac OS X不是一个做实验的好系统，因此不建议使用MacOS运行实验程序。
{: .notice--info}

## 3.1 安装Experiment Builder

由于Mac系统的问题，MacOS10.13及以上的系统暂不支持EB的Test Run功能。
{: .notice--info}

打开Experiment Builder安装包，如果出现<用户协议>窗口，则点击`Agree`。随后跳出安装窗口。按照指示将“程序文件”夹拖拽至“Application文件夹”即可。

![host_install_software_12](/assets/images/host_install_software_12.png)

> 注意不要着急关闭EB的安装窗口，后续需要继续安装加密狗驱动。

如果您的Mac中没有安装Java环境或者Xcode，则需要额外手动安装。

[<Java SE 6>下载链接](https://support.apple.com/kb/DL1572?viewlocale=zh_CN&locale=zh_CN)

下载完成后按照默认设置安装即可。

## 3.2 安装加密狗驱动

双击“Hasp”文件夹。

![host_install_software_13](/assets/images/host_install_software_13.png)

双击“Install Sentinel Runtime Environment”文件夹，点击继续。后续全部按照默认设置安装即可。

![host_install_software_14](/assets/images/host_install_software_14.png)

## 3.3 安装Data Viewer

打开Data Viewer安装包，如果出现<用户协议>窗口，则点击`Agree`。随后跳出安装窗口。按照指示将“程序文件”夹拖拽至“Application文件夹”即可。

![host_install_software_15](/assets/images/host_install_software_15.png)

## 3.4 安装Development Kit

双击打开安装包，默认设置安装即可。

![host_install_software_16](/assets/images/host_install_software_16.png)

---

以上。