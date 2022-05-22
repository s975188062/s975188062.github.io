---
title: "User Script(E-Prime with DevKit v2.1.1)"
excerpt: "放到E-Prime程序的最前面，不需要理解，完全复制粘贴。"
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

# 1. 简介

在 User Script 中，定义了整个实验程序需要用到的函数和变量。如要理解里面的全部代码，需要较多的编程基础，但好在我们基本不需要理解这些代码。

在所有的实验中，使用的这些定义代码都是相通的，所以我们只需要将其完整地复制到自己的程序中即可。

E-Prime 2 中 User Script 的位置如下：

![eprime-ep2_user_script_v2](/assets/images/eprime-ep2_user_script_v2.png)

E-Prime 3 中 User Script 的位置如下：

![eprime-ep3_user_script_v2](/assets/images/eprime-ep3_user_script_v2.png)

---

# 2. 源代码

~~~ vb
'
' Copyright (c) 1997 - 2021 by SR Research Ltd., All Rights Reserved        
'                                                                           
' This software is provided as is without warranty of any kind.  The entire 
' risk as to the results and performance of this software is assumed by the 
' user. SR Research Ltd. disclaims all warranties, either express or implied,
' including but not limited to, the implied warranties of merchantability,    
' fitness for a particular purpose, title and noninfringement, with respect 
' to this software.
'
' For non-commercial use by Eyelink licencees only
'
'-------------------------------------------------------------------------------
'
' The following routines provide drawing/input support for calibration,
'validation, drift check, and camera setup features.

'If using E-Prime 3, please be sure to use at a minimum EP3's update 2 (version
'3.0.3.80) for proper function.
'
'
'-------------------------------------------------------------------------------
'
Dim elCameraSetupType As Integer 'see elConnect script
Dim useSuspend As Integer	 'controls use of suspend/resume calls.
							 'See elConnect script.
Dim usePriority As Integer	 'controls use of thread priority calls.
							 'See elConnect script.
Dim nPriority As Integer	 'Holds elevated priority value if usePriority > 0.
Dim tracker As Object        'global reference to hold tracker object
Dim elutil As Object         'global reference to hold elutil object
Dim cal_background As String 'calibration background color
Dim cal_foreground As String 'calibration foreground
Dim cal_target_size As Integer 'calibration target size, see elConnect
Dim cal_pen_width As Integer   'calibration pen width, see elConnect
Dim use_cust_cal As Integer  'whether to use custom calibration stimuli
Dim cust_cal_stim As String	 'custom calibration image/video to use
Dim calibrationSoundsOn As Integer 'whether or not to enable calibration sounds
Dim dummyMode As Integer     'whether to use active or dummy tracker connection
Dim incount As Integer       'global integer to hold input count
Dim edfFileName As String	 'global variable for edfname
Dim edfShortName As String	 'global var for edfname sans .edf
Dim trialCounter As Integer	 'global var for sequential trial number

'-----EDF filename input--------------------------------------------------------

'This function requests a name for the EyeLink Data File (.edf) to which the
'gaze and trial data will be recorded.  The filename must adhere to the DOS
'naming convention of 8.3 (8 characters plus extension).  The function will
'automatically append the .edf extension, so you can use at most 8 characters
'for the name.  Special characters other than underscore are not allowed.  The
'function checks the validity of the requested .edf name and will bring up a
'dialog box if an invalid name is given.  If the chosen .edf name already exists
'on the Display PC for this project, a warning will be presented asking if you'd
'like to overwrite the existing file or choose a new name.

Sub getEDFName		'Request .edf name & check whether .edf name is acceptable
	Dim edfcheck As Integer 'set to 1 if name is okay
	Dim nmAscChar As String 'holds ASCII code of each character in proposed name
	Dim nmIter As Integer 'used to iterate through and check each character
	Dim spChars As Integer 'number of special characters found in proposed name

	Do While edfcheck = 0 'Keep askbox up until we have a proper .edf name
		edfcheck = 0
		spChars = 0 'reset number of special characters found for new attempts
		edfFileName = Display.AskBox("Please enter an EDF file name\n(up to " &_
		"8 alphanumeric characters)" , "Test")
		
		If len(edfFileName) = 0 Then
			edfFileName = "Test.edf" 'if nothing entered, use Test.edf
		End If
		If UCase(Right(edfFileName, 4)) <> ".EDF" Then 'if entered name doesn't
			edfFileName = edfFileName & ".edf"  'end in .edf, append with .edf
		End If
		
		edfShortName = left(edfFileName,len(edfFileName)-4) 'separate variable
				'with the proposed name without ".edf" -- done so we can check
				'the name and not have the . trip the check.
	
		For nmIter = len(edfShortName) To 1 Step -1 'iterate through once per
													'character in edfShortName
			nmAscChar = Asc(UCase(Mid(edfShortName,nmIter, 1))) 'set to ASCII
					'value of character.  If a text character, set to ASCII
					'value of uppercase version.
			If (nmAscChar > 64 And nmAscChar < 91) Or (nmAscChar > 47 And _
				nmAscChar < 58) Or (nmAscChar = 95) = True Then
					'if character checked is standard letter/number, or
					'underscore, do nothing
				Else 'otherwise, add 1 to spChars so we can reject the name.
					spChars=spChars+1
			End If
		Next nmIter ' loop to check next character
	
		If spChars = 0 And len(edfFileName) <=12 Then 'If there are no special
			edfcheck = 1		'characters and the name length is okay, accept
		End If 					'the name and continue with the project.
		If spChars > 0 Then	'if there are special characters, reject name and
			edfcheck = 0	'pull up dialog box before trying again.
			Display.MsgBox "\nName contains special characters.\nTry again " &_
			"with only letters, numbers, and/or underscores.\n"
		End If
		If len(edfFileName) > 12 Then	'if name is too long, reject it -- pull
			edfcheck = 0				'up dialog box before trying again.
			Display.MsgBox "\nName is too long.\nUp to 8 alphanumeric " &_
			"characters (including underscores) can be used.\n"
		End If
		If edfcheck = 1 And fileexists("./Results/" & edfShortName & "/" _
		& edfFileName) Then	'if requested file already in project's save folder,
			Dim answer As Integer	'ask if user wants to overwrite or rename.
			answer = Display.AnswerBox("An .edf of the same name already " &_
			"exists.  Overwrite?", "Overwrite", "Choose New Name")
			If answer = 2 Then
				edfcheck=0
			End If
		End If
	Loop
	On Error Resume Next 'ignore errors -- without this, if the Results folder
	'being generated below already exists, the project will error out.
	
	mkdir "Results"	'setting up the .edf save directory (within the project
	chdir "Results"	'directory, we'll have ./Results/<filename>/<filename.edf>
	mkdir edfShortName
	chdir ".."
End Sub

'Sub setMouseState
'Input: vis (True or False)
'Output:None
'Calibration graphics handler: Both GDICal and busyCal
'Purpose: Set visibility of mouse cursor to visible (True) or not (False).
' E-Prime does not automatically update mouse cursor visibility on-screen until
' the cursor object updates, so this sub simply forces an update by setting the
' mouse cursor position to its current location.

Sub setMouseState(vis As Boolean)
	Dim p As Point
	Mouse.GetCursorPos p.x, p.y
	If vis = True Then
		Mouse.ShowCursor True
	Else
		Mouse.ShowCursor False
	End If
	Mouse.SetCursorPos p.x, p.y
End Sub

'################### start calibration/validation/drift check items ############

'
'Sub checkKeyInput
'Input: None
'Output:None
'Calibration graphics handler: busyCal only
'Purpose: Check for and forward any input keypresses to the tracker.  Only
'needed while in Calibration/Validation/Drift Check modes.
'
Sub checkKeyInput() 
	'Retrieves the responses stored in the KeyboardDevice's History property.
	Dim kb_count As Integer
	Dim kb_resp As String

	kb_count = Keyboard.History.Count
	If (kb_count = 0) Then Exit Sub
	kb_resp = Keyboard.History(kb_count).RESP
	If (Len(kb_resp) = 1) Then  ' normal character
		tracker.sendKeybutton Asc(kb_resp), 0, 0
	Else  ' special character
		Select Case kb_resp
		Case "{SPACE}"
			tracker.sendKeybutton Asc(" "), 0, 0
		Case "{BACKSPACE}"
			tracker.sendKeybutton 08, 0, 0
		Case "{ENTER}"
			tracker.sendKeybutton 13, 0, 0
		Case "{ESCAPE}"
			tracker.sendKeybutton 27, 0, 0
		Case Else
			Debug.Print "ignoring kbd: " & kb_resp
		End Select
	End If
	Keyboard.History.RemoveAll
End Sub


'
'Sub doTrackerDrawings
'Input: 
'	bcal - reference to BusyCal object
'	customDrift - Optional argument, if custom drift check target needed.
'Output: None
'Calibration graphics handler: busyCal only
'Purpose: In a busy loop, check the job state of bcal, and draw target, 
'play target beeps accordingly.
'
Sub doTrackerDrawings(bcal As Object, Optional customDrift As Variant)
	Dim cnvs As Canvas
	Dim ofillColor As String 'original fill color
	Dim oPenColor As String 'original pen color
	Dim oPenWidth As Integer 'original pen Width
	Dim ELSoundBufferInfo As SoundBufferInfo
	Dim ELSoundBuffer As SoundBuffer

	Dim need_looping As Integer 'used for custom calibration video
	need_looping = 0
	
	Set cnvs = Display.Canvas
	If use_cust_cal=1 Then
		Dim calImg As ImageDisplay
		Set calImg = New ImageDisplay
		Set calImg.Filename = cust_cal_stim
		Set calImg.BackColor = CColor(cal_background)
	End If

	If use_cust_cal=2 Then
		Dim calMov1 As MovieDisplay
		Set calMov1 = New MovieDisplay
		Set calMov1.Filename = cust_cal_stim
		Set calMov1.BackColor = CColor(cal_background)
	End If

	ELSoundBufferInfo.MaxLength = 5000
	Set ELSoundBuffer = Sound.CreateBuffer(ELSoundBufferInfo)

	If usePriority > 0 Then
		nPriority = GetOSThreadPriority()

		'Temporarily set the thread priority to a normal application
		SetOSThreadPriority 3
	End If

	Do While Not bcal Is Nothing 
	    Dim job As Integer
		Dim  connected As Boolean
		connected = tracker.isConnected()
	    job = bcal.job
	    If job = -1 Or Not connected Then ' Exit
	        Set bcal = Nothing
	    Else
			Select Case job
			Case 0
				'do nothing
			Case 1 'Setup Cal Display
				'save color and pen info so that we can reset when we return.
				oPenWidth= cnvs.PenWidth
				oPenColor= cnvs.PenColor
				ofillColor = cnvs.FillColor
				
				'set the pen and color
				cnvs.PenWidth = cal_pen_width
				cnvs.PenColor = CColor(cal_foreground)
				cnvs.FillColor = CColor(cal_background)
			Case 2 ' Exit Cal Display
				'Reset anything that was changed in Setup Cal Display
				cnvs.PenWidth=oPenWidth
				cnvs.PenColor=oPenColor
				cnvs.FillColor=ofillColor
				If Not IsMissing(customDrift) Then
					Dim cd As TextDisplay
					Set cd = customDrift
					cd.draw
					Set cd = Nothing
 				End If
			Case 5, 6 'Clear Cal Display / Erase Cal Target
				If use_cust_cal = 2 Then
					need_looping = 0
					calMov1.Stop
					cnvs.clear
				Else
					cnvs.Clear
				End If
			Case 7, 8, 14, 15, 18, 19
			'Cal Target Beep or DC Target Beep or cal done beep or dc done beep 
				'Enable the following block if you need audio feedback
				If calibrationSoundsOn = 1 Then
					ELSoundBuffer.Filename = Switch( _
						(job <= 8), "Stimuli/EL_type.wav", _
						(job <= 15), "Stimuli/EL_beep.wav", _
						(job <= 19), "Stimuli/EL_error.wav" )
					ELSoundBuffer.Load
					ELSoundBuffer.Play
				End If
	        Case 9 ' Draw Cal Target
	            Dim x As Integer
	            Dim y As Integer
				If use_cust_cal = 1 Then
					cnvs.clear
					bcal.getCalLocation x, y
					Set calImg.X = x
					Set calImg.Y = Y
					calImg.Load
					calImg.Draw
				ElseIf use_cust_cal = 2 Then
					need_looping = 1
					cnvs.clear
					bcal.getCalLocation x, y
					Set calMov1.X = x
					Set calMov1.Y = y
					calMov1.Load
					calMov1.Play
				Else
		            bcal.getCalLocation x, y
					cnvs.Clear
					cnvs.Circle x, y, cal_target_size
				End If
			Case 10 To 13
				Debug.print "Camera Image Not available"
			Case Else
				Debug.print "Unhandled job " & job
	        End Select
	    End If
		checkKeyInput
		If need_looping = 1 Then
			If calMov1.Status = ebMovieStatusStopped Then
				calMov1.load
				calMov1.play
			End If
		End If
	Loop
	Set bcal = Nothing
	Set ELSoundBuffer = Nothing
	Set cnvs = Nothing
	If usePriority > 0 Then
		'Reset the thread priority
		SetOSThreadPriority nPriority
	End If
End Sub

'
'Sub doCameraSetup
'Input: None
'Output:None
'Calibration graphics handler: Both GDICal and busyCal
'Purpose: Call this subroutine to do camera setup. 
'		  busyCal - Setup busycal and calls doTrackerDrawings to perform busyCal
'		 	output.
'		  GDICal - Setup GDICal and calls doTrackerSetup to perform camera setup
'
'Note: Camera image transfer to the Display PC is not available under busyCal.
'
'
Sub doCameraSetup
	setMouseState False
	If tracker.isConnected <> -1 Then 'If not in dummy mode
		If elCameraSetupType = 1 Then 'If using GDICal
			Dim gcal As Object
			Dim theHistory As RteCollection
			Set theHistory = Keyboard.History ' ignore keys pressed while in
											  '   calibration.
			theHistory.RemoveAll

			If useSuspend = 1 Then
				Rte.DeviceManager.Suspend 'This code may switch the resolution
									  	  'back to the previous resolution
			End If

			Set gcal = elutil.getGDICal()

			'The following turns off calibration sounds.
			If calibrationSoundsOn = 0 Then
				gcal.setCalibrationBeepSound "off",0
			End If
	
			gcal.enableKeyCollection True 'tell the com interface to start
										  '  collecting keyboard
			gcal.setCalibrationTargetSize cal_pen_width,cal_target_size
			gcal.setCalibrationColors CColor(cal_foreground), _
			 CColor(cal_background)

			'The following indicates what stimuli to use for calibration,
			'if custom stimuli have been defined in elConnect.
			If use_cust_cal = 2 Then
				gcal.initAnimationCalibration cust_cal_stim, 0
			ElseIf use_cust_cal = 1 Then
				gcal.setCalibrationTarget cust_cal_stim
			End If

			gcal.setCalibrationWindow -1
			tracker.doTrackerSetup

			If useSuspend = 1 Then
				Rte.DeviceManager.Resume
			End If
			
			gcal.enableKeyCollection False ' tell the com interface to stop
			Set gcal = Nothing			   ' collecting keyboard.
		
			theHistory.RemoveAll
		Else 'If using busyCal
			If tracker.isConnected <> -1 Then 'skip if we're in dummy mode
				Dim bcal As Object
				Set bcal = elutil.getBusyCal()
				bcal.startCameraSetup
				doTrackerDrawings bcal
	
				Set bcal = Nothing
			End If
		End If
	End If
End Sub

'
'Sub doDriftCheck
'Input: 
'	xloc - xlocation of the drift check target
'	yloc - ylocation of the drift check target
'   draw - if false, no target is drawn.  If false, can optionally pass in
'           customDrift TextDisplay, so that custom target can be re-drawn if
'           drift check is cancelled and calibration is performed.
'	allow_setup - if false, pressing escape does not perform a calibration.
'	customDrift - optional argument (TextDisplay type) to re-draw custom target.
'Output:None
'Calibration graphics handler: Both GDICal and busyCal
'Purpose: Call this subroutine to do drift check.
'		  busyCal - Setup busycal and call doTrackerDrawings to perform busyCal
'		    output.
'		  GDICal - Setup GDICal and calls doDriftCorrectEX to perform drift
'		    check.

'Note: Camera image transfer to the Display PC is not available under busyCal.
'
'
Sub doDriftCheck(xloc As Integer, yloc As Integer, draw As Boolean, _
allow_setup As Boolean,Optional customDrift As Variant)
	Dim theHistory As RteCollection
	Set theHistory = Keyboard.History
	theHistory.RemoveAll ' remove all key presses prior to drift check

	If elCameraSetupType = 1 Then 'If using GDICal
		Dim gcal As Object

		Dim cd As TextDisplay
	
		Set gcal = elutil.getGDICal()

		'The following indicates what stimuli to use for calibration, if custom
		'stimuli have been defined in elConnect.
		If use_cust_cal = 2 Then
			gcal.initAnimationCalibration cust_cal_stim, 0
		ElseIf use_cust_cal = 1 Then
			gcal.setCalibrationTarget cust_cal_stim
		End If

		'The following turns off drift check sounds.
		If calibrationSoundsOn = 0 Then
			gcal.setCalibrationBeepSound "off",0
		End If

		gcal.enableKeyCollection True 'tell the com interface to start
									  '  collecting keyboard
			
		' Call do drift check.
		' If the return value is 27 and allow setup value is 1 then 
		' switch to camera setup.
		' Once the camera setup is done, return to this drift check.
	
		If Not isMissing(customDrift)  Then
			Set cd = customDrift
			cd.draw
				
			Dim ret As Integer
			ret = tracker.doDriftCorrectEx( xloc, yloc, 0, 0)			
	
			Do While ret=27 And allow_setup = True 		
				If useSuspend = 1 Then
					Rte.DeviceManager.Suspend  'This code may switch the
							   '  resolution back to the previous resolution
				End If
		
				gcal.enableKeyCollection True 'tell the com interface to start
											  '  collecting keyboard
				gcal.setCalibrationTargetSize cal_pen_width,cal_target_size
				gcal.setCalibrationColors CColor(cal_foreground), _
					CColor(cal_background) 'cd.BackColor				

				gcal.setCalibrationWindow -1
				tracker.doTrackerSetup
				gcal.setCalibrationWindow 0
				If useSuspend = 1 Then
					Rte.DeviceManager.Resume
				End If
			
				cd.draw
				ret = tracker.doDriftCorrectEx( xloc, yloc, draw, 0)			
			Loop
		Else
			If useSuspend = 1 Then
				Rte.DeviceManager.Suspend
			End If
			gcal.setCalibrationTargetSize cal_pen_width,cal_target_size
			gcal.setCalibrationColors CColor(cal_foreground), _
				CColor(cal_background)
			gcal.setCalibrationWindow -1
			tracker.doDriftCorrect xloc, yloc,draw,allow_setup
			If useSuspend = 1 Then
				Rte.DeviceManager.Resume
			End If
		End If
	
		gcal.enableKeyCollection False ' tell the com interface to stop
									   '   collecting keyboard
		Set gcal = Nothing
	Else 'If using busyCal
		Dim bcal As Object
		Set bcal = elutil.getBusyCal()
		bcal.startDriftCorrect xloc,yloc,draw,allow_setup
		doTrackerDrawings bcal,customDrift
		Set bcal = Nothing
	End If

	theHistory.RemoveAll ' remove keys pressed during drift check
End Sub

'######################## end calibration/validation/drift check ###############

'Below function is to be called if we require keyboard input during an inline
'script's execution.

Function GetLastResponseData(strResponse As String) As ResponseData

	'Clone input history collection so the script can enumerate through the
	'responses without the collection changing while the script is running
	'(e.g., if the subject presses another key during this time).
	Dim theHistoryCollection As RteCollection
	Set theHistoryCollection = Keyboard.History.Clone

	'Enumerate through the collection in reverse order to determine
	'the last state of the specified key.
	Dim theResponseData As ResponseData

	If theHistoryCollection.Count > incount And theHistoryCollection.Count > 0 _
	Then
		incount = theHistoryCollection.Count 'so we don't get multiple keys
		Set theResponseData = CResponseData(theHistoryCollection(1))
		If Not theResponseData Is Nothing Then
'			'Check if press matches what we're looking for.
			If UCase(strResponse) = UCase(theResponseData.Resp) Then
				Set GetLastResponseData = theResponseData
				Exit Function
			End If
		End If
	End If

	'If the script gets to this point, the key is assumed to have never been
	'pressed (assumed release).
	Set GetLastResponseData = Nothing
	Keyboard.History.RemoveAll
	incount = 0

End Function

'-------------
~~~

---

以上。