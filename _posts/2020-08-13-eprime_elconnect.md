---
title: "elConnect脚本(E-Prime眼动编程)"
excerpt: "用脚本建立 E-Prime 与 Eyelink Host 的链接。"
read_time: false
header:
  overlay_image: /assets/images/eprime_header.png
  overlay_filter: 0.5
  actions:
    - label: "下载示例文件(gsbv)"
      url: "https://pan.baidu.com/s/1dLvtJpazXb8DARSqknLyWg"
categories:
  - Eyelink
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

elConnect 脚本的位置在实验的指导语之后，elCameraSetup之前。

通过本脚本的代码，使 E-Prime 与 Eyelink 主试机相连接。

您可以直接复制示例项目中的整个脚本到自己的项目中，并进行适当修改。

下文会将 elConnect 脚本拆解成多个部分进行分段讲解。

## 请求 .edf 文件名。

> 此部分完全复制粘贴即可。

``` VB
Dim edfFileName As String   '定义一个名为edfFileName的字符型变量Mouse.ShowCursor True       '显示鼠标  
edfFileName = AskBox("Please enter an EDF file name" , "Test.edf")  
'请求一个.edf文件的名字，显示一个对话框，请求文本内容
'提示内容是"Please enter an EDF file name"
'输入框中的默认文本为"Test.edf"
'将输入的内容赋值给 edfFileName

'检测输入的字符串长度是否为0，即是否没有输入文本内容
'若没有输入任何内容，则对 edfFileName 赋值为 "Test.edf"If len(edfFileName) = 0 Then    	edfFileName = "Test.edf"End If
'隐藏鼠标Mouse.ShowCursor False'报告鼠标是否被隐藏Debug.Print "mouse is visible? " &Mouse.IsCursorVisible()```## 设置 CameraSetup 相关参数> 此部分根据需要更改校准点的**颜色**和**尺寸**设置。``` VB
'设置 CameraSetup 过程中的相关参数
'包括 Calibrate 、 Validate 和 DriftCorrection 

'设置 CameraSetup 背景色为 "192,192,192" 灰色cal_background = "192,192,192" 

'设置 CameraSetup 背景色为 "0,0,0" 黑色cal_foreground = "0,0,0"
cal_target_size=6       '设置校准点的外圈尺寸cal_pen_width =4        '设置校准点的圆环宽度   
'其内圈尺寸为 cal_target_size - cal_pen_width = 2```

## 建立链接

> 此部分完全复制粘贴即可。

``` VB
'暂时提升程序的线程优先级'否则 E-Prime 与 Eyelink Host 的链接会出现超时Dim nPriority As IntegernPriority = GetOSThreadPriority()SetOSThreadPriority 3On Error GoTo ErrorHandleSet elutil  = CreateObject("SREYELINK.EyeLinkUtil") '创建Eyelink链接Set tracker = CreateObject("SREYELINK.EyeLink")     '创建Eyelink对象tracker.open "100.1.1.1",0   '打开Eyelink链接tracker.openDataFile edfFileName '主试机打开 .edf 文件tracker.sendCommand "screen_pixel_coords =  0 0 " & Display.XRes & " " & Display.YRes 
'告知被试机显示器的分辨率范围tracker.sendCommand "calibration_type = HV9" '设置校准点类型tracker.sendMessage "DISPLAY_COORDS 0 0 " & Display.XRes & " " & Display.YRes 
'向 .edf 文件中写入被试机的分辨率信息tracker.sendMessage "FRAMERATE " & Display.CalculatedRefreshRate 
'向 .edf 文件中写入屏幕刷新率```## 根据主试机版本适配设置参数

> 此部分完全复制粘贴即可。
> 
> 本段代码没有学习的必要，直接使用。``` VB'SET UP TRACKER CONFIGURATION 'set parser saccade thresholds (conservative settings) Dim trVerStr As StringDim vindx As Integer Dim trswVer As IntegerDim eyelink_ver As Integereyelink_ver = tracker.getTrackerVersion()trVerStr = tracker.getTrackerVersionString()trVerStr = trim(trVerStr)vindx =instr (1,trVerStr,"EYELINK CL")If vindx >0 Then	trVerStr =  Mid(trVerStr,len("EYELINK CL")+1)	trVerStr = trim(trVerStr)	trswVer = trVerStrElse	trswVer = 0End IfIf eyelink_ver>=2 Then      tracker.sendCommand "select_parser_configuration 0" ' 0 = standard sensitivity 	  If eyelink_ver = 2 Then 'turn off scenelink camera stuff		tracker.sendCommand  "scene_camera_gazemap = NO"	  End If    Else  	  tracker.sendCommand "saccade_velocity_threshold = 35"	  tracker.sendCommand "saccade_acceleration_threshold = 9500"End If'set EDF file contents tracker.sendCommand "file_event_filter = LEFT,RIGHT,FIXATION,SACCADE,BLINK,MESSAGE,BUTTON,INPUT"If trswVer>=4 Then	tracker.sendCommand "file_sample_data  = LEFT,RIGHT,GAZE,AREA,GAZERES,STATUS,HTARGET,INPUT"Else	tracker.sendCommand "file_sample_data  = LEFT,RIGHT,GAZE,AREA,GAZERES,STATUS,INPUT"End If'set link data (used for gaze cursor) tracker.sendCommand "link_event_filter = LEFT,RIGHT,FIXATION,SACCADE,BLINK,BUTTON,INPUT"If trswVer>=4 Then	tracker.sendCommand "link_sample_data  = LEFT,RIGHT,GAZE,GAZERES,AREA,STATUS,HTARGET,INPUT"Else	tracker.sendCommand "link_sample_data  = LEFT,RIGHT,GAZE,GAZERES,AREA,STATUS,INPUT"End If```

## 其他设置

> 此部分完全复制粘贴即可。

``` VB'设置老式手柄反应盒的 5 号按钮可以用来接受 DriftCorrection tracker.sendCommand "button_function 5 'accept_target_fixation'"'重新将程序线程优先级为原始状态SetOSThreadPriority nPriority
```

---

以上。