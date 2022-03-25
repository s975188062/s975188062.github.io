tittle: "Experiment Builder 编程自检表"
excerpt: "百密必有一疏，检查常见的问题可以避免许多不必要的麻烦。"
read_time: false
header:
  overlay_image: /assets/images/host_data_collect_header.png
  overlay_filter: 0.5
categories:
  - Eyelink
tags:
  - Experiment Builder
toc: true
comments: false
author_profile: false
classes: wide
sidebar:
  title: "目录"
  nav: EB
---

> 您可以参考以下自查条目规避一些 EB 编程中的常见问题。
>
> 原版请移步[Experiment Builder Project Check List (version 2.3.1)](https://www.sr-support.com/attachment.php?aid=521)
>
> [下载 PDF 版](/assets/docs/EBProgramingCheckingList.pdf)

---

1. 是否将 Recording Sequence 的 Record 属性设置为 True ？

   *如果将 EB 程序中某个 Sequence 的 Reocrd 属性设置为 True，则在这个 Sequence 运行期间，眼动仪将同步记录数据（换一种说法，在这个 Sequence 开始运行之前和结束运行之后，EB 分别会发送令眼动仪开始记录和结束记录的命令）。通常只会对处于嵌套结构最内层的 Sequence 开启 Reocrd 属性。同时需要注意的是，Recording Sequence 仅用作数据采集，其 Interation Count 属性应当设置为 1 且不应在 Recording Sequence中创建 Datasource。*

2. 是否对关键控件设置了 Message 属性？

   *逐个检查 Recording Sequence 中的控件，确保每一个关键的控件都正确添加了 Message 属性，如 Display Action、PlaySound Action 或 Timer Trigger 等。当控件运行时，EB 需要将对应的 Message 属性写入 EDF 文件来标记事件的发生。如果没有这些 Message 信息，则无法将数据与发生的事件对其，无法在 DV 中分割 Interest Period，继而无法分析数据。*

   > *需要注意的是，Message 的内容不能是“纯数字”或“数字+空格+文本”的形式。建议使用纯文字进行 Message 标注，如需文本和数字配合使用（如[Eye-EEG](https://www.eyetracking-eeg.org/)要求的那样），请文本在前，数字在后，如“Keyword 33”。*

3. 是否将需要在主试机上进行检测的 Display 控件的 Use for Host Display 属性设置为 True？

   *如果不执行此项操作，您将无法在主试机上同步监测到被试机上呈现的刺激材料。您需要将目标 Display 控件的 Use for Host Display 属性设置为 True，并且将上一层 Trial Sequence 中的 Prepare Sequence 控件中的 Draw to Host 属性设置为 Image。*

   *需要注意的是，即便在 Recording Sequence 中可能有多个 Display 控件，也只能选择一个 Display 控件进行设置，如果您设置了多个，则只有第一个会生效。*

4. 是否添加了 Recording Status Message 以在实验过程中提示实验进度？

   *Recording Sequence 的 Record Status Message 让我们可以在数据采集过程中，在主试机的显示器上看到文本的提示信息，用于向主试告知实验进程（试次条件或实验完成进度等）。信息将在主试机 Record 屏 的右下角显示。*

5. 是否在 Recording Sequence 前添加了 PREPARE_SEQUENCE 控件。

   *Prepare Sequence 控件可以为其后面的 Recording Sequence 中所需的刺激材料进行预加载，将一个 Display 控件的显示内容发送到主试机上作为实验过程中的监测显示，重置后续 Sequence 的控件属性。如果使用的 Drift Correct 控件，则应将 Prepare Sequence 控件置于 Dirft Correct 控件之前，后面连接 Recording Sequence；若未使用 Drift Correct 控件，则应将 Prepare Sequence 控件和 Recording Sequence 相连。*

6. 是否为目标屏准备了兴趣区？

   *无论是阅读、Visual World 等呈现静态视觉刺激的实验范式，或一些其他需要呈现动态视觉刺激的实验范式，都可以在数据收集之前绘制好兴趣区。Experiment Builder 中的 Runtime word segmentation 或 Auto Segment 等功能可以帮您更便利的完成这一切。Data Viewer 也有创建和编辑兴趣区的功能。* 

7. 对显示设备的属性（分辨率、刷新率等）设置是否是显示器所满足的？

8. 是否将 Recording Sequence 的 `Is Real Time` 属性设置为 True？

   *Real Time 模式可以将实验程序进程置于最高级，这样可以发挥出被试机的全部性能，提高计时和操作的准确性。*

9. Camera Setup 控件和 Drift Correct 控件的 `Background Color` 是否和实验过程中屏幕的背景色保持一致？

   *若 Calibration/Validation/Drift Correction 过程中的屏幕亮度和实验过程中存在较大差别，将会导致较大的瞳孔基线变化。这样的瞳孔基线变化将对眼动仪的准确性产生干扰。请确保 Camera Setup 控件和 Dirft Correct 控件的背景色和实验过程中的刺激材料背景色保持一致。如果您在实验过程中使用了满屏显示的图片或视频材料，您可以使用中性灰（RGB: 128 128 128）作为 Camera Setup 和 Dirft Correct 过程的背景色。*

10. 在准备开始数据采集之前，是否在用于数据采集的被试机上执行了 Deploy 操作？

    *Deploy 程序可以创建一个用于数据采集的独立程序，这个独立程序可以脱离 EB 加密狗独立运行。此项操作务必在数据采集的电脑上执行，Deploy 过程需要对每一台电脑生成特定的依赖文件，在不同版本的操作系统中存在区别。*

11. 是否正确设置了随机选项？

    *请反复检查 Data Source 中的随机设置符合您的预期。您可以在 EB 的手册中找到随机设置的详细介绍。*

12. 您是否采取了一些设置来让被试机在运行实验过程中有更好的实时表现？ 

    *为了让被试机发挥出最佳的时间控制性能和稳定的表现，请务必确保遵循以下操作：*

    - [ ] *运行程序前，关闭**其他应用程序**；*
    - [ ] *关闭**任务栏**中其他应用程序；*
    - [ ] *如果操作系统是 Windows 10，关闭开始菜单中的**动态磁贴**；*
    - [ ] ***取消**全部计划任务（如数据备份、病毒查杀或系统升级等）；*
    - [ ] ***移除**非必要的硬件（如U盘、独立硬盘等）；*
    - [ ] *确保电脑没有被**木马**或**病毒**感染；*
    - [ ] ***关闭**屏幕保护程序；*
    - [ ] *笔记本电脑作为被试机时，将电源选项设置为高性能模式；*
    - [ ] *当电脑有多个网卡时，在控制面板中**禁用非必要的网卡**；*
    - [ ] *当您使用 NVidia 显卡时，**关闭** G-SYNC 功能；*
    - [ ] *卸载所有非必要软件，当您使用 NVidia 显卡时，确保电脑**没有安装** NVIDIA GeForce Experience；*
    - [ ] *如果您使用的是**最新的 NVidia 显卡**，请确保禁用 **NVIDIA Streamer Service**、**NVIDIA Streamer Network Service** 和 **NVIDIA Streamer User Agent**。 在 Windows 搜索框中键入“services.msc”以启动服务窗口。 如果您在“服务”窗口中找到这些服务，请选择该进程，单击“停止”按钮为会话禁用它，并将“启动类型”设置为“禁用”。 ；*
    - [ ] *使用管理员模式运行实验程序，**不要**在 EB 中使用 **Test Run** 进行数据采集，并在数据采集前关闭 EB 软件；*
    - [ ] *使用 LCD 显示器运行实验时，使用 LCD 显示器所能支持的**最高刷新率**。*

13. 备份和移动数据时，确保移动了整个 _deply 文件夹。

    *完成数据收集后，请确保不要将 EDF 文件或者 Results 文件夹移出 _deploy 文件夹。正确的操作是将整个 _deploy 文件夹压缩为压缩文件，之后将压缩文件转存至其他电脑解压缩。如果您只移动 EDF 文件或者 Reuslts 文件夹，您会丢失数据的依赖文件，继而无法在 DV 中查看饰演的背景材料和兴趣区。*

14. 完整进行预实验，确保可以得到全部计划的指标后，再开始正式实验的数据收集。

15. 是否使用了 Drift Correction？Drift Correction 的校准点是否设置在了合适的位置？

    *Drift Correction 可以让研究者在每个试次开始之前检查系统误差，也可以将被试的注视位置定位到屏幕上的特定位置。*

    *在阅读研究中，通常将校准点位置放置于文本第一个单词的位置或者稍偏前的位置。*

    *对于视觉搜索任务，通常会将校准点的位置置于所有搜索目标的中心，确保校准点到所有目标的距离相等。*

    *对于面孔相关的研究中，可以将校准点置于面孔的上方或下方，防止干扰。*

16. 多行文本素材是否设置了合适的字体、字号和行间距？

    **确保在阅读任务中使用双倍行距，对于需要固定字符宽度的英文阅读研究中，请使用 Courier New 字体来显示文本。更多的您可以参考文献来选择相关的参数。**

17. 是否使用了 ASIO 声卡来播放声音

    **EB 可以在 Windows 中使用 DriectX 或者 ASIO 声卡来播发声音刺激。与 ASIO 不同的是，DirectX 声卡无法保证声音刺激的播放精度，而且也不能录音。如果您需要精确的时间控制或录音，请使用 EB 支持的 ASIO 声卡播放声音（推荐：M-Audio Air 192｜4）。**

    **MacOS 系统自身的音频驱动系统可以与 WIndows 的 ASIO 媲美，可以忽略相关设置。**

---

以上。