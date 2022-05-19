---
title: "在 EB 中设计播放声音的实验"
excerpt: "听觉是我们第二大的信息来源。在言语加工领域，越来越多的研究者将语音信息作为刺激手段呈现在自己的实验中。本次我们来看一看如何在 EB 中播放声音。"
read_time: false
header:
  overlay_image: /assets/images/eb-play_sound_header.png
  overlay_filter: 0.5
  teaser: /assets/images/eb-play_sound_taster.png
categories:
  - Eyelink
tags:
  - Experiment Builder
  - Play Sound
toc: true
author_profile: false
sidebar:
  title: "目录"
  nav: EB
---

---

# 1. 声卡的抉择

在学习如何播放声音之前，我们需要先了解一下播放声音的硬件——声卡。

声卡是用来将声音文件播放出来的硬件，根据工作方式的不同，分为 集成声卡 和 ASIO声卡 两种。

**本节太长不看**：有条件的话尽量用 ASIO 声卡保证声音刺激呈现的时间精度，成本千元左右。实在不行用集成声卡也凑合。请跳到[<u>2. 播放声音</u>](/eyelink/eb-asio_sound_card/)继续阅读。
{: .notice--info}

## 1.1 集成声卡

简单来说，集成声卡在我们每个人的 Windows 电脑里面都有，是和计算机的主板集成在一起的，故名集成声卡。

> macOS 苹果电脑内部集成的声卡虽然是集成声卡，但是性能足以 ASIO 声卡，贵是有道理的……

根据集成声卡的型号不同，其性能表现也会有所差异，这种差异主要体现在声音的播放延迟上。

对于计算机而言，声音文件的播放流程是这样的：

    1. 读取声音文件；
    2. 将声音文件发给声卡播放；
    3. 声卡播放出声音。

对于我们写的实验程序而言，我们所记录的声音开始播放的时间是第 2 步，将声音文件交给声卡播放的时候。而声音真的播放出来的时间则是第 3 步。第 2 步和第 3 步之间的误差就是我们所说的声音播放延迟。

这种延迟在所有电脑中普遍存在，细心的同学可能早就感受过自己电脑的音画不同步。具体的延迟数值根据电脑情况不同，在 10 ～ 500 ms 之间浮动。

众所周知，**实验求稳**。抖动的延迟对于数据的影响不可预知，所以集成声卡并不是实验的最佳选择。

## 1.2 ASIO 声卡

> ASIO 是 Audio Stream Input Output 的缩写。是由 Steinberg 公司开发的专业声卡驱动模式的一种简称。
> 
> 简单来说，ASIO 的目的是降低音频延迟；同时 ASIO 作为系统中独立的音频通道可以避开 DirectSound（或其他通道）的干扰，从而使得 ASIO 应用程序（如我们的实验刺激程序）可以不受系统中正在运行的其他程序干扰，达到降低声音播放和录制延迟的目的。同样一块声卡，假设使用 MME 驱动时的延迟时间为 750 毫秒，那么当换成 ASIO 驱动后延迟量就有可能会降低到 10 毫秒以下。

关于 ASIO 声卡的内容，Charlie 已经在[<u>**播放声音或录音？整块声卡！**</u>](/eyelink/eb-asio_sound_card/)中进行了介绍，此处不再赘述。

对于 macOS 的用户，恭喜你，电脑里面已经内置了 ASIO 声卡，无需额外购买。
{: .notice--info}

---

# 2. 播放声音

## 2.1 准备声音文件

在 EB 中仅支持 *.wav 格式的音频，因此我们首先要将所有声音文件转换为 wav 格式。

在命名上，请一如既往的遵循 ASCII 命名，即**仅**使用英文字母、数字和下划线的组合对音频文件进行命名。

在数据格式上，EB 仅支持 16 bit PCM 编码，在转换音频文件格式的时候，请注意此选项。

> 关于 16 bit PCM 编码，大多数格式转换工具在输出 wav 格式时都默认遵循此编码格式。如果没有选择此编码格式，在下面导入 Library Manager 的过程中会报错，因此无需过于纠结编码格式。

接着，我们来将 *.wav 文件导入到 Library Manager 中。

![eb-play_sound-add_wav_file_to_library_manager](/assets/images/eb-play_sound-add_wav_file_to_library_manager.png)

> 此处 Charlie 仅添加了一个文件，如果你有多个文件要添加，可以一起选中，一起添加进来。

## 2.2 设置声卡驱动

在菜单栏打开 `Preference`，选中 `Audio`，设置 `Audio Driver`。

对于 macOS 苹果电脑用户，只有 OSX 一个选项，入下图所示。

![eb-play_sound-config_audio_driver_in_mac](/assets/images/eb-play_sound-config_audio_driver_in_mac.jpg)

对于 Windows 用户：

* 如果您使用的是集成声卡，请将 `Audio Driver` 选项设置为 `DirectX`
* 如果您使用的是 ASIO 声卡，请将 `Audio Driver` 选项设置为 `ASIO`

> 对于使用 ASIO 声卡的 Windows 用户，您需要先安装声卡驱动，并运行 Config ASIO 来进行声卡配置。您可以在[>>>播放声音或录音？整块声卡！<<<](/eyelink/eb-asio_sound_card/#23-config-asio)找到相关的内容.

![eb-play_sound-config_audio_driver_in_win](/assets/images/eb-play_sound-config_audio_driver_in_win.png)

## 2.3 设置播放声音 - 以 Visual World 范式为例

在给出一个播放声音的示例之前，我们先来看一个 Visual World 范式设计的典型案例（同 EB 的 Visual World 示例）。

![eb-play_sound-visual_world_paragdim](/assets/images/eb-play_sound-visual_world_paragdim.png)

* 屏幕上出现 4 幅图片，持续时间 500 ms；
* 保持屏幕显示内容不变，播放声音刺激；
* 被试操作鼠标点击正确答案。

在 EB 中，有两个播放声音的方法，一个是使用 Play Sound Action，另一个则是使用 Display Screen Action 的 `Synchronize Audio` 属性。

对于使用 DirectX 集成声卡来播放音频的用户，仅可以使用 Play Sound Action 来播放声音，而使用 ASIO 声卡来播放声音的用户，您可以使用下面任意一种播放方式。

### 2.3.1 Play Sound Action

> 本节内容基于 Experiment Builder 的 Visual World 示例。本章只讲述播放声音的部分。

您可以在 Action 控件库中找到 Play Sound Action 和 Play Sound Control Action。我们可以分别使用这两个控件播放音频或者控制播放的暂停和结束。

![eb-play_sound-where_is_play_sound_action](/assets/images/eb-play_sound-where_is_play_sound_action.png)

我们来看下如何在 EB 中实现 Visual World 范式。

![eb-play_sound-node_connections](/assets/images/eb-play_sound-node_connections.png)

Display Screen Action 和 Timer Trigger 两个控件在之前的教程里我们都讲过，此处略过不说，我们来看关键的 Play Sound Action。

选中 Play Sound Action，查看其属性栏。

![eb-play_sound-play_sound_action_directX](/assets/images/eb-play_sound-play_sound_action_directX.png)

在使用 DirectX 的情况下，Play Sound Action 仅有三个主要属性 `Sound File`、`Volume` 和 `Balance`。

* Sound File - 即需要播放的声音文件名。如上图所示，您可以将每个试次需要播放的声音文件名写入 Datasource，并将此属性引用到对应的 Datasorce 条目上，即可实现引用播放。
* Volume - 音量相对百分比，默认为 1 。最小为 0 ，最大为 1。
* Balance - 声道平衡，默认为 0 。负数偏左，正数偏右。如 0.5 代表左声道音量为 50% 且右声道音量为 100%。声音整体偏向右侧。此属性在 macOS 系统中无法使用。

至此我们就构成了 Visual World 范式的最基本呈现。

> 如果您是在 ASIO 驱动下使用 Play Sound Action 播放声音，还会有 `Play Start Sample` 和 `Play Start Sample` 两个可以属性，您可以在本文末尾的控件属性详表中找到相应的介绍。

有些情况下，声音刺激还没有完成播放，被试就已经完成反应，试次需要提前结束。此时我们可以使用 Play Sound Control Action 对 Play Sound Action 进行控制。

向需要停止声音播放的位置插入一个 Play Sound Control Action ，随后设置其属性。

![eb-play_sound-play_sound_control_action](/assets/images/eb-play_sound-play_sound_control_action.png)

* Target Play Sound Action - 即要控制哪一个 Play Sound Action，注意正确选择。
* Operation - 要对目标 Play Sound Action 执行哪种操作，停止、暂停或是播放。

> 您可以将 Play Sound Action 理解为一个音乐播放器，上面有播放、停止和暂停的三个开关。如果您的程序中出现了多个 Play Sound Action ，那您就拥有了多个音乐播放器。根据您的实验设计，他们可以独立工作，也可能一起工作，您可以通过 Play Sound Control Action 去控制特定 Play Sound Action 的工作状态。

如上图所示，即通过 Play Sound Control Action 将 PLAY_SOUND 这个控件播放的声音停止了。

### 2.3.2 Synchronize Audio

如果您使用 ASIO 声卡，则可以使用 Display Screen Action 的 `Synchronize Audio` 属性设置声音的播放。只需要一个 Display Screen Action 即可完成 “<u>视觉呈现</u> → <u>等待 500 ms</u> → <u>播放声音</u>” 的全部操作。

与之前不变的是首先您需要在 Display Screen Action 中设置视觉内容的呈现。随后回到 Recording 层，选中 对应的 Display Screen Action ，找到 `Synchronize Audio` 属性。勾选为 `True`，您会发现在 `Synchronize Audio` 属性后又多出了一些属性。

![eb-play_sound-sync_audio](/assets/images/eb-play_sound-sync_audio.png)

* Sound File - 即需要播放的声音文件名。如上图所示，您可以将每个试次需要播放的声音文件名写入 Datasource，并将此属性引用到对应的 Datasorce 条目上，即可实现引用播放。
* Volume - 音量相对百分比，默认为 1 。最小为 0 ，最大为 1。
* Pan - 声道平衡，同 Balance ，默认为 0 。负数偏左，正数偏右。如 0.5 代表左声道音量为 50% 且右声道音量为 100%。声音整体偏向右侧。此属性在 macOS 系统中无法使用。
* Sound Offset - 声音播放延迟，单位是 ms ，默认为 0 。如 500 即在视觉刺激呈现 500 ms 后播放声音。

如果需要提前停止声音播放，一样可以使用 Play Sound Control 控件进行控制，将其 `Target Play Sound Action` 属性设置为对应的 Display Screen Action 即可。

---

以上。