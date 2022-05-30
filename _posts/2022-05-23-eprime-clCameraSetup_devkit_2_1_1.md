---
title: "elCameraSetup脚本(E-Prime3 with DevKit v2.1.1)"
excerpt: "通过 E-Prime 执行相机校准。"
read_time: false
header:
  overlay_image: /assets/images/eprime_header.png
  overlay_filter: 0.5
  actions:
    - label: "下载示例文件(提取码：vo0m)"
      url: "https://pan.baidu.com/s/1A8QKilCfR1CNSBa_xsqKeA"
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

# 1. 脚本讲解

 > 这个脚本的内容很简单，就是执行眼动仪的校准。根据需要复制此脚本在需要校准的地方即可。

~~~ vb
' 进入眼动仪校准环节
' c - 校准
' v - 验证
' o - 继续实验
doCameraSetup	

setMouseState False	' 隐藏鼠标
~~~

---

# 2. 源代码

~~~ vb
doCameraSetup	'perform camera setup (C to calibrate, V to validate, O to
				'continue (output/record).
setMouseState False	'ensure mouse cursor is not visible before proceeding.
~~~

---

以上。