---
title: "elStopRecording脚本(E-Prime眼动编程)"
excerpt: "在 E-Prime 中命令主试机停止当前试次的数据记录，以及发送命令前后不多的收尾工作。"
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
  nav: EL_3rd
---

---

# 1. 代码讲解

## 1.1 计算呈现延迟

刺激呈现延迟是一个很容易被忽视的问题。有时候它是个问题，而有时他又不是。

提到显示延迟，我们就要从显示器的工作模式说起。想必大家都听说过“显示器刷新率”这个名次。它是指显示器每秒刷新呈现内容的次数。以一台刷新率为 60Hz 的显示器为例，其显示的内容每 1/60秒，约等于 16.66ms 刷新一次。

在刚刚完成一次显示内容刷新后的 16.66ms 内，即便我们命令显示器显示新的刺激内容，显示器也无法显示出来。假设试次数足够多的条件下，因为刷新率有限所导致的延迟将无限接近于显示器刷新间隔的一半。然而这只是刺激呈现延迟的其中一个组成成分。

在 E-Prime 中，在刺激呈现命令发送之前，还要经过刺激材料的读取和整屏刺激内容的生成等过程，这就导致了延迟可能在 20~30ms 的水平。

**为什么这是个问题**

在视觉搜索及类似的任务中，有一个常用且非常显著的指标叫做“眼跳潜伏期”。

> 眼跳潜伏期 = 首个眼跳开始的时间 - 刺激呈现的时间

由于刺激呈现延迟的存在，刺激呈现的时间会偏早，且这个偏移量是随机的。眼跳潜伏期的数值一般在 200ms 以内，20~30ms 的随机误差对于眼跳潜伏期的计算是很严重的。

**怎么又不是问题了呢？**

从上面眼跳潜伏期的例子中可以知道，刺激呈现延迟对于数据的影响仅仅局限在需要刺激呈现时间参与计算的指标中。常用的指标中，需要考虑刺激呈现延迟的并不多。

另一方面，一些需要刺激呈现时间参与计算的指标的数值量级远超 20~30ms 的水平。例如反应时等时长在秒级的指标，可以忽略呈现延迟对其的影响。

下面的代码将帮您在 E-Prime 中计算刺激真正呈现在屏幕上的时间，并将刺激呈现的 Message 按照真正呈现在屏幕上的时间进行标记。
 
~~~ vb
'下面的代码用于读取当前的被试机时钟时间
'这可以用于记录刺激内容在屏幕上消失的时候，相较于刺激开始呈现的时候差了多久，因此
'我们可以计算出来刺激内容真正呈现在屏幕上的时间
'向.edf文件中写入一个"pictureTrial_Onset"的message，并附上计算出来的偏移时间，来标记刺激真正开始呈现的时间。
'
'如果您需要自行读取刺激开始呈现的时间，请务必确保减去了刺激呈现的偏移时长。例如：
'MSG	5254942 pictureTrial_Start
'MSG	5264986 10019 pictureTrial_Onset
'MSG	5264986 pictureStart pictureOnset 21495  21520
'
'在上面的例子中，”pictureTrial_Onset” 这条 Message 的真实时间应当是 5254967 (=5264986 - 10019)
'数据文件中，”pictureTrial_Start” 和 “pictureTrial_Onset” 这两条 Message 的时间差是 25 (= 5254967 - 5254942),
'这与 E-Prime 中报告的时间延迟是一致的 (21520 - 21495)
`
`下面的代码中
`pictureStart 表示刺激呈现命令的发出，在 startRecording 脚本的末尾，以 “pictureTrial_Start” 的内容发送到了数据中
`pictureTrial_Onset 表示刺激呈现在了显示器上。

Dim timeEnd As Long             `定义一个名称为 timeEnd 的 Long 长整型变量
timeEnd = elutil.currentTime()  `将当前时间赋值给 timeEnd 记录刺激内容消失的时间
Dim offset As Long              `定义一个名称为 offset 的 Long 长整型变量
offset = (timeEnd - timeStart) - (pictureTrial.OnsetTime - pictureTrial.StartTime)          `计算刺激内容究竟在显示器上呈现了多长时间
tracker.sendMessage  offset & " pictureTrial_Onset"     `发送延迟Message，记录实际的Onset时间
tracker.sendMessage "pictureStart pictureOnset " & pictureTrial.StartTime & "  " & pictureTrial.OnsetTime       `报告E-Prime中所记录的延迟时间（没啥用）
~~~ 

如果有多个刺激呈现的屏幕，则需要多组计算显示延迟的代码围绕在每一个刺激呈现屏的前后，这个部分在后续的内容中会讲解。

根据需要您可以决定是否计算 Onset 时间，不需要直接删除此段代码即可。
{: .notice--info}

## 1.2 停止记录

~~~ vb
'多记录 100ms 的数据来等待被试机清空显示内容
elutil.pumpDelay 100
tracker.stopRecording   `停止记录眼动数据
~~~ 

复制粘贴即可。
{: .notice--info}

## 1.3 绘制兴趣区

~~~ vb 
'下面的代码是为了和 Eyelink DataViewer 衔接。
'详细内容你可以查看 EyeLink Data Viewer User Manual 中的 "Protocol for EyeLink Data to Viewer Integration" 部分
'下面所展示的 IAREA message 在屏幕上制定的区域内绘制了一个矩形的兴趣区，并指定其边界 
'每个试次可以有多个兴趣区 
'在下面的例子中，我们在屏幕中心绘制了一个 100 * 100 像素的兴趣区
'其中 1 为兴趣区ID，后面四个数字分别是兴趣区左上右下边界的像素值，最后一个变量为兴趣区的Label，此处直接引用了 TrialList 中的变量
tracker.sendMessage "!V IAREA RECTANGLE 1 " & Display.XRes/2 -50 & " " & Display.YRes/2 - 50 & " " & Display.XRes/2 + 50 & " " & Display.YRes/2 + 50 & " " &  TrialList.GetCurrentAttrib("imageName")
~~~

根据需要决定是否需要绘制兴趣区，此处不画也可以在DV中进行数据处理的时候再画。
{: .notice--info}

## 1.4 记录试次特征变量

首先我们要明确什么叫做特征变量。

简单来说，我们在进行实验设计的时候，会计划好每个试次呈现哪些刺激内容，并创建一个表格来记录。这个表格在 ExperimentBuilder 中叫做 Datasource，在 E-Prime 中叫做 List。

TRIAL_VAR 命令

~~~ vb
'!V TRIAL_VAR 命令可以对每个试次进行一些特征的标记。
'每条 Message 包含一个特征值的名称和对应的值
tracker.sendMessage "!V TRIAL_VAR trial " & TrialList.GetCurrentAttrib("trialid")
tracker.sendMessage "!V TRIAL_VAR picture " & TrialList.GetCurrentAttrib("imageName") 
~~~

这段代码的目的是将 E-Prime 中的 List 完全复制到眼动数据中，List 中有多少列，则发送多少个“!V TRIAL_VAR”的 Message ，引用List中的内容，将其发送到 .edf 文件中。
{: .notice—info}

## 1.5 结束试次

~~~ vb
'TRIAL_RESULT 这条 Message 是为了告诉 DataViewer 试次结束了
'这条 Message 与 stopRecording 的命令不同，stopRecording是为了命令眼动仪停止记录眼动，而这条 Message 则是为了告诉 DataViewer 试次结束
'DataViewer 不会处理这条 Message 后面的任何数据
tracker.sendMessage "TRIAL_RESULT 1" 
~~~

这条是一定要有的，复制粘贴即可。
{: .notice—info}

---

# 2. 源代码

~~~ vb
'The following is used to read the current display time. 
'This can be used to determine the offset time relative to the start of the screen and thus
'determine the exact time when the display is shown.
'Write out a "pictureTrial_Onset" message with the determined offset value to mark the actual onset of the picture screen.
'
'When reading the EDF time of the display onset, make sure that you subtract the offset value. For example,
'MSG	5254942 pictureTrial_Start
'MSG	5264986 10019 pictureTrial_Onset
'MSG	5264986 pictureStart pictureOnset 21495  21520
'
'In the above example, actual time stamp for the pictureTrial_Onset message should be 5254967 (=5264986 - 10019)
'The EDF file time difference between the pictureTrial_Start and pictureTrial_Onset message is 25 (= 5254967 - 5254942),
'which is consistent with the time difference reported by E-Prime (21520 - 21495)

Dim timeEnd As Long
timeEnd = elutil.currentTime()
Dim offset As Long
offset = (timeEnd - timeStart) - (pictureTrial.OnsetTime - pictureTrial.StartTime)
tracker.sendMessage  offset & " pictureTrial_Onset"
tracker.sendMessage "pictureStart pictureOnset " & pictureTrial.StartTime & "  " & pictureTrial.OnsetTime

'Allow Windows to clean up while we record additional 100 msec of data
elutil.pumpDelay 100
tracker.stopRecording
 
'The following code is for the EyeLink Data Viewer integration purpose.   
'See section "Protocol for EyeLink Data to Viewer Integration" of the EyeLink Data Viewer User Manual

'The IAREA message specifies the attributes of a rectangular interest area for the trial. 
'Each trial can have a set of such rectangular interest areas. 
'The example following draws a 100 * 100 pixels interest area in the center of the screen
tracker.sendMessage "!V IAREA RECTANGLE 1 " & Display.XRes/2 -50 & " " & Display.YRes/2 - 50 & " " & Display.XRes/2 + 50 & " " & Display.YRes/2 + 50 & " " &  TrialList.GetCurrentAttrib("imageName")

'This TRIAL_VAR command specifies a trial variable and value for the given trial. 
'Send one message for each pair of trial condition variable and its corresponding value.
tracker.sendMessage "!V TRIAL_VAR trial " & TrialList.GetCurrentAttrib("trialid")
tracker.sendMessage "!V TRIAL_VAR picture " & TrialList.GetCurrentAttrib("imageName") 

'The TRIAL_RESULT message defines the end of a trial for the EyeLink Data Viewer. 
'This is different than the end of recording message END that is logged when the trial recording ends. 
'Data viewer will not parse any messages, events, or samples that exist in the data file after this message. 
tracker.sendMessage "TRIAL_RESULT 1" 
~~~

---

以上。