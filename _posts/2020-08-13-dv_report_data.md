---
title: "导出数据 - Report Data"
excerpt: "Data Viewer 数据处理的最后一步。"
read_time: false
header:
  overlay_image: /assets/images/dv_intro_header.png
  overlay_filter: 0.5
categories:
  - Eyelink
tags:
  - Data Viewer
  - Data Report
toc: true
comments: true
author_profile: false
sidebar:
  title: "目录"
  nav: DV 
---

---

用DV进行眼动数据处理，一共分为五步：

* 导入数据
* Trial ReGroup
* 设置兴趣期 Interest Period
* 设置兴趣区 Interest Area
* 导出数据

我们已经完成了前面的部分，现在来讲解最有一步 数据导出 。

# 1. 数据导出的逻辑和指标分类

![dv_report_data_show_in_menu_bar](/assets/images/dv_report_data_show_in_menu_bar.png)

在菜单栏中依次点击“ Analysis - Reports ”，可以看到很多 Report 选项，我姑且称之为“报表”。结构如下：

```
Reports
├── Recording Event Sequence Data …         # 导出眼动 Event 数据流
|
├── Fixation Report …                       # 报告每个注视点 Fixation 的相关指标
├── Saccade Report …                        # 报告每个眼跳 Saccade 的相关指标
├── Interest Area Report …                  # 报告每个兴趣区 Interest Area 的相关指标
├── Trial Report …                          # 报告每个试次 Trial 的相关指标
├── Sample Report …                         # 报告每个采样点 Sample 的相关指标
├── Message Report …                        # 报告每个信息 Message 的相关指标
├── 
├── Aggregate Event Statistics …            # Event合计指标报表
├── Aggregate Interest Area Report …        # 兴趣区合计指标报表
├── Time Course (Binning) Analysis …        # 时序过程分析
|  
├── Import Report Variable Selection …      # 导入报表设置
└── Export Report Variable Selection …      # 导出报表设置
```

可见的是，报表的种类有很多，所涵盖的指标范围也很广，大概有800种。

然而没有人会使用全部的指标，通常在一篇研究中使用的指标数量为 3-4 种。

## 1.1 找到指标的类别和Report归属

眼动指标的种类有很多，但其本质都是某个特定元素的特定属性。

举个简单的🌰，眼动研究中最常用的指标：Dwell Time

> Dwell Time 中文一般译作“停留时间”。“停留时间”这个描述其实并不完整，如果要完全展开的话，应当翻译为“**在当前兴趣区的停留时间**”。
>
> 由此可见，Dwell Time 这一指标是以兴趣区为单位进行统计的，每一个兴趣区都有自己的 Dwell Time 属性。所以如果要导出 Dwell Time 的值，应当在 Interest Area Report 中进行操作。

再来一个🌰：Pupil Size Max

>Pupil Size Max是指瞳孔尺寸的最大值，用科学角度来严谨地展开描述是“**某个特定的时间段内的瞳孔尺寸最大值**”。
>
>由此可见，这个“**特定的时间段**”是什么很重要，可以是整个试次，也可以是某个特定的兴趣期。但是由于设定好兴趣期以后，整个试次就缩短为兴趣期的时间，因此 Trial Report 等价于 Interest Period Report。根据IP设置的不同，将IP设置为整个试次或是某个兴趣期，来导出特定时间段的 Pupil Size Max。

其他各种各样的指标有很多，需要各位先理解指标含义再进行数据导出操作。

## 1.2 执行数据导出操作

### 1.2.1 设置IP

在 Pupil Size Max 的例子中，我们已经强调了IP的重要性。所以在执行数据导出之前，首先确认是否已经设置了正确的 IP 。

### 1.2.2 选择指标类型并导出

以 Dwell Time 、 First Fixation Time 和 Gaze Duration 为例，进行数据导出操作。

以上三个指标都是基于兴趣区进行计算的，所以我们到 Interest Area Report 中进行操作。

依次在菜单栏中点击“ Analysis - Reports - Interest Area Report …”，可见如下窗口：

![dv_report_data_ia_report_window](/assets/images/dv_report_data_ia_report_window.png)

**红色区域 Available Variables** 是指可以选择的指标类型。

**橘色区域 Selected Variables** 是指已经选择的要导出的指标类型。

**绿色区域 Variable Definition** 是指标的定义，在 Available Variables 和 Selected Variables 中选中任意指标，都会在此处显示指标的含义。

**蓝色区域 其他的一些设置** 通常来讲我们是用不到的，再次做一些简单介绍。

> **Exclude Trial String ：** 排除某些特定的试次，操作起来比较复杂，不如直接全部导出，在做统计分析的时候再进行筛选排除。
> 
> **Place Quotes (") Around String / Text Variables ：** 在导出的数据中，对文本类型的数值前后添加英文引号(")。
> 
> **Create Output Report For All Custom Interest Period ：** 默认情况下，DV只会根据当前的IP设置导出数据，但若勾选此选项，则会根据我们现有的全部IP设置导出数据。此处有两个可用的选项，**Report IP Data in One File** 将所有的兴趣期都导出到一个文件中，**Report IP Data in Multiple File** 每个兴趣期单独作为一个文件导出。
> 
> **Create one Report File per EDF File ：** 为每个 .EDF 文件单独导出数据。

对于我们日常的操作其实很简单，选中需要导出的指标，点击导出即可，不需要过多的设置。

![dv_report_data_report_in_ia_report_window](/assets/images/dv_report_data_report_in_ia_report_window.png)

如上图所示，我们设置导出了 6 个指标。其中前两个指标是默认的，后四个是我手动选择的：

* RECORDING_SESSION_LABEL -> .edf文件名
* TRIAL_INDEX -> 试次的运行次序ID
* IA_LABEL -> 兴趣区的名字
* IA_DWELL_TIME -> 总停留时间
* IA_FIRST_FIXATION_DURATION -> 首次注视时长
* IA_FIRST_RUN_DWELL_TIME -> 首次加工时间

> 其中第 6 个指标 IA_FIRST_RUN_DWELL_TIME 首次加工时间 就是 Gaze Duration，同一个指标在不同研究方向中名称不同的现象其实是很常见的，所以要根据 Variable Definition 来判断是不是自己想要的指标。

完成指标选择后按 Next 执行导出操作。

> 如果您一直没有保存的您的 .evs 文件，那么在导出过程中，可能会提示保存 .evs 文件，按照指导保存到您希望的路径即可。
> 
> ![dv_report_data_save_evs](/assets/images/dv_report_data_save_evs.png)

默认情况下，导出的数据文件会保存至于 .evs 同路径的 Output 文件夹下，您可以选择其他路径导出。

![dv_report_data_save_report_file](/assets/images/dv_report_data_save_report_file.png)

Windows 系统会导出 Excl 格式，而 Mac OS X 系统则会导出 txt 格式。您可以直接读入统计软件或者使用 Excl 打开。

> 某些情况下使用 Excl 时可能会由于编码格式识别错误出现乱码，请先设置编码格式再打开。您也可以使用Libre Office 打开，一般不会出现编码错误的问题。

打开后文件内容如下：

![dv_report_data_report_file](/assets/images/dv_report_data_report_file.png)

根据需求继续导出其他数据即可。

---

# 2. 指标列表

**如何快速定位指标在哪一个Report类型中？** 这是一个很必要提出却又没有什么好的解决办法的问题。

Charlie推荐的方法是熟读文献，明白指标的含义，并且内化为自己的理解，再根据自己的理解到 Report 窗口中寻找，用 Variable Definition 来确定是不是自己要找的指标。

这是一个经验内化的过程，DV 的指标命名规则还是很简单的，所有指标都是由几个简单的名词缩写组成，大多数时候顾名思义即可。

您可以到 DataViewer User Manual 中第 6 节 Data Analysis and Report 中查找数据指标的定义表格，用户手册下载链接如下：

[DataViewer User Manual 4.1.1.pdf(w7f2)](https://pan.baidu.com/s/1wf4CD_c3Thp5oXa9cJGN-Q)

---

以上。
