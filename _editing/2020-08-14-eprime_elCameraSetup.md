---
title: "elCameraSetup脚本(E-Prime眼动编程)"
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
  nav: E-Prime
---

---

~~~ vb
'Important! 'The Windows screen resolution should be Set To the same As your actual experiment resolution. 'If you need To present image files, please resize the images To match the experiment resolution.'1. In your E-Prime project, the experiment resolution can be configured through '"Edit -> Experiment->Devices->Display->Edit->Width and Height" Of the display device.'2. The screen resolution Of the monitor can be configured through '"Start -> Control Panel -> Display -> Settings" On Windows XP, 'Or "Start -> Control Panel -> Appearance and Personalization -> Display -> Adjust screen resolution" 'On Vista And Windows 7".doCameraSetup ' perform camera setup
~~~