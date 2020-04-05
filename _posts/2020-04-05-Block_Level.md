---
title: "Block层"
categories:
  - Eyelink
tags:
  - Experiment Builder
  - CameraSetup
  - Sequence
toc: true
comments: true
author_profile: false
sidebar:
  title: "目录"
  nav: EB
---

---

>回顾实验设计
>>![eb_expdesign_block_level](/assets/images/eb_expdesign_block_level.png)
>
>根据实验设计，我们需要在`Block层`编辑`CameraSetup`和`Trial层级`两个部分。
>
>在`Experiment层`中双击编辑好的`Block`进入`Block层`内部开始编辑。
>>![eb_open_block_level](/assets/images/eb_open_block_level.gif)

---

# 1. CameraSetup

将`CameraSetup Action`从`控件库`中拖入工作区，链接箭头。
![eb_add_camerasetup](/assets/images/eb_add_camerasetup.gif)

## 1.1 设置校准点和背景的颜色

根据实验设计，需要将校准过程中的背景色设置为中性灰，校准点设置为黑色。首先我们查阅`CameraSetup`的属性，可以看到`ForegroundColor`和`BackgroundColor`两个属性。

![eb_camerasetup_properties_color](/assets/images/eb_camerasetup_properties_color.png)

当前的设置中，`ForegroundColor`为黑色，`BackgroundColor`为白色。也就是说，在`CameraSetup`环节中呈现校准点的时候，校准点的颜色是黑色，背景色为白色。如下图所示：

![drift_display_white_background](/assets/images/drift_display_white_background.png)

我们将`BackgroundColor`设置为中性灰`128, 128, 128, 255`。

![eb_set_camerasetup_bkgrdcolor](/assets/images/eb_set_camerasetup_bkgrdcolor.png)

## 1.2 设置校准类型

![eb_camerasetup_cal_type](/assets/images/eb_camerasetup_cal_type.png)

设置`Claibration Type`为`HV9`，即9点校准。

> H3 -> 水平三点校准 - 适用于单行文本阅读
> HV3 -> 三点校准
> HV5 -> 五点校准
> HV9 -> 九点校准
> HV13 -> 十三点校准 - 适用于遥测模式
>
> 随着校准点数量的增加，校准后的眼动仪精度越高，同时校准难度页会稍有增加。

---

# 2. Trial Sequence

将一个新的`Sequence`从`控件库`中拖入工作区，链接箭头，修改名字为“Trial”。

![eb_add_Trial_sequence](/assets/images/eb_add_Trial_sequence.gif)

---

至此完成`Block层`的编辑。

--
# 附：`CameraSetup Action`属性列表

一下属性是`CameraSetup Action`控件的全部属性，根据您实验的不同设置，显示出来的属性可能有所不同。

标记为`#`的属性为不可直接修改的属性。


| 属性 | 解释 |
| --- | :-- |
| Label | 当前Drift Correction控件的标签，默认为“DRIFT_CORRECTT”。 |
| Type# | 当前Drift Correction控件的类型。 |
| Node Path# | 当前Drift Correction控件在实验中的绝对路径。 |
| Message | 完成当前Drift Correction控件的执行后，向EDF文件发送的Message内容。 |
| Time# | 完成当前Drift Correction控件的执行后，被试机时钟的时间。 |
| NTP Time# | 完成当前Drift Correction控件的执行后，NTP时钟的时间。 |
| Start Time# | 开始执行当前Drift Correction控件时，被试机时钟的时间。 |
| Clear Input Queues | 如果为True，当此控件开始执行时，会刷新全部输入。 |
| Calibration Type | 校准类型（H3、HV3、HV5、HV9、HV13）。 |
| Horizontal Target Y Position | 在Calibration Type设置为H3时，会出现该属性。在此处设置三个水平角准点出现的Y方向位置。 |
| Pacing Interval | 自动校准延迟。 |
| Randomize Order | 校准点是否随机顺序出现。 |
| Repeat First Point | 是否重复第一个校准点。 |
| Force Manual Accept | 是否使用手动校准。 |
| Lock Eye After Calibration | 校准后锁定追踪的眼睛。 |
| Select Eye After Validation | 在校准后允许注视选择一个校准结果更好的眼睛进行追踪。 |
| Enable Customized Calibration Positions | 若选为True，则允许用户自定义Calibration过程中校准点出现的位置。 |
| Customized Calibration Positions | 设置Calibraiton自定义校准点的位置数组。 |
| Enable Customized Validation Positions | 若选为True，则允许用户自定义Validation过程中校准点出现的位置。 |
| Customized Validation Positions | 设置自定义Validation校准点的位置数组。 |
| Foreground Color | 校准点的颜色。 |
| Background Color | 屏幕的背景色。 |
| Use Animation Target | 是否使用一个动画作为校准点 |
| Animation Target | 校准动图的文件名，仅支持.avi格式的视频文件。 |
| Animation Play Count | 校准视频的播放次数，若为“-1”则无限循环播放。 |
| Apply Transparency | 是否允许无色部分透明。 |
| Use Custom Target | 是否使用自定义的图片作为校准点。 |
| Custom Target | 自定义校准点图片的文件名。 |
| Use Custom Background | 是否使用自定义的图片作为Camera Setup过程中的背景。 |
| Custom Background | 自定义背景图片的文件名。 |
| Target Outer Size | 默认校准点的尺寸，以像素为单位。 |
| Button State Callback Function | 对于启动了Custom Class的实验任务，可以通过Custom Class监测其他外部设备的按键状态。以达到自定义设备完成External Control的目的。 |
| Result# | 始终返回“None”。 |

---

以上。