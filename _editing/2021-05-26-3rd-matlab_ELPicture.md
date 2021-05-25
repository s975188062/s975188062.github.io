---
title: "Matlab/PsychToolBox 眼动编程概述"
excerpt: "Matlab 几乎是每一个学习数据处理的人都会使用的编程语言。其自然语言的代码风格和简单的代码逻辑非常适合不需要考虑代码运行效率的场合。Psychtoolbox 作为一个基于 Matlab 的开源刺激程序工具包也可以说是从科研工作者的实际需求出发，让我们可以在一个软件中完成实验程序编写和数据处理的全部工作。"
read_time: false
header:
  overlay_image: /assets/images/3rd-ptb-overview.png
  overlay_filter: 0.5
categories:
  - Eyelink
tags:
  - Matlab
  - PsychToolBox
toc: true
comments: true
author_profile: false
sidebar:
  title: "目录"
  nav: EL_3rd
---

---

# 1. 安装 PsychToolBox

首先，了解任何软件的最好地方是它的官网：

[>>>PsychToolBox-3 官网<<<](http://psychtoolbox.org/)

我们可以在网站左侧的导航栏中轻易地发现 Download 链接，Charlie 也非常贴心地帮大家列在下面，各位可以直接点击跳转：

* [>>>PsychToolBox-3 Windows 安装教程<<<](http://psychtoolbox.org/download.html#Windows)
* [>>>PsychToolBox-3 MacOS 安装教程<<<](http://psychtoolbox.org/download.html#Mac)

官网中对于安装方法已经介绍的非常详细了，Charlie 在此不再赘述。

注意！一定要使用 PsychToolBox 官方的安装指导方法来下载 PsychToolBox，不能从其他电脑处直接复制过来。虽然直接复制 PsychToolBox 可能可以运行行为实验的函数，但是绝大多数情况下，Eyelink 相关的代码都无法正常工作。{: .notice--warning}

安装完成后，您可以在 Command Window 中直接运行 `Eyelink`，如果显示了下列内容，则说明 PsychToolBox 已经成功安装。

``` matlab
>> Eyelink

% This is the main function of the Eyelink toolbox
Usage:

% For general advice, try:
help Eyelink

% For a more detailed explanation of any Eyelink function, just add a question mark "?".
% E.g. for 'Initialize', try either of these equivalent forms:
Eyelink('Initialize?')
Eyelink Initialize?

% If you think you've found a bug, please report it
on the forum, see: http://psychtoolbox.org/


% Initialize or shutdown Eyelink connection:
[status =] Eyelink('Initialize' [, displayCallbackFunction])
[status =] Eyelink('InitializeDummy' [, displayCallbackFunction])
[status =] Eyelink('IsConnected')
[status =] Eyelink('SetAddress', ipaddress);
Eyelink('Shutdown')
oldlevel = Eyelink('Verbosity' [,level]);
Eyelink('TestSuite')
[status =] Eyelink('OpenFile', filename [, dontOpenExisting=0])
[status =] Eyelink('CloseFile')
[status =] Eyelink('ReceiveFile',['filename'], ['dest'], ['dest_is_path'])

% Calibration:
[result =] Eyelink('StartSetup' [, stype=0])
[status = ] Eyelink('DriftCorrStart', x, y [,dtype=0][, dodraw=1][, allow_setup=0])
[result = ] Eyelink('ApplyDriftCorr')
[result, tx, ty] = Eyelink('TargetCheck')
[result = ] Eyelink('AcceptTrigger')
[result, messageString =] Eyelink('CalMessage')

% Start or stop recording and acquiring data:
[startrecording_error =] Eyelink('StartRecording' [,file_samples, file_events, link_samples, link_events] )
Eyelink('Stoprecording')
error = Eyelink('CheckRecording')
eyeused = Eyelink('EyeAvailable')
NewOrOld = Eyelink('NewFloatSampleAvailable')
sample = Eyelink('NewestFloatSample')
[sample, raw] = Eyelink('NewestFloatSampleRaw' [, eye])
type = Eyelink('GetNextDataType')
item = Eyelink('GetFloatData', type)
[item, raw] = Eyelink('GetFloatDataRaw', type [, eye])
[samples, events, drained] = Eyelink('GetQueuedData'[, eye])

% Miscellaneous functions to communicate with Eyelink:
result = Eyelink('ButtonStates')
[status =] Eyelink('Command', 'formatstring', [...])
[status =] Eyelink('Message', 'formatstring', [...])
[result =] Eyelink('SendKeyButton', code, mods, state)
[time =] Eyelink('TrackerTime')
[offset =] Eyelink('TimeOffset')
[status =] Eyelink('RequestTime')
[time =] Eyelink('ReadTime')
[mode =] Eyelink('TrackerMode')
[result, reply =] Eyelink('ReadFromTracker', VariableName)

% Miscellaneous Eyelink functions:
[result =] Eyelink('WaitForModeReady', maxwait)
[result =] Eyelink('ImageModeDisplay')
mode = Eyelink('CurrentMode')
result = Eyelink('CalResult')
Eyelink('SetOfflineMode')
[version, versionString]  = Eyelink('GetTrackerVersion')
[time =] Eyelink('TrackerTime')
[offset =] Eyelink('TimeOffset')
[status] = Eyelink('ImageTransfer', imagePath [, xPosition=0][, yPosition=0][, width=0][, height=0][, trackerXPosition=0][, trackerYPosition=0][, xferoptions=0])

% Eyelink Velocity related functions:
[vel, acc, fsample]= Eyelink('CalculateOverallVelocityAndAcceleration' [, sample_model])
[vel,fsample] = Eyelink('CalculateVelocity' [,sample_model] )
[x_vel,y_vel,fsample] = Eyelink('CalculateVelocityXY' [,sample_model] )




% EyelinkToolbox version for the OpenGL PsychToolbox
% The EyelinkToolbox was developed by:

	Frans Cornelissen
	Enno Peters
	John Palmer
	Chris Burns
	Mario Kleiner
	Erik Flister
	Nuha Jabakhanji
```

显示出来的这些函数，即为 PsychToolBox 与 Eyelink 进行通信和控制的全部函数。

您大可不必在这里仔细研究每一个函数的具体参数和用法。在示例程序中边看注释边学习的效率要高很多。

---

# 2. 示例程序路径

安装好 PsychToolBox 的同时，你就已经拥有了 Eyelink 在 PsychToolBox 中的全部示例代码。其路径在 `\PsychHardware\EyelinkToolbox` 下：

![3rd-ptb-eyelink_tool_box_folder](/assets/images/3rd-ptb-eyelink_tool_box_folder.png)

其中文件和文件夹的内容如下：

```
EyelinkToolBox
├── EyelinkBasic                    # EyelinkToolBox 的基本函数
├── EyelinkDemos                    # EyelinkToolBox 示例程序
    ├── EyelinkShortDemos           # 精简示例（不推荐阅读）
    ├── GazeContingentDemos         # GC-Window 范式 示例程序
    ├── SR-ResearchDemo             # 标准示例程序（推荐阅读学习！）
        ├── AntiSaccade             # 反向眼跳范式 示例程序
        ├── change                  # 变化盲视范式 示例程序
        ├── ELCustomCalibration     # 自定义校准点 示例程序
        ├── EyelinkFixationWindow   # 注视触发窗口 示例程序
        ├── pursuit                 # 追随范式 示例程序
        └── EyelinkPicture.m        # 图片自由浏览 示例程序
    └── contents.m
├── EyelinkOnLiners                 # EyelinkToolBox 的基本函数
├── EyelinkTests                    # EyelinkToolBox 基本函数的不完全测试脚本
├── changes.m                       # 更新记录
├── compatibility.m                 # 兼容性声明
└── contents.m                      # EyelinkToolBox 内容物介绍
```

以上内容中，推荐有限阅读 `EyelinkPicture.m` 中的代码来作为入门学习。

后续的 PsychToolBox 教程中也将从 `EyelinkPicture.m` 开始。

---

以上。
