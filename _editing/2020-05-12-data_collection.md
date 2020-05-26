---
title: "眼动仪数据采集操作 - Eyelink 1000Plus"
excerpt: "可以比较极端地说，数据质量的好坏，几乎完全取决于主试者在数据采集过程中的操作。本章以Eyelink 1000Plus系列眼动仪为例，讲解数据采集过程中规范操作和执行标准。"
read_time: false
header:
  overlay_image: /assets/images/host_data_collect_header.png
  overlay_filter: 0.5
  actions:
    - label: "B站视频教程"
      url: "https://www.bilibili.com/video/BV1rE411w7qv?p=4"
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

选定左/右眼后，还需要在`Global View`的窗口中点击一下对应的眼睛，确保`Search Limit`的红圈在想要追踪的眼睛上。

![host_put_search_limit_in_track_eye](/assets/images/host_put_search_limit_in_track_eye.gif)

当前眼动仪的工作模式为`Monocular`，因此我们现在只能追踪一只眼睛。如需追踪双眼，则需要在`SetOption`中更改为`Binocular`即可。
{: .notice--info}

## 4.2 调焦

在讲解调焦之前，我们先要认识眼睛上的两个特殊的部分——`Pupil`和`CR`。

Eyelink 1000Plus系列是一个使用暗瞳技术的红外眼动仪。依靠Pupil和CR来识别眼动，在此章最开始说的持续稳定追踪眼睛，实际上是指持续稳定地追踪Pupil和CR。

在眼动仪的相机视野中采集到的视频图像均为红外图像，我们可以理解为类似老实的黑白电视的画面。

![host_tracker_grey_image](/assets/images/host_tracker_grey_image.png)

在这幅黑白画面中，我们将所有的像素依据黑白灰度划分为0-255共计256个亮度级。0是纯黑最暗，255是纯白最亮。

当眼动仪发射的红外光照射到面部时，发生漫反射，所有的光随机地向四周发散反射。但是这其中，有两个部位是例外的。

>**瞳孔/Pupil**
>
>我们都曾有过这样的经历，在夏日艳阳高照的时候，站在室外向屋内望去，之间黑漆漆一片，看不清屋里发生了什么。这是因为阳光从窗户照进室内后，在室内多次反射衰减，很少有阳光可以从窗户反射出来。我们的眼睛大概就是这样的结构。
>
>![host_eye_architecture](/assets/images/host_eye_architecture.png)
>
>红外光从瞳孔射入眼球后，在玻璃体中反射衰减，几乎不会反射出来。因此在眼周范围内，瞳孔反射的红外光是最少的，也就是最暗的。
{: .notice--info}

>**角膜反射点/Corneal-Reflection/CR**
>
>与瞳孔相反的是，`CR`则是一个镜反射点。
>
>我们的眼球平时是被泪液所包裹的，吸附在眼球表面的泪液将眼球变成了一个表面光滑的球。当红外光照射到液体表面时，则发生反射率很高的镜面反射。
>
>在眼睛周围，没有任何一个地方的反射率可以和`CR`相媲美。`CR`也因此就成为了眼睛周围最亮的区域。
{: .notice--info}

在调焦这一步，我们希望眼动仪的相机可以看的尽量清楚，即将相机的焦对到眼睛上。

我们以眼睛上的`CR`为标准来进行调校，轻微旋动镜头，`CR`的大小会发生变化。我们要找到CR最小的时刻，此时即完成了调焦。

![host_adjust_forcus_min_CR](/assets/images/host_adjust_forcus_min_CR.png)

## 4.3 调节阈值

前面调焦的部分已经介绍了相机采集到的灰度图像，这一部分我们首先来介绍一下相机如何从从这幅灰度图里面找到`Pupil`和`CR`。眼动仪使用的是一个叫做“阈值分割”的算法。

我们已知在眼睛周围，瞳孔是最暗的，亮度最低的部分。而CR则是最亮的，亮度最高的。我们是否可以设定一高一低的两个值，将最暗和最亮的两部分从眼睛上找出来。

以瞳孔为例，已知瞳孔是图像上最暗的部分，那我设定一个值，将图像上亮度比这个值还低的部分找出来。如果这个值设定得当的话，我是可以准确提取出瞳孔的。

反过来我也可以设置一个比较高的值，将图像上比这个值还亮的部分提取出来，即CR。

那么用来识别瞳孔和CR的两个值就称为`Pupil的阈值`和`CR的阈值`。

在主试机`Camera Setup`界面的左上角是用来调整阈值的。您可以分别调整Pupil和CR的阈值，或者大多数情况下点击`Auto Threshold`即可。

**`Auto Threshold`是自动调节阈值，适用于绝大多数的情况。**

![host_show_auto_threshold](/assets/images/host_show_auto_threshold.png)

细心的同学的同学一定已经注意到了上图相机视野中的蓝色区域。实际上系统会对图像进行染色，将满足瞳孔识别条件的像素染为深蓝色，将符合CR识别条件的像素染为浅蓝色。除了眼睛周围也有大片区域被染成上述两种颜色，但是这些都不会影响相机对于眼睛的识别，因为眼动仪只会在`Search Limit`的红圈搜索眼睛。

这种染色在我们判断阈值设置时候恰当时非常重要。

如下图所示是瞳孔阈值过低、刚好和过高的三种情况：

![host_pupil_threshold_low_to_high](/assets/images/host_pupil_threshold_low_to_high.png)

如下图所示是CR阈值过高、刚好和过低的三种情况：

![host_cr_threshold_low_to_high](/assets/images/host_cr_threshold_low_to_high.png)

综上，我们要将Pupil和CR的阈值调节到刚好将Pupil/CR全部染色，又不将其他部分染色。当然大部分时候都不需要我们手动调节，`Auto Threshold`就够用了。

Pupil的阈值一般在60-80之间，CR的阈值一般在200-220之间，具体的数值和实验室环境相关。

**小结**：按“A”自动调节阈值，如果有问题再手调。
{: .notice--info}

## 4.4 执行4-Point Check

完成阈值调解后，我们来对被试下一串简单的指令，同时监控CR和Pupil的识别情况。

* “请看屏幕的左上角……“ -> 观察CR和Pupil的识别情况
* ”请看屏幕的左下角……“ -> 观察CR和Pupil的识别情况
* ”请看屏幕的右上角……“ -> 观察CR和Pupil的识别情况
* ”请看屏幕的右下角……“ -> 观察CR和Pupil的识别情况

每次下指令后，都停顿一下，观察Pupil和CR的识别是否出现下图所示异常：

![host_4_poing_check_error](/assets/images/host_4_poing_check_error.png)

如出现异常，则参照[实验室环境设置](/eyelink/LabSetup/)重新设置眼动仪设备摆放。

## 4.5 执行Calibrate

点击`Calibrate`或在键盘上按`C`进入到Calibrate界面。

<figure class="half">
    <a href="/assets/images/host_claibrate_screen.png"><img src="/assets/images/host_claibrate_screen.png"></a>
    <a href="assets/images/host_display_calibrate_screen.png"><img src="/assets/images/host_display_calibrate_screen.png"></a>
    <figcaption>如上图所示左侧为主试机进入Calibrate界面；右侧为被试机上出现校准点，等待开始校准。</figcaption>
</figure>

在主试机屏幕上除了校准点之外，我们也可以看到一个“D”在屏幕上来回移动。这个“D”就是瞳孔中心在相机传感器上的位置。“D”的运动和眼睛的运动是同步的。

在Calibrate的过程开始之前，首先要跟被试叮嘱“三套嗑”：

* 盯住校准点中心的小黑点；
* 不要预判下一个点出现在哪里；
* 直到这个点消失之后去找下一个；

在被试盯住第一个校准点之后，按“空格”开始校准。

![host_calibrate](/assets/images/host_calibrate.gif)

校准过程会自动进行下去。每进行一个校准点，“D”就会在屏幕上打一个十字光标。我们可以通过十字光标的排列情况粗略低判断校准结果好坏：

![host_calibrate_results](/assets/images/host_calibrate_results.png)

如上图所示，左侧的十字光标排列均匀，校准结果较好；右侧十字光标排列混乱，校准结果很差。当所有校准点都显示完成后，系统会在右下角提示校准完成。

可以点击`Accept`或者按“Enter”来接受校准结果。

**小结**：按“C”进入Calibrate界面，被试盯住校准点后按“空格”开始，当所有校准点都执行完之后，按“回车”接受校准结果。
{: .notice--info}

## 4.6 执行Validate

即便校准结果不错，也终究是有误差的，那么误差到底是多少呢？我们在Validate环节来看一看。

在这一部中，被试的任务和Calibrate一样，只需要追随校准点即可。所以继续强调“三套嗑”即可。

点击`Validate`或者按`v`进入Validate界面。此时我们已经可以看到被试正在注视屏幕上哪一个位置了。

同样要求被试注视校准点，按“空格”开始Validate。

![host_validate](/assets/images/host_validate.gif)

系统会依次报告屏幕上不同位置的误差有多大，并且最后会在主试机右下角报告平局误差和最大误差。

![host_report_valiadte_error](/assets/images/host_report_valiadte_error.png)

在文字阅读任务中，一般要求平均误差小于0.5度，最大误差不超过1度；
在非文字的任务中，一般要求平均误差小于1度，最大误差不超过1.5度；
具体的精度要求根据研究对象适当调整。

如误差满足实验要求，则按“回车”返回到`Camera Setup`界面。

如误差不满足实验需求，则需要退回到Calibrate界面，重新执行校准。然后再通过Validate环节检测误差。

**小结**：按“V”进入Validate界面，被试盯住校准点后按“空格”开始，当所有校准点都执行完之后，检查当前精度是否满足要求。如满足则按“回车”接受校准结果，如不满足则按“Esc”退出Validate界面，从Calibrate环节重新开始执行。
{: .notice--info}

---

# 5. 开始实验

在主试机的`Camera Setup`界面按两下英文字母“O”开始实验。

第一次按“O”是进入到`Output`界面。第二次按“O”是命令眼动仪开始记录。

随后我们会进入到每个试次前都有的`Drift`界面。要求被试盯住校准点后，按空格继续实验。更多`Drift`相关的内容请点击[传送门](/eyelink/Drift/)。

随后实验将根据您的设计自动进行下去。

---

# 6. 结束实验

在实验流程结束后，实验程序会自动关闭，结果文件会自动储存在“/Results”文件夹下。

---

眼动仪的数据采集操作就这么多，如果还需要给被试做后测的话，就先去忙吧:P

以上。