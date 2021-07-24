---
title: "WebLink Overview"
excerpt: "不同于在 Experiment Builder 中进行的传统序列实验，Weblink 为研究人员提供了一个更加开放而且多元的刺激呈现空间，允许被试自由地完成各种行为任务。"
read_time: false
header:
  overlay_image: /assets/images/wl-header.png
  overlay_filter: 0.5
  teaser: /assets/images/wl-header.png
categories:
  - Eyelink
tags:
  - WebLink
toc: true
comments: true
author_profile: false
sidebar:
  title: "目录"
  nav: WL
---

---

![wl-overview-main_window](/assets/images/wl-overview-main_window.png)

`WebLink` 是加拿大 SR Research 公司（Eyelink 眼动仪的制造商）在 2019 年底针对媒体实验发布的一款数据采集软件。

与大家已经非常熟悉的 `Experiment Builder` 不同，`WebLink` 更适用于非序列式的、自由的实验材料。

在 `Experiment Builder` 中，我们通常会准备几十甚至上百个试次，每个试次都会重复相同的任务流程。

`WebLink` 则是设计用来进行网页浏览、移动终端交互设计、电子竞技等新兴领域研究。并且 `WebLink` 在上述领域中都进行了从实验需求角度出发的优化。

# 1 授权

`WebLink` 是单独授权的。

在没有加密狗的情况下无法打开软件体验。但是好在 SR Research 会一定程度上开放 `WebLink` 的试用。

如果您看了此篇文章并对 `WebLink` 感兴趣，您可以向 SR Research 中国代表处 北京博润视动科技有限公司 的技术支持邮箱（support@bjbrainvision.com）写邮件申请试用。

截止本文发布的时间（2021年7月），试用仍然可以申请。但需要注意的是，每个学院只能申请一次激活码，而且试用期限仅为 30 天。

您可以先与工程师建立联系，在实验完成一定程度的准备后，再申请激活码进行相关实验研究。

---

# 2 控件介绍

## 2.1 Camera Setup
    
![wl-overview-camerasetup_icon](/assets/images/wl-overview-camerasetup_icon.png)

相机校准，继承了 Eyelink 的一贯风格，可以自定义校准点位置、顺序、颜色和提示音。相对于 EB 中满血的校准不同，在 Weblink 中删除掉了自定义校准图片的设置，不过从应用实验的角度出发，这点对实际实验完全没有影响。
    
## 2.2 Accuracy Check
    
![wl-overview-accuracy_check_icon](/assets/images/wl-overview-accuracy_check_icon.png)

误差检测，即 EB 中的 `Drift Correct`，略。
    
## 2.3 Webpage

![wl-overview-webpage_icon](/assets/images/wl-overview-webpage_icon.png)

网页。重头戏来了！WebLink 这个软件的精髓就在 Webpage 这个控件上。

此处 Charlie 借用软件开发行业的一句话：“将一切可以 web 化的东西全部 web 化。”

> *“这句话的意思是指，由于互联网技术的发展，网页技术已经不再仅仅满足于媒体信息的呈现，而同时也具备了很多数据交互的功能。对于传统软件开放行业而言，开发一款软件的耗费是巨大的，开发一个网页站点所需要的经历要小得多。如果一个软件的需求可以全部通过 web 技术来实现，那么就将这个网站套壳伪装成桌面软件，而不去进行耗时耗力的桌面应用开发。”*

闲话少叙，我们来看一看都有哪些类型的实验可以做到 Webpage 里面。

* **UI/UX：**在UI设计领域，眼动研究一直是用以测量和评价设计合理性的理想工具。

* **长篇阅读：**在 EB 中的阅读研究主要面向言语心理学和心理语言学方面的研究。而研究者希望进行一些长篇阅读中的注意力方面的研究时，EB 每屏呈现的一两百词就显得很局促了。因此我们可以将文本材料做到网页中，让被试以浏览网页的方式进行阅读。**需要指明的是，如果只是单纯的阅读，不包含任何交互的话，您可以通过后面介绍的 PDF 控件，以更简单的方式呈现长篇阅读材料。**

* **试题测量：**试题测量是 Charlie 接触最多的需求了。通过记录和分析考生在答题过程中的眼动数据，研究作答过程中的注意力分配和认知加工负荷变化等问题，同时也从另一个角度评价试题的效度。此类研究的传统方法是通过屏幕录制或回溯式访谈等主观因素较多的方式生产数据，而通过 Webpage 进行答题的话，可以同时记录按键和眼动信息，从客观的角度提供了“硬”数据。

* **翻译研究：**翻译研究可以认为是一种特化的答题过程，webpage 在记录眼动信息的同时，还支持同时录音或录像。值得一提的是，如果在实验过程中被试借助了网页端的翻译工具的话，webpage 控件是可以检测到并在数据处理的时候过滤掉的。
 
* **其他：**正如前面所说，一切可以做到网页中的东西都可以用 webpage 控件进行实验。但有一个大前提是页面必须相对简单。如果有很复杂的页面变化和交互的话，webpage 最重要的页面拼接功能有可能没法正常工作。
 
那么我们来继续介绍一下**页面拼接**这个功能。

其实上面所介绍的 webpage 功能，在 Weblink 出来之前也已经有很多探索性的研究在做了。例如用户界面设计中的研究，可以通过录屏的方式进行。只要记录下浏览过程和眼睛注视在显示器上的位置，也可以通过数据处理的手段来或者注意力分配的数据。

但问题是这样处理一个人的数据所需的工作量太大了！非常非常大！而且有很多主观操作的不确定性。如果你进行过类似的研究，相信你一定还记得绘制兴趣区的痛苦。因为在录屏的视频中，兴趣区一会儿移动，一会小时，一会又再出现。整个数据预处理的过程繁复枯燥。
 
传统录屏方式的数据：
 
![wl-overview-demo_page](/assets/images/wl-overview-demo_page.gif)
 
Weblink 页面拼接的数据：

![wl-overview-demo_page](/assets/images/wl-overview-demo_page.png)

Weblink 的页面拼接功能通过记录网页浏览过程中的页面位移，将眼动数据还原到整个页面上。这与传统方式最大的进步是将动态的视频变为静态的图片。用完整页面的视角观察眼动过程，而不是每次仅能看到一个小窗口。

数据处理的工作量显而易见。

[>>>点此<<<](https://pan.baidu.com/s/161p9rq4CW3SKAbFc5RjX-w)下载 Webpage 示例程序和数据（提取码：37ti）。需要注意的是，需要使用 v4.2 以上的 DV 才可以查看。

## 2.4 Image

![wl-overview-image_icon](/assets/images/wl-overview-image_icon.png)

Image 的话相信不需要过多的介绍，这只是一个呈现图片的控件，与 E-Prime 中的 Image 控件基本一致。

在 Weblink 中，Image 控件多用于呈现一些功能性的提示。

## 2.5 Video

![wl-overview-video_icon](/assets/images/wl-overview-video_icon.png)

播放视频。可以指定视频播放的速度和起止帧。相对于 EB，无需将视频和音频分开播放。

## 2.6 PDF

![wl-overview-pdf_icon](/assets/images/wl-overview-pdf_icon.png)

顾名思义，是 PDF 浏览。对于不需要进行交互的长篇阅读任务，将其转写成 PDF 格式的成本要远低于网页的 html 格式。

## 2.7 Screen Recording

![wl-overview-screen_recording_icon](/assets/images/wl-overview-screen_recording_icon.png)

屏幕录制。通过一边录制屏幕，一边记录眼动的方法，几乎可以进行任何类型的眼动实验。

当然也可以通过这种方法进行一些有趣的应用研究，例如运（dian）动（jing）心理学。

## 2.8 External Video

![wl-overview-external_video_icon](/assets/images/wl-overview-external_video_icon.png)

External Video 可以理解为 External Screen Recording。

通过视频采集卡等外部硬件，可以讲手机和平板等设备的显示内容进行录制，同时采集眼动数据，即可以进行移动设备使用的眼动研究。例如：微信聊天、浏览淘宝和移动端游戏等。

![wl-overview-external_video_demo](/assets/images/wl-overview-external_video_demo.png)

## 2.9 Scene Camera

![wl-overview-scene_camera_icon](/assets/images/wl-overview-scene_camera_icon.png)

Scene Camera 即场景相机。允许客户通过 网络/USB 摄像头录制被试面前发生的事情，并同时记录眼动。

通过眼动仪和场景相机的组合，Scene Camera 实际上相当于一个视角固定的头戴式眼动仪。

![wl-overview-scene_camera_demo](/assets/images/wl-overview-scene_camera_demo.png)

---

# 3. 其他功能

除了 Weblink 的核心功能外，Weblink 的安装包中也预置了两个小工具。

* Remote Live Video - 远程监控插件
* Subject Camera Plugin - 同步视频录制插件

## 3.1 实验监控 Remote Live Video

![wl-overview-remote_preview](/assets/images/wl-overview-remote_preview.png)

通过前面的介绍，相信大家都已经体会到 Weblink 进行的任务是非常开放的。

除了给被试机开放第二屏监测实验之外，也可以通过 Remote Live Video 插件在局域网内的第三台机器上进行实验进程监控。

## 3.2 监控相机 Subject Camera Plugin

![wl-overview-participant_camera_plugin](/assets/images/wl-overview-participant_camera_plugin.png)

Subject Camera Plugin 则是提供了一个类似视频脑电的概念。

在实验过程中可以录制被试的面部表情，以便数据挖掘分析。

---

# 4 写在后面

Weblink 的最大特点就是灵活。

不同于 EB 或者 E-Prime 中传统的序列实验，在 Weblink 中进行的实验都是非常开放无序的。因此，如何处理好这些开放的数据将是研究者所必需面对的问题。

同时也因为 Weblink 的开放，在实验中如何进行变量控制也是研究者所必须解决的问题。

---

工具的发展可以大力推动研究发展，但切忌为了使用工具而进行研究。所有的工具和方法都应该是为了目的而服务的。——Charlie
{: .notice--info}

以上。