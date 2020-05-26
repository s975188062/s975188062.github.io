---
title: "眼动仪数据采集操作 - Eyelink 1000Plus"
excerpt: "可以比较极端地说，数据质量的好坏，几乎完全取决于主试者在数据采集过程中的操作。本章以Eyelink 1000Plus系列眼动仪为例，讲解数据采集过程中规范操作和执行标准。"
read_time: false
header:
  overlay_image: 
  overlay_filter: 0.5
  teaser: 
categories:
  - Eyelink
tags:
  - Host PC
  - 数据采集
  - 校准和验证
toc: true
comments: true
author_profile: false
sidebar:
  title: "目录"
  nav: host
---

---

本章中除设备操作外，都是个人偏好的流程习惯，在此仅将其分享给大家，并不作为教程或者执行标准。
{: .notice--info}

---

# 0. 准备实验话术和SOP

> 许多刚刚接触实验的同学，可能不会很在意实验前的准备工作，因为他们不知道差微的语言和行为暗示都会对实验结果造成影响。

实验的核心是控制变量，要求除了因变量之外的所有其他条件必须完全一致。不同人对于这种一致的水平是有不同理解的，初出茅庐的研究者一般会认为只要设计实验程序时控制变量就可以了，富有经验的研究者则认为自己在每个被试进行实验时的表现必须完全一致。这种苛刻的一致甚至涉及到如何引导被试进入房间坐下，如何介绍实验，出了问题的时候说什么话等等……

如果您和我一样吹毛求疵的话，请和您的朋友多多演练，固定流程。

这些近乎变态的准备最终都会变成一篇优秀论文的细节。

---

# 1. 打开实验程序，等待被试

在被试进入到实验室之前，打开实验程序。让实验停留在指导语阶段，等待被试阅读。

准备好《知情同意书》，在被试到达后阅读并签字。

---

# 2. 调整被试座椅高度

随后引导被试坐下，调整座椅高度，让被试的水平视线对应到屏幕的上1/4范围内。如下图所示：

![host_adjust_chair_height](/assets/images/host_adjust_chair_height.png)

---

# 3. 交代实验内容，被试阅读指导语

随后让被试阅读指导语，让被试根据自行判断结束指导语的浏览。如果被试在阅读过程中进行了提问，则进行最低限度的回答。如果被试问了一些没有准备的问题，可以选择不回答。

---

# 4. Camera Setup

⚠️注意⚠️：如果是`Head-Stablized`模式，则从此步骤开始就要告诉被试“尽量不要移动头部”。如果被试不小心移动了头部，则要求被试告知你他移动了头部。
{: .notice--info}

需要注意的是，整个Camera Setup环节的目的是：让眼动仪可以**持续**、**稳定**地追踪到眼睛。

继续实验到`CameraSetup`环节，此时屏幕是空的，不会显示任何内容。

## 4.1 找眼镜

在主试机或者被试机键盘上按`Enter`，此时被试机屏幕上会出现如下图所示的图像：

![host_display_PC_show_zoom_view](/assets/images/host_display_PC_show_zoom_view.png)

此时在被试机显示器上显示的是`Zoom View`。对应主试机`CameraSetup`界面下部眼睛的小窗口。

![host_pc_camera_setup_screen_show_zoom_view](/assets/images/host_pc_camera_setup_screen_show_zoom_view.png)

我们可以注意到左侧的小窗口中提示“Left Eye Image Not Available”。这是因为我们目前只设置了追中右眼。

**如何在选择追踪哪一只眼睛呢？** 大多数研究中，研究者选择统一追踪右眼。我们已知双眼的运动是共轭的，两只眼睛的注视位置总会非常接近。因此除一些双眼信息对抗的范式外，没有采集双眼的必要。而我们只要左右眼是分化出一只优势眼和一只非优势眼的，理论上来讲采集优势眼的数据会更为准确。但是实际操作层面来看，优势眼的判定本身具有很多争议，且右眼为优势眼的人多于左眼为优势眼的人。综上所述，建议统一采集右眼即可，仅供参考。
{: .notice--info}

在主试机上我们将相机的整个视野称为`Global View`，在`Global View`中可以看到小绿框，小绿框中的图像会显示到下方的`Zoom View`，即眼睛的部分。

![host_PC_zoom_view_in_golbal_view](/assets/images/host_PC_zoom_view_in_golbal_view.png)

在被试机上按`Enter`显示`Zoom View`后，可以继续按⬅️或➡️在`Global View`和`Zoom View`间随意切换。

首先，让我们从`Global View`开始。

![host_display_show_globalview](/assets/images/host_display_show_globalview.png)

如上图所示，在`Global View`下，我们可以看到相机的整个视野。此时，我们需要将眼睛放到相机视野的中央。或者您也可以这样理解，我们要**调整相机的角度，将相机视野的中央对准要追踪的眼睛**。

上图所示的情况是非常理想的，被试的眼睛已经在相机视野的中央了。如果实际情况不尽理想，则首先旋松下图所示的球形关节，随后按住相机底座，调整上半部分相机和发光器的采集角度。注意调整过程中不要遮挡相机和发光器。

![host_adjust_tracker_angle](/assets/images/host_adjust_tracker_angle.png)

角度调整完成后，重新拧紧固定球形关节即可。

如果在整个过程中，相机的视野非常模糊，无法看清任何东西，那么恭喜你，上一个做实验的同学调了个皮。他把镜头胡乱拧了一顿，导致镜头失焦。不过不需要担心，您只需调整一下镜头，即可让眼动仪看清，此处仅粗略看清即可，这个阶段的目的是让被试的眼睛落在相机视野的中央。

![host_how_to_adjust_focus](/assets/images/host_how_to_adjust_focus.png)

调焦过程中您可以看到效果如下：

![host_adjust_focurs_result](/assets/images/host_adjust_focurs_result.gif)

将眼睛调整到相机视野中央后，我们来选择要追踪的眼睛。我们需要到主试机这边来执行这部分操作。

![host_select_eye_to_track](/assets/images/host_select_eye_to_track.png)

选定左/右眼后，还需要在`Global View`的窗口中点击一下对应的眼睛，确保`Search Limit`的红圈完全包括眼睛。



当前眼动仪的工作模式为`Monocular`，因此我们现在只能追踪一只眼睛。如需追踪双眼，则需要在`SetOption`中更改为`Binocular`即可。
{: .notice--info}

## 4.2 调焦

## 4.3 调节阈值

## 4.4 执行4-Point Check

## 4.5 执行Calibrate

## 4.6 执行Validate

# 5. 开始实验

# 6. 结束实验
