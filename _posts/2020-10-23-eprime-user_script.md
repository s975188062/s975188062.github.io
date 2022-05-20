---
title: "User Script(E-Prime with DevKit v1.11.5)"
excerpt: "放到E-Prime程序的最前面，不需要理解，完全复制粘贴。"
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

> 本篇是基于安装了 `1.11.5` 版本 Development Kit 的用户，如果您安装的是 `2.1.1` 版本的 Development Kit ，请点击 [>>>传送门<<<](/eyelink/e-prime/eprime_user_script_devkit_2_1_1/)。
{: .notice--warning}

# 1.User Script

本文中所涉及的是 Eyelink 在 E-Prime 下运行的依赖代码。

理解起来需要较多的编程基础，但好在我们不需要理解。

在任何 Eyelink 的 E-Prime 实验中，都需要在 E-Prime 的 User Script中填入相同的依赖代码，完全复制粘贴即可。

E-Prime 3 中 User Script 的位置如下：

![eprime-user_script](/assets/images/eprime-user_script.png)

E-Prime 2 中 User Script 的位置如下：

![eprime-userscript-2](/assets/images/eprime-userscript-2.png)

如果您有其他的 User Script，将 Eyelink 相关的代码复制粘贴在您的代码后面即可。

---

# 2.源代码

将下面的全部代码复制粘贴，不要修改。

~~~ vb
'
' Copyright (c) 1997 - 2009 by SR Research Ltd., All Rights Reserved        
'                                                                           
' This software is provided as is without warranty of any kind.  The entire 
' risk as to the results and performance of this software is assumed by the 
' user. SR Research Ltd. disclaims all warranties, either express or implied
' ,including but not limited, the implied warranties of merchantability,    
' fitness for a particular purpose, title and noninfringement, with respect 
' to this software.                                                         
'                                                                           
'                                                                           
' For non-commercial use by Eyelink licencees only                          
'                                                                                                    
'
'
'

Dim tracker As Object        'global reference to hold tracker object
Dim elutil As Object         'global reference to hold elutil object
Dim cal_background As String 'calibration background color
Dim cal_foreground As String 'calibration foreground
Dim cal_target_size As Integer 'calibration target size
Dim cal_pen_width As Integer   'calibration pen width, can be used to set the inner hole size.
							   'The inner hole size = cal_target_size-cal_pen_width

Dim incount As Integer       'global integer to hold input count




'############################### start calibration/validation/drift correct ##############
'
' The following routines provide drawing/input support for calibration,validation,drift
'correct and camera setup features.  In e-prime 1.x the GDICal is used for all drawings except
'for custom drift correct target.  In e-prime 2.x the GDICal is used conditionally and BusyCal is 
'used conditionally to overcome some incompatibilities between the GDICal and e-prime2.x.
'
'In e-prime 2.x, the GDICal is used, by disabling the e-prime 2.x graphics environment.  If e-prime 2.x
'graphics environment is not disabled, all the graphics will be presented behind the e-prime 2.x 
' full screen window  so that the calibration drawings will not show up.   Under certain circumstances, eg.
'when custom drift correct target is presented, the BusyCal is used for calibration as disabling of e-prime 2.x
'drawing environment is not possible.   
'
'Note that the busycal in e-prime does not support cameraimage over the link.
'
						
							
'
'Sub doCameraSetup
'Input: None
'Output:None
'Purpose: Setup gdical and calls doTrackerSetup to perform camera setup.
'Call this subroutine to do camera setup. 
'
'
Sub doCameraSetup
	Dim gcal As Object
	Dim theHistory As RteCollection
	Set theHistory = Keyboard.History ' we want to ignore the keys pressed while calibration.

	theHistory.RemoveAll
	Set gcal = elutil.getGDICal()

	Rte.DeviceManager.Suspend 'This code may switch the resolution back to the previous resolution
	

	gcal.enableKeyCollection True 'tell the com interface to start collecting keyboard
	gcal.setCalibrationTargetSize 2,9
	gcal.setCalibrationColors CColor(cal_foreground), CColor(cal_background)

	gcal.setCalibrationWindow -1
	tracker.doTrackerSetup
	Rte.DeviceManager.Resume		
	
	gcal.enableKeyCollection False ' tell the com interface to stop collecting keyboard
	Set gcal = Nothing
	
	theHistory.RemoveAll
End Sub

'
'Sub doDriftCorrect
'Input: 
'	xloc - xlocation of the drift correction target
'	yloc - ylocation of the drift correction target
'   draw - if false, no target is drawn. If this is false, optionally pass in customDrift
'			TextDisplay, so that custom target can be re-drawn if drift correction is cancelled and calibration is performed.
'	allow_setup - if this is false, pressing escape does not perform a calibration.
'	customDrift - optional argument of type TextDisplay to re-draw custom target.
'Output:None
'Purpose: Setup gdi cal and calls doDriftCorrect to perform drift correct.
'Call this subroutine to do drift correct. 
'
'
Sub doDriftCorrect(xloc As Integer, yloc As Integer, draw As Boolean, allow_setup As Boolean,Optional customDrift As Variant)
	Dim gcal As Object
	Dim theHistory As RteCollection
	Set theHistory = Keyboard.History ' we want to ignore the keys pressed while calibration.
	theHistory.RemoveAll

	Dim cd As TextDisplay

	Set gcal = elutil.getGDICal()
	gcal.enableKeyCollection True 'tell the com interface to start collecting keyboard
		
	' call do drift correct with allow setup 0
	' if the return value is 27 and allow setup value 1 then 
	' do  normal calibration as in the else part of the previous else (do a camera setup.)
	' once the camera setup Is done, recall Do drift correct.
		
	If Not isMissing(customDrift)  Then
		Set cd = customDrift
		cd.draw
			
		Dim ret As Integer
		ret = tracker.doDriftCorrectEx( xloc, yloc, 0, 0)			

		While ret=27 And allow_setup = True 		
	
			Rte.DeviceManager.Suspend  'This code may switch the resolution back to the previous resolution
	
			gcal.enableKeyCollection True 'tell the com interface to start collecting keyboard
			gcal.setCalibrationTargetSize 2,9
			gcal.setCalibrationColors CColor(cal_foreground), CColor(cal_background) 'cd.BackColor				
			
			gcal.setCalibrationWindow -1
			tracker.doTrackerSetup
			Rte.DeviceManager.Resume			
		
			cd.draw
			ret = tracker.doDriftCorrectEx( xloc, yloc, draw, 0)			
		Wend
	Else
		Rte.DeviceManager.Suspend
		gcal.setCalibrationTargetSize 2,9
		gcal.setCalibrationColors CColor(cal_foreground), CColor(cal_background)
		gcal.setCalibrationWindow -1
		tracker.doDriftCorrect xloc, yloc,draw,allow_setup
		Rte.DeviceManager.Resume
	End If

	gcal.enableKeyCollection False ' tell the com interface to stop collecting keyboard
	Set gcal = Nothing
	
	theHistory.RemoveAll

End Sub



'############################### end calibration/validation/drift correct ##############

~~~

---

以上。
