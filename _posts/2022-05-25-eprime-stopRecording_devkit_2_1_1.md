---
title: "elStopRecording脚本(E-Prime with DevKit v2.1.1)"
excerpt: "在 E-Prime 中命令主试机停止当前试次的数据记录，以及发送命令前后不多的收尾工作。"
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

Stop Recording 脚本是所有几个脚本中难度最大的一个，需要对眼动实验有一定程度的理解才能完成修改。

眼动数据中最关键的 Interest Period 和 Interest Area 概念都会在这个脚本中有所涉猎。

## 1.1 在眼动数据中标记 Message 事件

> 认真阅读，尝试理解。根据自己的实验内容修改！

所谓 Message 事件，就是一种事件标记，类似于脑电中的 Marker 。

这里需要明确一件事情，在 E-Prime 中运行眼动程序，与其说是 E-Prime 控制眼动仪采集，不如说是 E-Prime 和眼动仪各干各的，在需要的时候互相沟通下而已。

所以如果在一个 Trial 内有多个刺激屏的时候，E-Prime 按照流程设计一屏一屏的呈现刺激材料，而眼动仪则再此过程中单纯的记录眼动数据。对于眼动仪来说，只有一整个试次的数据，而无法区分出具体是哪一个刺激屏的数据。

这里我们就引入了 Message 的概念，当 E-Prime 呈现某一屏刺激的时候，向眼动的数据里添加一个标记，这个标记即 Message 。那么我们就可以在眼动数据中根据 Message 精确定位某一屏开始的时间，从而将某一特定刺激屏的眼动数据截取出来，截取出来的这个数据时间段叫做 Interest Period。那么对应的，所有的屏幕都需要添加这样的标记，才能准确的截取出任意一个 Interest Period。

在 E-Prime 中，Message 是无法实时进行标记的，这是一个无法解决的系统问题。

一方面由于计算机系统原理的关系，显示命令不会立即执行，计算机需要在显示命令执行之前读取和处理需要显示的刺激材料。另一方面，即使计算机可以立即执行显示命令，刺激材料也不会立即显示在屏幕上。大家都知道显示器有一个参数叫做刷新率，大多数显示器的刷新率是 60 Hz ，即显示器 1 秒钟会刷新 60 次，每 16.6 ms 更新一次显示的内容。这也就是说如果显示器刚刚完成了一次刷新，在完成刷新后的 16.6 ms 内，无论计算机命令显示器刷新什么内容，都需要等 16.6 ms 后的下一次刷新才能显示出来。

这样的系统延迟在 Eyelink 自己的 Experiment Builder 得到了严格的优化，但是在 E-Prime 中却无法执行。在 E-Prime 3 中新增的 TaskEvent 功能其实可以侧面解决这个问题，但难度比较大，消费比低。

因此我们选择一种异步标记的方法，用 “xx时间之前发生了xx” 替代 “现在发生了xx” 。

Message 的标记函数是 `tracker.sendMessage [offset] “MessageText”` 。

`offset` 即 “xx时间之前发生了xx” 中的 “xx时间” ，“MessageText” 即 “发生了什么”。

在示例程序中，分别在 1-24 和 84-93 行的代码中进行了 PictureSlide 和 BlankScreen 的 Message 事件标记，我们统一放到本节中讲解。

### 1-24 行 PictureSlide 的 Message 标记

~~~ vb
'-------------------------------------------------------------------------------

' 创建一个名为 offset 的长整型变量用以记录自特定刺激屏呈现开始到当前的时间
' 改事件将作为 Message 的一部分，用以在数据中精确定位刺激屏开始的时间
Dim offsetValue As Long 

'----- 计算刺激屏开始的时间 -------------------------------------------------------

' 下面的代码通过一条 Message 向数据中精确记录 PictureSlide 屏开始的时间。
' PictureSlide 屏是在此脚本之前呈现的，因此我们需要计算出 PictureSlide 屏是多长时间之前呈现的
' 通过 Clock.ReadMillisec 函数可以获得当前的时间戳，单位 ms
' 通过 PictureSlide.OnsetTime 变量可以获取 PictureSlide 屏呈现在屏幕上时的时间戳
' 二者相减即为 PictureSlide 屏多久之前呈现在屏幕上
' 通过 trakcer.sendMessage 函数将 offset 也记录在 "PictureSilde_Onset" 的 Message 中
' 确保可以准确获取 PictureSlide 屏真正的开始时间
' 注意！" PictureSilde_Onset" 的第一个字符是空格，用以区分 offsetValue 和 Message 的文本
offsetValue = Clock.ReadMillisec - PictureSlide.OnsetTime
tracker.sendMessage offsetValue & " PictureSlide_Onset"

'----- 记录刺激呈现的屏幕图片，用以在 Data Viewer 中显示 -----------------------------

' 下面的代码用来适配 DataViewer 的刺激背景显示
' 详情请查阅：Eyelink Data Viewer User Manual > Protocol for EyeLink Data to Viewer Integration > Image Commands

' IMGLOAD 命令用来在 Data Viewer 中显示刺激的背景图片
' 你可以对特定图片出现的时间和位置进行编码
' 同事图片的位置坐标系也分为 FILL 、CENTER 和 TOP_LEFT 三种

' FILL 命令可以让图片满屏显示
'tracker.sendMessage offsetValue & " !V IMGLOAD FILL " & c.GetAttrib("imageName")

' CENTER 命令可以将图片的中心放置于指定的坐标位置
' 图片也可以根据输入的 width 和 height 参数在放置后进行缩放
tracker.sendMessage offsetValue & " !V IMGLOAD CENTER ../../" & _
 c.GetAttrib("imageName") & " " & c.GetAttrib("imXLoc") & " " & _
 c.GetAttrib("imYLoc") & " " & c.GetAttrib("imWidth") & " " & _
 c.GetAttrib("imHeight")

' TOP_LEFT 命令用来指定图片放置位置的左上角坐标
' 您也可以像下面的示例一样缺省 width 和 height 参数以放弃对图片的缩放
'tracker.sendMessage offsetValue & " !V IMGLOAD TOP_LEFT " & _
 'c.GetAttrib("imageName") & " 0 0"
~~~

### 84-93 行 BlankScreen 的 Message 标记

~~~ vb
'----- 计算刺激屏结束的时间 ------------------------------------------------------

' 现在我们需要在数据中对 BlankScreen 开始的时间进行标记，也就是 PictureSlide 结束的时间
' 和前面计算 PictureSlide 开始的时间相同，还是用当前的时间减去 BlankScreen 开始的时间
offsetValue = Clock.ReadMillisec - BlankScreen.OnsetTime

' 发送带有 offset 的 Message
tracker.sendMessage offsetValue & " BlankScreen_Onset"

' 在标记 BlankScreen 开始的同时，用 BlankScreen 的背景色清空 DataViewer 的显示
tracker.sendMessage offsetValue & " !V CLEAR 192 192 192"
~~~ 

## 1.2 添加兴趣区

> 根据你的实验情况选择是否要在实验过程中进行兴趣区的绘制。

兴趣区作为一个统计的区域单位我们在 Data Viewer 的教程中已经有了基本的介绍，不清楚的朋友请点击[>>>传送门<<<](/eyelink/dv_set_ia/)。

此部分代码仅对使用 Data Viewer 进行数据分析的用户有帮助。

~~~ vb
'---------------- 设置兴趣区 ---------------------------------------------------

' 下面的代码用于在数据中标注兴趣区，注意仅在 Data Viewer 中可见
' 详情请参阅: DataViewer User Manual > Protocol for EyeLink Data to Viewer Integration > Interest Area Commands

' 下面的代码用于在屏幕中心绘制一个 200 x 200 像素的矩形兴趣区
' 兴趣区 ID 为 1 ，名字为 center_rect_ia
'tracker.sendMessage offsetValue & " !V IAREA RECTANGLE 1 " & Display.XRes/2 _
 '-100 & " " & Display.YRes/2 - 100 & " " & Display.XRes/2 + 100 & " " & _
 'Display.YRes/2 + 100 & " center_rect_ia"

' 下面的代码可以在屏幕中心绘制一个直径 100 像素的圆形兴趣区
' 兴趣区 ID 为 2 ，名字为 center_circ_ia
'tracker.sendMessage offsetValue & " !V IAREA ELLIPSE 2 " & Display.XRes/2 _
 '-50 & " " & Display.YRes/2 - 50 & " " & Display.XRes/2 + 50 & " " & _
 'Display.YRes/2 + 50 & " center_circ_ia"

' 下面的代码用于绘制不规则兴趣区(钻石形状)
' 兴趣区 ID 为 3 ，名字为 freehand_ia
'tracker.sendMessage offsetValue & " !V IAREA FREEHAND 3 512,284 612,384 " & _
 '"512,484 412,384 freehand_ia"

' 下的代码将 PictureSlide 中的图片位置绘制为一个矩形兴趣区区。
tracker.sendMessage offsetValue & " !V IAREA RECTANGLE 1 " & _
 c.GetAttrib("imXLoc")-(c.GetAttrib("imWidth")/2) & " " & _
 c.GetAttrib("imYLoc")-(c.GetAttrib("imHeight")/2) & " " & _
 c.GetAttrib("imXLoc")+(c.GetAttrib("imWidth")/2) & " " & _
 c.GetAttrib("imYLoc")+(c.GetAttrib("imHeight")/2) & " " & _
 "IMAGE_IA"
~~~

## 1.3 停止记录

> 复制粘贴。

~~~ vb
'----- 停止记录 ------------------------------------------------------------

' 在每个试次结束的时候停止记录眼动数据

sleep 100 ' 等待 100 ms 以确保屏幕清空
tracker.stopRecording
~~~

## 1.4 记录实验变量和行为数据

> 根据实际需要仔细更改

如同我们在前面所说的，眼动和 E-Prime 实际上是两套系统。虽然是在 E-Prime 中写眼动实验，但其实两个系统各干各的，偶尔互相知会而已。

所以对于眼动数据而言，我们需要在其中标记每个试次是什么样的实验条件，呈现了什么样的刺激材料，行为反正是对是错或者用了多久。下面的代码就是用来做这样的工作。

~~~ vb
'----- 记录试次的变量 -------------------------------------------------------------
' TRIAL_VAR 命令用于对当前试次添加实验变量
' 每个实验变量需要单独发送一条 TRIAL_VAR 命令
' 在下面的示例中，记录了试次序号，刺激显示的内容，反应时和反应的正误等
' 其格式是：!V TRIAL_VAR <edfVarName> <value>

' 向眼动数据中记录试次序号和 E-Prime 中的 TrialList 的变量
tracker.sendMessage "!V TRIAL_VAR trial_number " & trialCounter
tracker.sendMessage "!V TRIAL_VAR trial_identifier " & c.GetAttrib("trialid")
tracker.sendMessage "!V TRIAL_VAR picture " & c.GetAttrib("imageName")
tracker.sendMessage "!V TRIAL_VAR maxDuration " & c.GetAttrib("duration")
tracker.sendMessage "!V TRIAL_VAR imWidth " & c.GetAttrib("imWidth")

' 每发送 5 条 Message 延迟 1 ms 以确保不回阻塞主试机的接受
sleep 1

tracker.sendMessage "!V TRIAL_VAR imHeight " & c.GetAttrib("imHeight")
tracker.sendMessage "!V TRIAL_VAR imXLoc " & c.GetAttrib("imXLoc")
tracker.sendMessage "!V TRIAL_VAR imYLoc " & c.GetAttrib("imYLoc")

' 记录行为反应数据

tracker.sendMessage "!V TRIAL_VAR RT " & PictureSlide.RT
tracker.sendMessage "!V TRIAL_VAR response " & PictureSlide.RESP

sleep 1

' 记录 RESPPressed 变量来标记试次是按键结束(1)或者非按键结束(0)
If PictureSlide.RT > 0 Then
	tracker.sendMessage "!V TRIAL_VAR RESPPressed 1"
	' 补发一条额外的 Message ，时间上与按键反应的时间同步
	tracker.sendMessage Clock.ReadMillisec - PictureSlide.RTTime & " KeyPress"
Else
	tracker.sendMessage "!V TRIAL_VAR RESPPressed 0"
End If

tracker.sendMessage "!V TRIAL_VAR corrResp " & PictureSlide.CRESP
tracker.sendMessage "!V TRIAL_VAR trialAccuracy " & PictureSlide.ACC

' 记录 E-Prime 的 subject 信息
tracker.sendMessage "!V TRIAL_VAR EP_subject " & c.GetAttrib("Subject")

sleep 1

tracker.sendMessage "!V TRIAL_VAR EP_session " & c.GetAttrib("Session")

' 记录相机校准方式
tracker.sendMessage "!V TRIAL_VAR camSetupType " & elCameraSetupType

' 以上添加到眼动数据中的变量均可在 Data Viewer 中查看并导出，您可以在变量导出的对话框底部找到他们，Data Viewer 会用蓝色单独标记出这些变量。
~~~

## 1.5 结束试次

> Ctrl C，Ctrl V。

~~~ vb
'----- 标记试次结束 ----------------------------------------------------------

' 向眼动数据写入 TRIAL_RESULT message，Data Viewer 会识别此 Message 并作为试次的结束
' TRIAL_RESULT 后的数字为 0 即可
' 详情请参阅：DataViewer manual > Protocol for EyeLink Data to Viewer Integration > Defining the Start and End of a Trial
tracker.sendMessage "TRIAL_RESULT 0" 
~~~

---

# 2. 源代码

~~~ vb
'-------------------------------------------------------------------------------

'Create a variable for holding the elapsed time since the experimental event
'occurred.  This value will be included as part of the message for the events
'and will be taken into account automatically by Data Viewer to position the
'message at the appropriate point in time.
Dim offsetValue As Long 'Variable to be set to give the offset for our messages

'-----SET/LOG STIMULUS ONSET TIME-----------------------------------------------

'The below code sends a message to the eye tracker recording the exact time that
'the PictureSlide screen was displayed (start of trial display).  Since the
'screen was displayed some time ago, to get the correct time we take the current
'time in milliseconds (Clock.ReadMillisec), and subtract the 'PictureSlide'
'screen onset time as reported by E-Prime (PictureSlide.OnsetTime).  This value
'is included in the message sent to the eye tracking data file as an offset.
'Data Viewer will automatically subtract this offset time from the EDF timestamp
'of the message so the 'PictureSlide_Onset' message appears in the proper
'location in the eye data, with the appropriate trial timestamp.  If you do not
'use Data Viewer, the offset will appear in the PictureSlide_Onset message so
'that you can note proper onset times by subtracting the offset from the message
'timestamp.
offsetValue = Clock.ReadMillisec - PictureSlide.OnsetTime
tracker.sendMessage offsetValue & " PictureSlide_Onset"

'-----SET TRIAL IMAGE INFORMATION (for Data Viewer display)---------------------

'The following code is for EyeLink Data Viewer integration purposes.  See
'section "Protocol for EyeLink Data to Viewer Integration > Image Commands" of
'the EyeLink Data Viewer User Manual.

'The IMGLOAD command is used to show a background image in Data Viewer.  This
'will code the time and location at which the image should appear.  You can
'choose to use a FILL, CENTER, or TOP_LEFT command, descriptions and examples
'of which are below:

'A FILL command sizes the image to fill the display
'tracker.sendMessage offsetValue & " !V IMGLOAD FILL " & _
 'c.GetAttrib("imageName")

'A CENTER command places the center of the image at the specified coordinates.
'The image will only be re-sized if optional width and height parameters are
'provided after the coordinates.
tracker.sendMessage offsetValue & " !V IMGLOAD CENTER ../../" & _
 c.GetAttrib("imageName") & " " & c.GetAttrib("imXLoc") & " " & _
 c.GetAttrib("imYLoc") & " " & c.GetAttrib("imWidth") & " " & _
 c.GetAttrib("imHeight")

'A TOP_LEFT command places the top left of the image at the specified
'coordinates.  The image will only be re-sized if optional width and height
'parameters are provided after the coordinates.
'tracker.sendMessage offsetValue & " !V IMGLOAD TOP_LEFT " & _
 'c.GetAttrib("imageName") & " 0 0"


'----------------SETTING INTEREST AREAS-----------------------------------------

'The following code is for EyeLink Data Viewer integration purposes.  See
'section "Protocol for EyeLink Data to Viewer Integration > Interest Area
'Commands" of the EyeLink Data Viewer User Manual

'The following draws a 200 x 200 pixel interest area at screen center:
'tracker.sendMessage offsetValue & " !V IAREA RECTANGLE 1 " & Display.XRes/2 _
 '-100 & " " & Display.YRes/2 - 100 & " " & Display.XRes/2 + 100 & " " & _
 'Display.YRes/2 + 100 & " center_rect_ia"

'To draw a 100 pixel diameter circular interest area at screen center:
'tracker.sendMessage offsetValue & " !V IAREA ELLIPSE 2 " & Display.XRes/2 _
 '-50 & " " & Display.YRes/2 - 50 & " " & Display.XRes/2 + 50 & " " & _
 'Display.YRes/2 + 50 & " center_circ_ia"

'The following example draws a freehand interest area (here a diamond).
'tracker.sendMessage offsetValue & " !V IAREA FREEHAND 3 512,284 612,384 " & _
 '"512,484 412,384 freehand_ia"

'The following draws a rectangular interest area around the presented image.
tracker.sendMessage offsetValue & " !V IAREA RECTANGLE 1 " & _
 c.GetAttrib("imXLoc")-(c.GetAttrib("imWidth")/2) & " " & _
 c.GetAttrib("imYLoc")-(c.GetAttrib("imHeight")/2) & " " & _
 c.GetAttrib("imXLoc")+(c.GetAttrib("imWidth")/2) & " " & _
 c.GetAttrib("imYLoc")+(c.GetAttrib("imHeight")/2) & " " & _
 "IMAGE_IA"

'-----SET/LOG STIMULUS REMOVAL TIME---------------------------------------------

'Now we want to mark the onset time of the blank screen in the EDF, which tells
'us when our image presentation ended.  To do so, we first calculate the
'offsetValue for our message by subtracting the onset time of the "BlankScreen"
'display (BlankScreen.OnsetTime) from the current time (Clock.ReadMillisec).
offsetValue = Clock.ReadMillisec - BlankScreen.OnsetTime

'Then we send a message that begins with this offsetValue time.
tracker.sendMessage offsetValue & " BlankScreen_Onset"

'Set screen clear message for Data Viewer as well. 192, 192, 192 is the RGB for
'our slide background colours (see slide properties).
tracker.sendMessage offsetValue & " !V CLEAR 192 192 192"

'-----STOP RECORDING------------------------------------------------------------

'Here we issue the stop recording command to stop the tracker recording gaze
'location between trials.

sleep 100 'Allow 100 ms for clean up before ending recording
tracker.stopRecording

'-----SENDING TRIAL VARIABLE VALUES---------------------------------------------
'The TRIAL_VAR command sets a trial variable and value for the given trial. 
'Send one message for each trial condition variable and its corresponding value.
'Examples of trial variables include trial number, stimulus displayed, RT,
'accuracy, etc.  Format is:
'!V TRIAL_VAR <edfVarName> <value>

'Adding trial counter and general E-Prime list columns as variables to the .edf
tracker.sendMessage "!V TRIAL_VAR trial_number " & trialCounter
tracker.sendMessage "!V TRIAL_VAR trial_identifier " & c.GetAttrib("trialid")
tracker.sendMessage "!V TRIAL_VAR picture " & c.GetAttrib("imageName")
tracker.sendMessage "!V TRIAL_VAR maxDuration " & c.GetAttrib("duration")
tracker.sendMessage "!V TRIAL_VAR imWidth " & c.GetAttrib("imWidth")

'Including a sleep 1 line after every 5 contiguous messages to ensure we don't
'flood the Host.
sleep 1

tracker.sendMessage "!V TRIAL_VAR imHeight " & c.GetAttrib("imHeight")
tracker.sendMessage "!V TRIAL_VAR imXLoc " & c.GetAttrib("imXLoc")
tracker.sendMessage "!V TRIAL_VAR imYLoc " & c.GetAttrib("imYLoc")

'Adding trial performance variables (response time, response given, correct
'response, accuracy).

tracker.sendMessage "!V TRIAL_VAR RT " & PictureSlide.RT
tracker.sendMessage "!V TRIAL_VAR response " & PictureSlide.RESP

sleep 1

'Add a RESPPressed variable to note whether the trial ended via keypress (1) or
'not (0).
If PictureSlide.RT > 0 Then
	tracker.sendMessage "!V TRIAL_VAR RESPPressed 1"
	'Additionally include a KeyPress message in the .edf at the time the
	'response was issued.
	tracker.sendMessage Clock.ReadMillisec - PictureSlide.RTTime & " KeyPress"
Else
	tracker.sendMessage "!V TRIAL_VAR RESPPressed 0"
End If

tracker.sendMessage "!V TRIAL_VAR corrResp " & PictureSlide.CRESP
tracker.sendMessage "!V TRIAL_VAR trialAccuracy " & PictureSlide.ACC

'Adding E-Prime subject and session values as variables to the .edf.
tracker.sendMessage "!V TRIAL_VAR EP_subject " & c.GetAttrib("Subject")

sleep 1

tracker.sendMessage "!V TRIAL_VAR EP_session " & c.GetAttrib("Session")

'Adding camera setup type to the .edf.
tracker.sendMessage "!V TRIAL_VAR camSetupType " & elCameraSetupType

'The variables set with these messages will be available as Trial Variables in
'Data Viewer, and can be included in any of the output reports.  In the report
'variable selection dialog, these user-defined variables will be at the bottom
'of the list, in blue.

'-----DEFINE END OF TRIAL (for Data Viewer)-------------------------------------

'Write TRIAL_RESULT message to EDF: marks the end of a trial for DataViewer.
'See DataViewer manual section: Protocol for EyeLink Data to Viewer
'Integration > Defining the Start and End of a Trial
tracker.sendMessage "TRIAL_RESULT 0" 
~~~

---

以上。
