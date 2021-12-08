---
title: "数据清洗 4-Stage Fixation Cleaning"
excerpt: "阅读中的数据清洗，可能会让效应不显著的数据变得显著，也可能什么效果有没有。"
read_time: false
header:
  overlay_image: /assets/images/dv_intro_header.png
  overlay_filter: 0.5
categories:
  - Eyelink
tags:
  - Data Viewer
  - 4-Stage Fixation Cleaning
toc: true
comments: true
author_profile: false
sidebar:
  title: "目录"
  nav: DV
---

---

# 1. 什么是 4-Stage Fixation Cleaning

4-Stage Fixation Cleaning 是一种数据清洗方法，一般在**阅读研究**的数据预处理中使用。其背后的逻辑大致如下：

1. 被试完全集中在阅读过程中，每一个注视和每一次眼跳都是集中在阅读材料上，且均有其言语加工上的意义*（这是所有阅读研究的前提）*；
2. 数据质量尚可，对每个兴趣区*（通常为一个词）*的注视都不会跑到其他的兴趣区去；
3. 40-80ms 的注视点由于时间过短，无法获取任何有效的视觉信息；
4. 时间过长的注视存在注视溢出，虽然在注视着某个兴趣区，但实际上大脑中思考的内容已经与这个兴趣区不直接相关。

所以当数据中出现以下的几种注视事件时，我们可以将其删除或者就近合并：

* 时间过短的注视点 - 没有获取到视觉信息；
* 时间过长的注视点 - 注视溢出无效
* 没有落在任何兴趣区中的注视点 - 没有获取到视觉信息；

这就是 4-Stage Fixation Cleaning 所做的事情。

# 2. 执行 4-Stage Fixation Cleaning

我们来看一下如何在 DV 中进行 4-Stage Fixation Cleaning。

导入数据之后，选中 Inspector 中选择总父分支，即保存的 .evs 文件的名称。

![dv-perform_4_stage_fix_cleaning](/assets/images/dv-perform_4_stage_fix_cleaning.png)

> 如果你愿意尝试一下，你会发现点击了下面的子分支也会有 4-Stage Fixation Cleaning 的选项。我们右键了哪个分支，就是我们要对哪些数据进行数据清洗。一般情况下，我们需要对所有数据进行清洗，所以直接点击 .evs 文件的总分枝即可。

随后我们可以看到如下的执行窗口，点击绿色方框中的 `?` 按钮可以查看 4-Stage Fixation Cleaning 的说明。

![dv-4_stage_fix_cleaning_window](/assets/images/dv-4_stage_fix_cleaning_window.png)

![dv-4_stage_fix_cleaning_help](/assets/images/dv-4_stage_fix_cleaning_help.png)

我们来逐个拆解。

## 2.1 Stage 1 删除/合并 时间过短的注视点

![dv-4_stage_fix_cleaning_stage_1](/assets/images/dv-4_stage_fix_cleaning_stage_1.png)

> STAGE 1. Within each interest area (typically a word in reading studies), Data Viewer checks whether each fixation's duration is shorter or equal to the Stage 1 Duration Threshold. For those short fixations, the software further checks the duration and distance of the fixations immediately before and after the current fixation. The fixation will be merged to one of its neighboring fixations if the neighbour’s duration is longer than the threshold value and its distance along the x-axis (in degrees) from the current fixation is shorter or equal to the Stage 1 Distance Threshold. If both the previous and next fixations meet the above criteria, the current fixation will be merged to the longer of the two. Uncheck the “Stage 1” box to skip this stage.

Stage 1 有两个参数，分别为 Duration Threshold 和 Distance Threshold。

DV 会依次对每个注视点进行检测，首先检测每个注视点的持续时间是否超过 Duration Threshold（ Stage 1 的默认设置中为 80ms）。如果没有超过 Duration Threshold，则会检测这个注视点与其前一个或者后一个注视点在 x 方向（水平）上的距离是否小于等于 Distance Threshold（ Stage 1 的默认设置中为 0.5度）。

如果找到了临近的注视点，则将这个持续时间很短的注视点就近合并到邻近的注视点中；如果没有，则删除这个时间过短的注视。

我们也可以取消 Stage 1 的勾选来跳过这个阶段的数据清洗。

## 2.2 Stage 2 删除/合并 时间过短的注视点

![dv-4_stage_fix_cleaning_stage_2](/assets/images/dv-4_stage_fix_cleaning_stage_2.png)


> STAGE 2. This stage is similar to STAGE 1 except that different fixation Duration and Distance Threshold values are used. For this stage to be effective, a shorter duration and a larger distance threshold should be used compared to Stage 1. Uncheck the “Stage 2” box to skip this stage.

Stage 2 和 Stage 1 的执行逻辑是完全相同的，只不过是阈值设置相对于 Stage 1 有所变化。

由于在 Stage 1 和 2 中需要先检测注视的持续时间，所以如果已经执行过 Stage 1 的话，Stage 2 将无法筛选出任何注视，所以一般 Stage 1 和 Stage 2只会选择一个执行。
{: .notice--info}

我们也可以取消 Stage 2 的勾选来跳过这个阶段的数据清洗。

## 2.3 Stage 3 合并 时间过短的注视点

![dv-4_stage_fix_cleaning_stage_3](/assets/images/dv-4_stage_fix_cleaning_stage_3.png)

> STAGE 3. Data Viewer searches for interest areas that include at least three fixations shorter than the Stage 3 Duration Threshold value and no fixations longer than the Duration Threshold. In such cases the shorter fixations are merged into a single fixation. Uncheck the "Stage 3" box to skip this stage.

在 Stage 3 的过程中，DV 会依次对每个兴趣区进行筛查。检查每个兴趣区中是否至少有 3 个持续时间短于 Duration Threshold（默认 140ms）的注视点。

如果有的话，这些持续时间较短的注视点将被合并为一个持续时间较长的注视点。

取消 Stage 3 的勾选可以跳过这个阶段的数据清洗。

## 2.4 Stage 4 删除 时间过短/过长的注视点

![dv-4_stage_fix_cleaning_stage_4](/assets/images/dv-4_stage_fix_cleaning_stage_4.png)

> STAGE 4. Data Viewer deletes every fixation shorter than or equal to the Stage 4 Minimum Duration threshold, or longer than or equal to the Stage 4 Maximum Duration threshold. Uncheck the "Stage 4" box to skip this stage.

在 Stage 4 中，DV 会依次检测每个注视的持续时间，是否小于等于 Minimum Duration Threshold（默认 140ms） 或 大于等于 Maximum Duration Threshold（默认 800ms）。

如果满足筛选条件，则删除这个注视点。

取消 Stage 4 的勾选可以跳过这个阶段的数据清洗。

## 2.5 Stage appended 删除兴趣区外的注视点

![dv-4_stage_fix_cleaning_stage_append](/assets/images/dv-4_stage_fix_cleaning_stage_append.png)

> If the "Delete Fixations Outside Interest Areas" box is checked, this will remove all fixations falling outside of any interest area. **Important:** If no interest area is defined, checking this option will result in all fixations being removed!

最后，如果我们勾选了这个附加选项，则 DV 会删除没有落在任何兴趣区内的注视点。

**注意！**如果还没画兴趣区的话，这个操作会删除全部的注视点。如果一不小心这样错误删除了，就只能重新导入数据了。
{: .notice--warning}

# 3. 4-Stage Fixation Cleaning Log

点击 `Start`，DV 会自动完成数据清洗。

![dv-4_stage_fix_cleaning-cleaning_log](/assets/images/dv-4_stage_fix_cleaning-cleaning_log.png)

随后，我们可以在 Output 文件夹中看到清洗的日志文件 cleaning.txt，截取其部分内容如下：

```
Data Cleaning started Thu Mar 18 00:13:59 CST 2021
Cleaning EDF: dm Trial: 1
Starting Stage 1 Right Eye
Starting Stage 2 Right Eye
Starting Stage 3 Right Eye
Starting Stage 4 Right Eye
S4 DELETING: Fixation: 29933 ms
S4 DELETING: Fixation: 29494 ms
S4 DELETING: Fixation: 27171 ms
S4 DELETING: Fixation: 25098 ms
S4 DELETING: Fixation: 22230 ms
S4 DELETING: Fixation: 21725 ms
S4 DELETING: Fixation: 18496 ms
S4 DELETING: Fixation: 14111 ms
S4 DELETING: Fixation: 8730 ms
S4 DELETING: Fixation: 7938 ms
S4 DELETING: Fixation: 7794 ms
S4 DELETING: Fixation: 6707 ms
Non IA Left Eye
S4 DELETING NON IA: Fixation: 7 ms
Cleaning EDF: dm Trial: 2
Starting Stage 1 Right Eye
Starting Stage 2 Right Eye
Starting Stage 3 Right Eye
Starting Stage 4 Right Eye
S4 DELETING: Fixation: 34755 ms
S4 DELETING: Fixation: 33356 ms
S4 DELETING: Fixation: 30584 ms
S4 DELETING: Fixation: 26012 ms
S4 DELETING: Fixation: 22149 ms
S4 DELETING: Fixation: 18880 ms
S4 DELETING: Fixation: 18797 ms
S4 DELETING: Fixation: 16157 ms
S4 DELETING: Fixation: 12657 ms
S4 DELETING: Fixation: 11941 ms
S4 DELETING: Fixation: 11588 ms
S4 DELETING: Fixation: 8749 ms
S4 DELETING: Fixation: 5777 ms
S4 DELETING: Fixation: 3409 ms
Non IA Left Eye
```

此日志文件中记录了 **每个被试、每个试次、每只眼睛、每个 Stage** 具体删除了 **哪个注视点**。

一般来说，如果执行了 4-Stage Fixation Cleaning，那么我们需要在论文中报告了删除了多少比例的注视点。

为了方便大家计数，Charlie 写了一个小工具送给大家：

[>>>Cleaning Counter - v0.1 （密码：ckjs）<<<](https://pan.baidu.com/s/1aqNcy_j7NA1M8uAFRJXUeA)

![dv-4_stage_fix_cleaning-cleaning_counter](/assets/images/dv-4_stage_fix_cleaning-cleaning_counter.png)

随后在 Output 文件夹中会生成名为 “clean_count.xls” 的表格文件。

![dv-4_stage_fix_cleaning-counter_result](/assets/images/dv-4_stage_fix_cleaning-counter_result.png)

我们可以看到，已经根据被试和试次进行了自动计数。
 
---

以上。