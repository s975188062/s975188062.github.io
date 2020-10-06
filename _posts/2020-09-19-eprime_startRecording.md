---
title: "elStartRecording脚本(E-Prime眼动编程)"
excerpt: "在 E-Prime 中命令主试机开始记录眼动，以及发送命令前后的相当多的准备工作。"
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

> elStartRecording 和 elStopRecording 是最关键的两个脚本。本文中会涉及一些眼动实验编程的基础概念。

elStartRecording 脚本是难度最高的一个脚本，需要了解一些 E-Basic 的语法才能读懂。

# 1. 讲解

## 1.1 执行 DriftCorrect

第一部分的代码是在执行 DriftCorrect 。

> 关于 DriftCorrect 的功能和作用您可以查看[_>>>DriftCorrection可以帮我们做到哪些事情<<<_](/eyelink/Drift/)。

~~~ vb
'使用用户自定义的 Drift 界面进行 DriftCorrect

'上面两行代码根据需要自行取用。

~~~vb
doDriftCorrect Display.XRes/2,Display.YRes/2,False,True,Fixation
~~~

![eprime_start_recording_trialproc](/assets/images/eprime_start_recording_trialproc.png)

在使用自定义的 DriftCorrection 屏的时候，一定要将 elStartRecording 放在自定义的 DriftCorretcion 屏之后。

## 1.2 标示试次的开始

此处我们看到的是 `tracker.sendMessage` 这个函数的基本用法。在函数后面直接设置需要发送的 Message 文本内容。

“ & ” 符号在 E-Basic 语言中有链接字符串的功能。

`TrialList.GetCurrentAttrib("trialid")` 则直接引用了 TrialList 中 trialid 的值。

假设 TrialList 中，当前行 trialid 的值为 12 ，则发送的 Message 内容为 “TRIALID 12”。

## 1.3 配置主试机的 Record 页面

### 1.3.1 Record Status Message

> 此项功能在大多数情况下是不需要的。

~~~

提示内容将显示在下图所示的位置（红框标记）：

![eprime_elStartRecording_record_status_message](/assets/images/eprime_elStartRecording_record_status_message.png)

这部分其实可以删除。
{: .notice--info}

### 1.3.2 设置主试机监控区域的显示屏幕

此段代码主要用于设置主试机Record屏中，用于监控被试机显示内容的部分。

> 主试机和被试机是两台机器，是需要分别设置的。
> 
> 被试机已经通过E-Prime程序设置了刺激呈现序列，主试机显示的内容则需要我们通过代码来手动完成。

~~~ vb
' “Display.XRes/2 -50 & " " & Display.YRes/2 - 50 & " " & Display.XRes/2 + 50 & " " & Display.YRes/2”为左上右下边界

> 上述的 draw_box 代码是为了在屏幕上做一个标记。
> 
> 在有些情况下，我们希望监控一个试次内连续出现的多屏刺激（如学习再认范式）。每屏刺激中所呈现的刺激物的位置和大小会有所不同。受到某些限制，我们不能让主试机显示的内容随着被试机同步迭代，我们只能选择多屏中的一屏进行监控。
> 
> 为了能监控所有屏中被试有没有看刺激物，我们退而求其次地一次向在屏幕上把所有屏的刺激物的范围全部画出来，以此进行刺激物监控。

不需要 draw_box 的功能则可以直接删除这段代码。
{: .notice--info}

~~~ vb
{: .notice--info}

'如果某一个参数为 0 或 False ，则对应的数据操作则不会被执行 
~~~

代码最后定义的 timeStart 将在 elStopRecording 脚本中用于计算刺激呈现延迟，我们放到后面讲解。

前半部分复制粘贴即可，后面发送 Message 的部分修改一下 Message 的内容。
{: .notice--info}

---

# 2. 源代码

~~~ vb
'perform drift correct with custom target
~~~

---

以上。