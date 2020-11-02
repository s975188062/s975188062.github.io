---
title: "在 EB 中设置播放声音"
excerpt: "在Visual-World等相关研究范式中，需要播放声音刺激。正常来讲播放声音不是什么难事，但我们希望声音的播放延迟尽可能小，这就需要用到 ASIO 声卡。"
read_time: false
header:
  overlay_image: /assets/images/eb-asio-header.png
  overlay_filter: 0.5
categories:
  - Eyelink
tags:
  - Experiment Builder
  - Play Sound
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

简单来说，ASIO 的目的是降低音频延迟；同时 ASIO 作为系统中独立的音频通道可以避开 DirectSound（或其他通道）的干扰，从而使得 ASIO 应用程序（如音乐创作软件）可以不受系统中正在运行的其他程序干扰，达到降低声音播放和录制延迟的目的。同样一块声卡，假设使用 MME 驱动时的延迟时间为 750 毫秒，那么当换成 ASIO 驱动后延迟量就有可能会降低到 10 毫秒以下。

