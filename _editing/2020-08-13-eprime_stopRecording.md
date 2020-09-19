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
  nav: E-Prime
---

---

```VB
'The following is used to read the current display time. 'This can be used to determine the offset time relative to the start of the screen and thus'determine the exact time when the display is shown.'Write out a "pictureTrial_Onset" message with the determined offset value to mark the actual onset of the picture screen.''When reading the EDF time of the display onset, make sure that you subtract the offset value. For example,'MSG	5254942 pictureTrial_Start'MSG	5264986 10019 pictureTrial_Onset'MSG	5264986 pictureStart pictureOnset 21495  21520''In the above example, actual time stamp for the pictureTrial_Onset message should be 5254967 (=5264986 - 10019)'The EDF file time difference between the pictureTrial_Start and pictureTrial_Onset message is 25 (= 5254967 - 5254942),'which is consistent with the time difference reported by E-Prime (21520 - 21495)Dim timeEnd As LongtimeEnd = elutil.currentTime()Dim offset As Longoffset = (timeEnd - timeStart) - (pictureTrial.OnsetTime - pictureTrial.StartTime)tracker.sendMessage  offset & " pictureTrial_Onset"tracker.sendMessage "pictureStart pictureOnset " & pictureTrial.StartTime & "  " & pictureTrial.OnsetTime'Allow Windows to clean up while we record additional 100 msec of dataelutil.pumpDelay 100tracker.stopRecording  'The following code is for the EyeLink Data Viewer integration purpose.   'See section "Protocol for EyeLink Data to Viewer Integration" of the EyeLink Data Viewer User Manual'The IAREA message specifies the attributes of a rectangular interest area for the trial. 'Each trial can have a set of such rectangular interest areas. 'The example following draws a 100 * 100 pixels interest area in the center of the screentracker.sendMessage "!V IAREA RECTANGLE 1 " & Display.XRes/2 -50 & " " & Display.YRes/2 - 50 & " " & Display.XRes/2 + 50 & " " & Display.YRes/2 + 50 & " " &  TrialList.GetCurrentAttrib("imageName")'This TRIAL_VAR command specifies a trial variable and value for the given trial. 'Send one message for each pair of trial condition variable and its corresponding value.tracker.sendMessage "!V TRIAL_VAR trial " & TrialList.GetCurrentAttrib("trialid")tracker.sendMessage "!V TRIAL_VAR picture " & TrialList.GetCurrentAttrib("imageName")  'The TRIAL_RESULT message defines the end of a trial for the EyeLink Data Viewer. 'This is different than the end of recording message END that is logged when the trial recording ends. 'Data viewer will not parse any messages, events, or samples that exist in the data file after this message. tracker.sendMessage "TRIAL_RESULT 1" 
```