---
title: "elCameraSetup脚本(E-Prime with DevKit v1.11.5)"
excerpt: "通过 E-Prime 执行相机校准。"
read_time: false
header:
  overlay_image: /assets/images/eprime_header.png
  overlay_filter: 0.5
  actions:
    - label: "下载示例文件(gsbv)"
      url: "https://pan.baidu.com/s/1dLvtJpazXb8DARSqknLyWg"
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

# 1. 讲解

这个脚本里面的内容其实很简单，实际的代码只有一行。

~~~ vb
'执行相机校准
doCameraSetup 
~~~

实例文件中，这行代码的前面还有许多注释内容，翻译如下：

> **重要！**
> 
> E-Prime中的显示器分辨率应当和实验的分辨率设置一致。如果您需要呈现图片文件，请将图片重新调整大小，与显示器分辨率尺寸一致。
> 
> 在您的 E-Prime 程序中，您可以在下面的地方更改显示器分辨率：
>  
> "Edit -> Experiment->Devices->Display->Edit->Width and Height"
> 
> 您可以通过下面的方法获取您显示器的实际分辨率：
> 
> "桌面 -> 右键 -> 显示设置 -> 分辨率"

---

# 2. 全部源代码

~~~ vb
'Important! 

'The Windows screen resolution should be Set To the same As your actual experiment resolution. 
'If you need To present image files, please resize the images To match the experiment resolution.

'1. In your E-Prime project, the experiment resolution can be configured through 
'"Edit -> Experiment->Devices->Display->Edit->Width and Height" Of the display device.

'2. The screen resolution Of the monitor can be configured through 
'"Start -> Control Panel -> Display -> Settings" On Windows XP, 
'Or "Start -> Control Panel -> Appearance and Personalization -> Display -> Adjust screen resolution" 
'On Vista And Windows 7".

doCameraSetup ' perform camera setup
~~~

---

以上。