---
title: "播放声音或录音？整块声卡！"
excerpt: " 最近 Visual-World 和翻译研究都很火，这是两种需要播放或者记录声音通道的任务。正常来讲播放或者记录声音不是什么难事，但我们希望延迟尽可能小，这就需要用到 ASIO 声卡。"
read_time: false
header:
  overlay_image: /assets/images/eb-asio-header.png
  overlay_filter: 0.5
categories:
  - Eyelink
tags:
  - Experiment Builder
  - ASIO
toc: true
comments: true
author_profile: false
sidebar:
  title: "目录"
  nav: EB
---

---

ASIO 是 Audio Stream Input Output 的缩写。是由 Steinberg 公司开发的专业声卡驱动模式的一种简称。

简单来说，ASIO 的目的是降低音频延迟；同时 ASIO 作为系统中独立的音频通道可以避开 DirectSound（或其他通道）的干扰，从而使得 ASIO 应用程序（如我们的实验刺激程序）可以不受系统中正在运行的其他程序干扰，达到降低声音播放和录制延迟的目的。同样一块声卡，假设使用 MME 驱动时的延迟时间为 750 毫秒，那么当换成 ASIO 驱动后延迟量就有可能会降低到 10 毫秒以下。

---

# 1. ASIO 声卡的选购

本文的写作时间是 2020年 年底，截止目前最新的 EB 软件版本为 2.3.38。根据用户手册的信息，SR Research 官方测试了如下几款声卡：

|Tested Sound Cards for EB Compatibility Using ASIO Driver|32-bit and 64-bit Windows 10|Format|Notes|
|---|---|---|---|
|Creative Labs Sound Blaster Audigy 5/RX (Model SB1550)|Supported|PCI-Express||
|Creative Labs Sound Blaster AE-5 (Model SB1740)|Supported|PCI-Express|This card has a 4ms delay for the for Voice Key triggers.|
|M-Audio M-Track Air|Supported|USB||
|M-Audio M-Track 2X2M|Supported|USB||
|Steinberg UR22MKII|Supported|USB||
|Roland Rubix 22c|Supported|USB||

## 1.1 PCI-e 声卡

PCI-e 声卡是指声卡使用 PCI-Express 接口。翻译一下就是这种声卡是安装在电脑机箱内部的（插在主板上）。虽然 PCI 和 PCI-e 是两种不同的接口，但我们暂且不表。下图所示就是一个 PCI-e 声卡：

![eb-asio-pcie_card](/assets/images/eb-asio-pcie_card.png)

由于这种声卡的输入端没有增益，录制的音频音量特别小。因此这种声卡一般只适合播放声音。

> 至于为什么 PCI-e声卡 在 windows 系统录音机中录音可以听清而 EB 软件里面录音却声音很小，Charlie 也还没搞清楚。

如上图所示的声卡型号是 [创新科技（Creative）Sound BlasterX AE-5](https://item.jd.com/5273333.html)。在京东上的价格为 1049 。这个价格并不比既可录音也可播放声音的 USB声卡 便宜。

## 1.2 USB 声卡

下图所示，是 Charlie 比较推荐的声卡型号 [M-Audio M-Track Air 192|4](https://search.jd.com/Search?keyword=M-Audio%20M-Track%20Air%20192&enc=utf-8) 。

![eb-asio-usb_audio_card](/assets/images/eb-asio-usb_audio_card.png)

由于 USB声卡 内部有功放（功率放大器），因此我们输入的声音信号可以经过放大再拾取记录。

M-Audio M-Track Air 192 系列共计有四个分型号，分别是：

> M-Audio M-Track Air 192｜4
> M-Audio M-Track Air 192｜6
> M-Audio M-Track Air 192｜8
> M-Audio M-Track Air 192｜14

最后数字的变化是 I/O 数量不同，即 输入/输出 接口的数量不同。这个地方我们要从声卡最原始的功能来解释：

USB声卡本身是作为乐器/人声的录音使用的，所以声卡本身是一个音乐制作的硬件。我们实验中看重其毫秒级的声音录制/播放延迟，将其用作我们播放/录制声音的工具。其 输入/输出 接口数量的多少实际上是可以同时连接多少乐器和音箱，而作为我们实验过程中的工具，保证输入和输出通道各一个既可满足需求。所以我们选择 I/O 最小的 _M-Audio M-Track Air 192｜4_ 。

---

# 2. 安装 ASIO 声卡

## 2.1 硬件连接

首先我们先简单了解一下声卡上的接口，下图是声卡厂商提供的连接示意图：

![eb_asio_usb_card_io_port](/assets/images/eb_asio_usb_card_io_port.png)

### 2.1.1 连接电脑

所谓 USB声卡 ，就是通过 USB线 连接到电脑上的声卡。所以我们连接硬件的时候就通过包装中的 USB 线来连接。

在包装中应该附赠如下图所示的两个 USB 线，声卡的一端是 Type-C 接口，在声卡的后面，电脑的一端根据情况选择 Type-C 或者 Type-A。

![eb-asio-usb_card_usb_cable](/assets/images/eb-asio-usb_card_usb_cable.png)

声卡后面接线处如下图所示：

![eb-asio-usb_card_back_face](/assets/images/eb-asio-usb_card_back_face.png)

如果成功连接到电脑上，声卡上面的电源指示灯（Power）就会亮起。

![eb-asio-usb_card_power_led](/assets/images/eb-asio-usb_card_power_led.png)

如果电源指示灯没有正常亮起，注意检查接线处是否没有插紧。

### 2.1.2 连接麦克（不需要录音可以跳过）

![eb-asio-usb_card_mic_input_port](/assets/images/eb-asio-usb_card_mic_input_port.png)

将麦克接入上图所示的接口。大家所购买的麦克不同，麦克所接出的线也不同，一般来讲是四种：卡侬接口、6.5mmm 大三芯、3.5mm 小三芯和 USB 接口。其中USB接口的麦克是直接接电脑使用的，不能接到声卡上，选购麦克的时候一定要避免。

![eb-asio-audio_cables](/assets/images/eb-asio-audio_cables.png)

卡侬头 和 6.5mm 大三芯是可以直接接到声卡后面的接口上的，直接插入即可。而 3.5mm 小三芯由于尺寸问题，需要下图所示的转接头将 3.5mm小三芯 转为 6.5mm大三芯 再插入声卡使用。

![eb-asio-35_2_65_audio_adapter](/assets/images/eb-asio-35_2_65_audio_adapter.png)

### 2.1.3 连接音箱/耳机（不需要播放声音可以跳过）

声音的接口根据实验室的实际情况选择连接音箱或者耳机。

音箱的接口在声卡的背面，如下图所示，由两个 6.5mm大三芯 分为左右两个声道。

![eb-asio-usb_card_back_panel_monitor_output](/assets/images/eb-asio-usb_card_back_panel_monitor_output.png)

耳机的接口在声卡的前面板，规格是双声道的 6.5mm大三芯 。

![eb-asio-usb_card_headphone_port](/assets/images/eb-asio-usb_card_headphone_port.png)

连接耳机的时候，耳机通常都是 3.5mm小三芯 ，因此我们在连接耳机的时候同样需要一个 3.5mm 转 6.5mm 的转接口。

![eb-asio-35_2_65_audio_adapter](/assets/images/eb-asio-35_2_65_audio_adapter.png)

>最后我们再回顾一下所有的硬件连接，请忽略下图中的乐器：
>
>![eb_asio_usb_card_io_port](/assets/images/eb_asio_usb_card_io_port.png)

## 2.2 安装驱动

驱动文件下载链接：[Install M-Audio AIR 192 4 1.0.3(提取密码: t0ca)](https://pan.baidu.com/s/1_XO6yGfI0-bzfkHSvHrLBg)

M-Audio Air 192｜4 声卡支持 Win7 以上系统，下载好驱动程序默认安装即可。

驱动安装好之后，我们可以在 “开始菜单 - M-Audio” 文件夹中找到 `M-Audio AIR 192 4 Control Panel`。如果声卡已经连接在电脑上的话，打开 `M-Audio AIR 192 4 Control Panel` 则可以看到如下窗口。

![card_not_connected](/assets/images/eb-asio-card_connected.png)

但是如果看到如下图所示的窗口，提示 "Hardware not connected"，请检查硬件连接或者驱动安装。详细情况可咨询购买声卡的商家客服。

![card_not_connected](/assets/images/eb-asio-card_not_connected.png)

## 2.3 Config ASIO

成功安装声卡之后，在开始菜单的 SR Research 文件夹中打开 Configure ASIO 工具并打开。

将 ASIO Driver 设置为我们的声卡（此处Charlie用的是M-Aduio AIR 192|6）。

![asio_settings](/assets/images/eb-asio-asio_settings.png)

---

以上。