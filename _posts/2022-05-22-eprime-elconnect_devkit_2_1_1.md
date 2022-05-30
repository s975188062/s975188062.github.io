---
title: "elConnect脚本(E-Prime3 with DevKit v2.1.1)"
excerpt: "用脚本建立 E-Prime 与 Eyelink Host 的链接。"
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

elConnect 脚本通常位于实验的最开始，虽然在示例中该脚本的位置在指导语之后，但 Charlie 建议将其放在实验的最开始。

程序通过本脚本与 Eyelink 的主试机建立连接。

您需要做的是将代码复制到自己的程序中，并根据需要进行适当修改。

## 1.1 设置基本参数

> 一般情况下，不需要修改这部分参数，直接将其复制粘贴进入自己的程序即可。但如果您遇到了程序卡死或者其他莫名其妙的 Bug ，请根据代码中的注释对变量值进行适当修改。

~~~ vb
' 眼动仪校准的设置
' 1 - 执行校准的时候可以在主试机上按 “回车” 来查看眼睛的视频图像
' 2 - 无法在校准过程中查看眼睛图像
' 更多信息请参阅 README inline 中的 18 - 36 行
elCameraSetupType = 1

' 是否使用设备挂起和重调用功能
' 如果您的电脑是 Windows 7 或者更早的系统，请将其设置为 1 ，否则设置为 0
' 如实验过程中出现了卡死的情况，请尝试改变该变量的数值
useSuspend = 0 

' 线程优先级调用相关设置
' 如果您的电脑是 Windows 7 或者更早的系统，请将其设置为 1 ，否则设置为 0
' 如实验过程中出现了卡死的情况，请尝试改变该变量的数值
' 若 0 和 1 还是出现卡死的情况，请尝试将其更改为 2 ，通常会在少量的 Windows 10 电脑中出现这样的问题
usePriority = 0 

' 设置是否在运行程序的时候链接眼动仪
' 我们通常会在调试程序的时候更改此变量的设置
' 0 - 连接眼动仪运行程序
' 1 - 跳过所有眼动相关函数
' 正式运行实验程序手机数据的时候请务必将其设置为 0 
dummyMode = 0 

' 1 - 开启校准提示音
' 0 - 关闭校准提示音
calibrationSoundsOn = 1 
~~~

## 1.2 命名眼动数据文件

> 此部分全部复制粘贴即可。

~~~ vb
setMouseState True ' 显示鼠标

' 显示一个窗口，要求输入 *.edf 数据文件的名称
' 输入的名称仅支持不超过 8 个字符，且仅支持使用字母、数字和下划线进行命名，英文字母不区分大小写
getEDFName

setMouseState False ' 隐藏鼠标
~~~

## 1.3 设置校准相关参数

> 根据需要自行更改。

~~~ vb
' 校准、验证和飘逸检查的背景色
' 注意！！！此颜色应与实验过程中的背景色相匹配
cal_background = "192,192,192" 

' 设置校准点颜色
cal_foreground = "0,0,0"       

' 设置校准点外环尺寸
cal_target_size=10

' 设置校准点内环尺寸
cal_pen_width =5 

' 是否使用自定义的校准点
' 0 - 使用默认的圆环校准点
' 1 - 使用静态图片作为校准点
' 2 - 使用动态图片作为校准点
' 更多信息请参阅 README inline 中的 62 - 65 行
use_cust_cal = 1 

If use_cust_cal = 1 Then
	cust_cal_stim="Stimuli/EL_calibDot.png" ' 指定静态自定义校准点文件
ElseIf use_cust_cal = 2 Then
	cust_cal_stim="Stimuli/EL_calibration.avi" ' 指定动态自定义校准点文件
End If
~~~ 

## 1.4 其他系统设置

> 根据情况可能会需要更改主试机 IP 为实际值，其他不需要更改。

~~~ vb
If usePriority > 0 Then
	nPriority = GetOSThreadPriority()
	' 暂时将线程优先级设置为普通应用程序，否则在连接到跟踪器时会超时。
	SetOSThreadPriority 3
End If

On Error GoTo ErrorHandle
Set elutil = CreateObject("SREYELINK.EyeLinkUtil")  ' 创建与主试机通信所必须的对象
Set tracker = CreateObject("SREYELINK.EyeLink")     ' 创建与眼动仪通信所必须的对象

If dummyMode = 0 Then
	tracker.open "100.1.1.1",0	' 连接眼动仪，默认主试机 IP 为 100.1.1.1
ElseIf dummyMode = 1 Then
	tracker.dummyOpen		' 创建 dummy 连接，用于在没有眼动仪的状态下调试程序
End If

tracker.openDataFile edfFileName ' 在主试机上打开数据文件

' 向 eprime 日志中添加 "EDFNAMELOG" 字段，用以将眼动数据文件与 E-Prime 数据文件对应
' 按需使用即可，Charile 建议统一 *.edf 文件和 E-Prime 数据文件的命名
If Not c.AttribExists("EDFNAMELOG") Then c.SetAttrib "EDFNAMELOG", edfFileName

' 向 *.edf 文件中添加实验信息，实验的名称将会可以在 Data Viewer 中查看
tracker.sendCommand "add_file_preamble_text 'RECORDED BY E-Prime Project " _
 & c.GetAttrib("Experiment") & "'"

' 告知主试机实验程序分辨率
tracker.sendCommand "screen_pixel_coords = 0 0 " & Display.XRes-1 & " " &_
 Display.YRes-1

' 设置校准类型 H3、HV3、HV5、HV9 或 HV13
tracker.sendCommand "calibration_type = HV9"

' 在 *.edf 文件中记录实验程序的分辨率
' 更多信息请参阅 Data Viewer 手册：Protocol for EyeLink Data to Viewer Integration > Pre-trial Message Commands
tracker.sendMessage "DISPLAY_COORDS 0 0 " & Display.XRes-1 & " " &_
 Display.YRes-1

' 在 *.edf 文件中记录实验程序的屏幕刷新率
tracker.sendMessage "FRAMERATE " & Display.CalculatedRefreshRate

' 根据眼动仪型号匹配最优设置
' getTrackerVersionString() 函数返回当前主试机的系统版本
' 2.x 对应 EyeLink II
' 4.x 对应 EyeLink 1000,
' 5.x 对应 EyeLink 1000 Plus
' 6.x 对应 EyeLink Portable Duo
' 以下代码将根据主试机的版本情况判断是否可以记录 HTARGET 信息
' HTARGET 信息仅在主试机版本 >= 4 的 Remote 模式下才可记录

Dim el_soft_ver_str As String
Dim el_soft_ver_num As Integer

el_soft_ver_str = trim(tracker.getTrackerVersionString())
If instr (1,el_soft_ver_str,"EYELINK CL") > 0 Then
	el_soft_ver_num = trim(Mid(el_soft_ver_str,len("EYELINK CL")+1))
End If

' 设置记录可用的 SAMPLE 或 EVENT 数据
' 更多信息请参阅 EyeLinkProgrammers Guide manual > Useful EyeLink Commands > File Data Control & Link Data Control

' 设置需要记录到数据中的事件类型。默认全部记录以防万一。
tracker.sendCommand "file_event_filter = LEFT,RIGHT,FIXATION,SACCADE,BLINK," _
	& "MESSAGE,BUTTON,INPUT"
	
' 设置需要在线传输的事件类型。默认全部记录以防万一。
tracker.sendCommand "link_event_filter = LEFT,RIGHT,FIXATION,SACCADE,BLINK," _
	&"BUTTON,FIXUPDATE,INPUT"

' 设置需要记录到数据中或者在线传输的原始数据类型。默认全部记录以防万一。
' 根据主试机版本确定是否支持 HTARGET 数据
If el_soft_ver_num >= 4 Then
	tracker.sendCommand "file_sample_data = LEFT,RIGHT,GAZE,HREF,RAW,AREA," _
	& "HTARGET,GAZERES,BUTTON,STATUS,INPUT"
	tracker.sendCommand "link_sample_data = LEFT,RIGHT,GAZE,AREA,HTARGET," _
	& "GAZERES,STATUS,INPUT"
Else
	tracker.sendCommand "file_sample_data = LEFT,RIGHT,GAZE,HREF,RAW,AREA," _
	& "GAZERES,BUTTON,STATUS,INPUT"
	tracker.sendCommand "link_sample_data = LEFT,RIGHT,GAZE,AREA,GAZERES," _
	& "STATUS,INPUT"
End If

' 设置可以通过老式手柄反应盒的按键 5 进行校准和漂移矫正过程中的确认操作
tracker.sendCommand "button_function 5 'accept_target_fixation'"

' 清空主试机监测界面的显示
tracker.sendCommand "clear_screen 0"

If usePriority = 1 Then
	' 重制线程优先级
	SetOSThreadPriority nPriority
End If
~~~

---

# 2. 源代码

~~~ vb
'-------------------------------------------------------------------------------

elCameraSetupType = 1 '1 for withCam (allows camera transfer in camera setup) or
			'2 for noCam (no camera transfer, but no transition delays).  See
			'README lines 18 - 36 for more information.

useSuspend = 0 'Controls whether device suspend and resume calls are used.  If
	'using Windows 7 or earlier, set to 1.  Otherwise, leave at 0.  If issues
	'are observed with experiment freezing, try switching this value.

usePriority = 0 'Controls whether thread priority changing calls are used.  If
	'using Windows 7 or earlier, set to 1.  Otherwise, leave at 0.  If issues
	'are observed with experiment freezing, try switching this value.  If both
	'values lead to freezing, please set to 2 -- this enables all but the last
	'thread priority call at the end of the elConnect script, which was seen to
	'cause issues on a small number of Windows 10 systems.

dummyMode = 0 '0 for an active tracker connection (for running a project with
	'eye tracking enabled) or 1 for running in dummy mode (for testing without
	'a connected tracker).

calibrationSoundsOn = 1 '1 to enable calibration sounds, 0 to disable.

setMouseState True ' show mouse cursor

'Ask edf file name -- limited to 8 alphanumeric characters (plus .edf extension)
'No special characters allowed, barring underscore.  See User Script for code:
getEDFName

setMouseState False ' hide the mouse cursor

cal_background = "192,192,192" ' Set background color for calibration/validation
			'/drift check screens.  This should be set to match the background
			'color of the main experiment.  
cal_foreground = "0,0,0"       ' Set foreground color for calibration/validation
			'/drift check screens. This is the color of the dots during the
			'calibration/validation/drift check.   
cal_target_size=10 ' Set calibration target circle size:
				  ' withCam - Radius in pixels to the outer circle's outer edge.
				  ' noCam - Radius in pixels to the middle of the line area of
				  '	 the outer circle.
cal_pen_width =5 ' Set calibration target center hole size / pen width:
				 ' withCam - Radius in pixels of the inner circle/inner hole
				 '  size
				 ' noCam - Thickness of outer circle line in pixels.  Half the
				 '  value will fall on either side of the circular line
				 '  specified by cal_target_size, so the diameter of the
				 '  "outside circle" of the calibration dot will extend and the
				 '  diameter of the "inner circle" target hole will shrink.
use_cust_cal = 1 ' 0=off, 1=static image, 2 = video
			' SEE README LINES 62-65.
If use_cust_cal = 1 Then
	cust_cal_stim="Stimuli/EL_calibDot.png" 'set calibration target image file
ElseIf use_cust_cal = 2 Then
	cust_cal_stim="Stimuli/EL_calibration.avi" 'set calibration target video
											   ' file
End If

If usePriority > 0 Then
	nPriority = GetOSThreadPriority()
	'Temporarily set the thread priority to a normal application
	'otherwise you will get timed out when connecting to the tracker.
	SetOSThreadPriority 3
End If

On Error GoTo ErrorHandle
Set elutil = CreateObject("SREYELINK.EyeLinkUtil") 'get instance of EyeLinkUtil
	'(necessary to get information from the Host PC)
Set tracker = CreateObject("SREYELINK.EyeLink") ' get instance of EyeLink object
	'(necessary to interface with the tracker)

If dummyMode = 0 Then
	tracker.open "100.1.1.1",0	' open tracker connection (default IP 100.1.1.1)
ElseIf dummyMode = 1 Then
	tracker.dummyOpen		' open dummy mode connection for testing without an
							' active tracker connection.
End If

tracker.openDataFile edfFileName ' open edf file

'Set an attribute "EDFNAMELOG" to log the .edf name chosen into the output
'.edat and .txt files.  This is to help verify .edf/session match-up, if needed.
If Not c.AttribExists("EDFNAMELOG") Then c.SetAttrib "EDFNAMELOG", edfFileName

'Mark the .edf with the E-Prime session information.  Starting the text with
'"RECORDED BY" will make the text available in Data Viewer's Inspector window
'(click the EDF session node in the top panel, then look for the "Recorded By:"
'field in the bottom panel of the Inspector).
tracker.sendCommand "add_file_preamble_text 'RECORDED BY E-Prime Project " _
 & c.GetAttrib("Experiment") & "'"

'Set display coordinates for EyeLink data by entering left, top, right and
'bottom coordinates in screen pixels
tracker.sendCommand "screen_pixel_coords = 0 0 " & Display.XRes-1 & " " &_
 Display.YRes-1

'Set calibration type (e.g., HV9, HV13)
tracker.sendCommand "calibration_type = HV9"

'Write DISPLAY_COORDS message to EDF file: sets display coordinates in
'DataViewer.  See DataViewer manual section: Protocol for EyeLink Data to
'Viewer Integration > Pre-trial Message Commands
tracker.sendMessage "DISPLAY_COORDS 0 0 " & Display.XRes-1 & " " &_
 Display.YRes-1

'Add a message to the EDF reporting refresh rate
tracker.sendMessage "FRAMERATE " & Display.CalculatedRefreshRate

'SET UP TRACKER CONFIGURATION 
'Get EyeLink tracker software version
'getTrackerVersionString() returns the tracker software version.  The version
'number will be 2.x for the EyeLink II, 4.x for the EyeLink 1000, 5.x for the
'EyeLink 1000 Plus, or 6.x for the EyeLink Portable Duo.  This Is used below to
'set whether HTARGET information can be recorded (version >= 4).

Dim el_soft_ver_str As String
Dim el_soft_ver_num As Integer

el_soft_ver_str = trim(tracker.getTrackerVersionString())
If instr (1,el_soft_ver_str,"EYELINK CL") > 0 Then
	el_soft_ver_num = trim(Mid(el_soft_ver_str,len("EYELINK CL")+1))
End If

'SELECT AVAILABLE SAMPLE/EVENT DATA
'See EyeLinkProgrammers Guide manual > Useful EyeLink Commands > File Data
'Control & Link Data Control

'Select which events are saved in the EDF file. Include everything just in case 
tracker.sendCommand "file_event_filter = LEFT,RIGHT,FIXATION,SACCADE,BLINK," _
	& "MESSAGE,BUTTON,INPUT"
	
'Select which events are available online for gaze-contingent experiments.
'Include everything just in case
tracker.sendCommand "link_event_filter = LEFT,RIGHT,FIXATION,SACCADE,BLINK," _
	&"BUTTON,FIXUPDATE,INPUT"

'Select the sample data to save in EDF file or have available online. Include
'everything just in case
'Check tracker version and include 'HTARGET' to save head target sticker data
'for supported eye trackers
If el_soft_ver_num >= 4 Then
	tracker.sendCommand "file_sample_data = LEFT,RIGHT,GAZE,HREF,RAW,AREA," _
	& "HTARGET,GAZERES,BUTTON,STATUS,INPUT"
	tracker.sendCommand "link_sample_data = LEFT,RIGHT,GAZE,AREA,HTARGET," _
	& "GAZERES,STATUS,INPUT"
Else
	tracker.sendCommand "file_sample_data = LEFT,RIGHT,GAZE,HREF,RAW,AREA," _
	& "GAZERES,BUTTON,STATUS,INPUT"
	tracker.sendCommand "link_sample_data = LEFT,RIGHT,GAZE,AREA,GAZERES," _
	& "STATUS,INPUT"
End If

'Allow a supported EyeLink Host PC button box to accept calibration or
'drift-check targets via button 5
tracker.sendCommand "button_function 5 'accept_target_fixation'"

'Clear Host PC display from any previus drawing
tracker.sendCommand "clear_screen 0"

If usePriority = 1 Then
	'Reset the thread priority
	SetOSThreadPriority nPriority
End If

~~~

---

以上。