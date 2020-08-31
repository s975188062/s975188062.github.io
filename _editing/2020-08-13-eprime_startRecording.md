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

# 1. 讲解

~~~ vb
'perform drift correct with custom target'please notice that for 'doDriftCorrect(xloc As Integer, yloc As Integer, draw As Boolean, allow_setup As Boolean,Optional customDrift As Variant)'if  customDrift is provided, please set draw as False (without default drift target (a circle) drawn to the screen)'if  no customDrift is provided, please set draw as True (means: using the default target) doDriftCorrect Display.XRes/2,Display.YRes/2,False,True,Fixation'or using:'doDriftCorrect Display.XRes/2,Display.YRes/2,True,True  'Always send a TRIALID message before starting to record.'EyeLink Data Viewer defines the start of a trial by the TRIALID message.  'This message is different than the start of recording message START that is logged when the trial recording begins. 'The Data viewer will not parse any messages, events, or samples, that exist in the data file prior to this message.tracker.sendMessage "TRIALID " & TrialList.GetCurrentAttrib("trialid") 'This supplies the title at the bottom of the eyetracker displaytracker.sendCommand "record_status_message 'Picture " & TrialList.GetCurrentAttrib("imageName") & " Trial " & TrialList.GetCurrentAttrib("trialid") & "' "'Before recording, we place reference graphics on the EyeLink displaytracker.sendCommand "set_idle_mode"  'Must be offline to draw to EyeLink screen'The command "clear_screen" erases the tracker display to color 0 (black) tracker.sendCommand "clear_screen 0" 'The command "draw_box" draws a box in color 7 (medium gray).tracker.sendCommand "draw_box " & Display.XRes/2 -50 & " " & Display.YRes/2 - 50 & " " & Display.XRes/2 + 50 & " " & Display.YRes/2 + 50 & " 7"'The following code is for the EyeLink Data Viewer integration purpose.   'See section "Protocol for EyeLink Data to Viewer Integration" of the EyeLink Data Viewer User Manual'The IMGLOAD command is used to show an overlay image in Data Viewer tracker.sendMessage "!V IMGLOAD FILL " & TrialList.GetCurrentAttrib("imageName")'Send image to EyeLink screen: Image format can be 24bit bmp, jpg, or gif.tracker.BitmapToBackdrop TrialList.GetCurrentAttrib("imageName"), 4'Puts the tracker into the idle mode for a brief period before actually calling the startRecording function'Four arguments to startRecording() function set what data will be recorded to the EDF file and sent via the link.  'If an argument is 0, recording of the corresponding data is disabled tracker.sendCommand "set_idle_mode"Sleep 50 ' delay so tracker is ready tracker.startRecording True, True, False, False'Allowing 100 milliseconds of data to accumulate before the trial display startselutil.pumpDelay 100'The following is used to read the current time when the pictureTrial screen is processed'Write out a "pictureTrial_Start" message to mark this. Dim timeStart As LongtimeStart = elutil.currentTime()tracker.sendMessage  "pictureTrial_Start"
~~~

---

# 2. 源代码

~~~ vb
'perform drift correct with custom target'please notice that for 'doDriftCorrect(xloc As Integer, yloc As Integer, draw As Boolean, allow_setup As Boolean,Optional customDrift As Variant)'if  customDrift is provided, please set draw as False (without default drift target (a circle) drawn to the screen)'if  no customDrift is provided, please set draw as True (means: using the default target) doDriftCorrect Display.XRes/2,Display.YRes/2,False,True,Fixation'or using:'doDriftCorrect Display.XRes/2,Display.YRes/2,True,True  'Always send a TRIALID message before starting to record.'EyeLink Data Viewer defines the start of a trial by the TRIALID message.  'This message is different than the start of recording message START that is logged when the trial recording begins. 'The Data viewer will not parse any messages, events, or samples, that exist in the data file prior to this message.tracker.sendMessage "TRIALID " & TrialList.GetCurrentAttrib("trialid") 'This supplies the title at the bottom of the eyetracker displaytracker.sendCommand "record_status_message 'Picture " & TrialList.GetCurrentAttrib("imageName") & " Trial " & TrialList.GetCurrentAttrib("trialid") & "' "'Before recording, we place reference graphics on the EyeLink displaytracker.sendCommand "set_idle_mode"  'Must be offline to draw to EyeLink screen'The command "clear_screen" erases the tracker display to color 0 (black) tracker.sendCommand "clear_screen 0" 'The command "draw_box" draws a box in color 7 (medium gray).tracker.sendCommand "draw_box " & Display.XRes/2 -50 & " " & Display.YRes/2 - 50 & " " & Display.XRes/2 + 50 & " " & Display.YRes/2 + 50 & " 7"'The following code is for the EyeLink Data Viewer integration purpose.   'See section "Protocol for EyeLink Data to Viewer Integration" of the EyeLink Data Viewer User Manual'The IMGLOAD command is used to show an overlay image in Data Viewer tracker.sendMessage "!V IMGLOAD FILL " & TrialList.GetCurrentAttrib("imageName")'Send image to EyeLink screen: Image format can be 24bit bmp, jpg, or gif.tracker.BitmapToBackdrop TrialList.GetCurrentAttrib("imageName"), 4'Puts the tracker into the idle mode for a brief period before actually calling the startRecording function'Four arguments to startRecording() function set what data will be recorded to the EDF file and sent via the link.  'If an argument is 0, recording of the corresponding data is disabled tracker.sendCommand "set_idle_mode"Sleep 50 ' delay so tracker is ready tracker.startRecording True, True, False, False'Allowing 100 milliseconds of data to accumulate before the trial display startselutil.pumpDelay 100'The following is used to read the current time when the pictureTrial screen is processed'Write out a "pictureTrial_Start" message to mark this. Dim timeStart As LongtimeStart = elutil.currentTime()tracker.sendMessage  "pictureTrial_Start"
~~~

---

以上。