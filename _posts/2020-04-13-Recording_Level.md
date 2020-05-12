---
title: "Recording层"
excerpt: "EB Basic系列的Recording层设置方法，涉及建立Datasource、设置引用/Reference、试运行/TestRun和添加Message等。"
categories:
  - Eyelink
tags:
  - Experiment Builder
  - Datasource
  - 引用
  - Test Run
  - Message
  - Sequence
toc: true
comments: true
author_profile: false
sidebar:
  title: "目录"
  nav: EB
---

---

> 实验设计回顾
> 
> ![eb_exp_design_record_level](/assets/images/eb_exp_design_record_level.png)
> 
> 本节中我们将编辑红色选框选中的部分。
> 
> 首先，我们将在第一部分中构建一个只呈现1篇文章实验。随后我们将在第二部分通过`Datasource`和`引用`构建出多个试次的循环。

---

# 1. 构建单个试次结构

本阶段中用到的所有控件其实已经在之前的部分介绍过了。我在这里会做一个最基本的叙述作为复习。

<div class="notice">
  <h4>⚠️注意⚠️：</h4>
  <p>A basic message.</p>
</div>{: .notice--info}

## 1.1 Display_TextPage

* 添加一个新的`DisplayScreen Action`空间，连接箭头，修改名称为“Display_TextPage”。

![eb_add_dsiplay_textpage](/assets/images/eb_add_dsiplay_textpage.gif)

* 修改`Background Color`为中性灰`128, 128, 128, 255`。

![eb_set_textpage_bgcolor](/assets/images/eb_set_textpage_bgcolor.png)

* 打开`Display_TextPage`的`Screen Builder`，添加一个`MultiLine_Text_Resource`。

![eb_textpage_add_multiline_text_resource](/assets/images/eb_textpage_add_multiline_text_resource.gif)

* 设置`MultiLine_Text_Resource`的`Margin`，编辑文本内容，设置文字为`黑色 - 15号 - Courier New - Double行距`。

![eb_textpage_edit_text](/assets/images/eb_textpage_edit_text.gif)

* 关闭`MultiLine_Text_Resource`窗口完成编辑。

> **⚠️注意⚠️**：我们需要额外设置`Use For Host Display`属性为`True`，以配合`Trial层`中的`Prepare Sequence`的`Draw to Eyelink Host`设置[（传送门）](/eyelink/Trial_Level/#12-将实验材料等信息发送到主试机上以便监控实验)。

## 1.2 Keyboard_of_TP

* 添加一个新的`Keyboard Trigger`，连接箭头，修改`Label`为“Keyboard_of_TP”。设置触发按键为`Space`。

![eb_add_key_TP](/assets/images/eb_add_key_TP.gif)

## 1.3 Display_Question

* 添加一个新的`DisplayScreen Action`空间，连接箭头，修改名称为“Display_Question”。

* 在`Screen Builder`中添加`MultiLine Text Resource`，设置`Margin`，添加文本。

* 设置`黑色 - 15号 - Courier New`。

![eb_add_display_q](/assets/images/eb_add_display_q.gif)

> ⬆️此处是操作动图，文件较大可能加载缓慢。

## 1.4 Keyboard_of_Question

* 添加一个新的`Keyboard Trigger`，连接箭头，修改`Label`为“Keyboard_of_TP”。

* 此处我们的设计是让被试判断问题的对错，如果问题是对的，则按`f`键，反之则按`j`键。因此我们设置`Keyboard_of_Question`的触发按键为`f`或`j`。

>设置按键多选时可以直接使用`Ctrl`或者`Shift`进行多选。

![eb_set_keyboard_q_keys](/assets/images/eb_set_keyboard_q_keys.gif)

至此，单一试次任务编写完成。

---

# 2. 测试程序

在完成但一试次任务编写之后，我们可以试运行我们的实验，以此来检测整个实验流程是否是按照我们的想法进行的。

这里要用到`Experiment Builder`的`Test Run`功能，你可以在工具栏中找到它。

![eb_testrun_in_toolbar](/assets/images/eb_testrun_in_toolbar.png)

无论是否连接眼动仪，我们都可以通过点击`Test Run`按钮来试运行程序。

* 连接眼动仪 - 点击`Test Run`直接运行即可；
* 未连接眼动仪 - 开启`Dummy Mode`后试运行实验。

很多时候我们都不能连接眼动仪来调试设备，那么`Dummy Mode`的存在就非常有意义了。在`Dummy Mode`下，可以不连接眼动仪、直接跳过眼动仪校准等环节运行试验。

当然这样做的代价是，在整个实验的运行过程中，和眼动相关的很多触发器等功能都无法使用了。

## 2.1 设置Dummy Mode

首先设置`Dummy Mode`，如果您已经连接了眼动仪，请跳过此步骤直接进行`2.2 Test Run试运行`。。

在`Structure面板`中选中`Devices`标签页，点击`Eyelink`，在其`Properties`面板中即可找到`Dummy Mode`属性。

![eb_set_dummymode](/assets/images/eb_set_dummymode.png)

## 2.2 Test Run试运行

点击`工具栏`中的`Test Run`开始试运行。

![eb_testrun_in_toolbar](/assets/images/eb_testrun_in_toolbar.png)

首先会出现第一个提示，告知`Test Run`会清空现存的所有数据，即之前`Test Run`时采集的数据。

![eb_testrun_1_confrim](/assets/images/eb_testrun_1_confrim.png)

在这里强调一下，`Test Run`仅仅适用于试运行程序，**不能用于正式的数据采集**。

之后会和正常运行程序一样，出现第二个提示窗口，输入`.edf`文件的名称。

![eb_testrun_creat_edf](/assets/images/eb_testrun_creat_edf.png)

然后是最后一个提示框，告知`Test Run`模式下部分眼动相关的功能可能会失效。

![eb_testrun_2_confrim](/assets/images/eb_testrun_2_confrim.png)

整个过程如下图所示（可能加载缓慢）：

![eb_testrun](/assets/images/eb_testrun.gif)

---

# 3. Datasource与引用

在前面的工作中，我们已经构建好了**单试次**的实验，只呈现了一篇文章和一个问题。

实验设计中共计划呈现4篇文章和4个问题，剩下的三个要怎么办呢？复制粘贴也许是个能解决问题的办法，但是这不是“正宗”的方案。

在这里我们要引入一个新的概念——`Datasource`。

`Datasource`实际上是一个表格，以当前实验任务为例：一共四个试次，每个试次的结构都是一样的，即“呈现文章 - 看完按空格 - 呈现问题 - 按f/j作答“。每个试次运行的时候都是套用这样一个格式，区别在于每次呈现的文章和问题不同。

我们将每个试次之间不同的部分放到一个表格里面，套用相同的流程格式，就成功创建了许多个试次。而这个表格就叫做`Datasource`，如下表所示：

| Trial ID | Text Page             | Question                         | Correct Answer |
|:---------|:----------------------|:---------------------------------|:---------------|
| 1        | Buck did not read...  | The heavy dogs are popular.      | f              |
| 2        | The house was...      | There are 13 boys here in total. | f              |
| 3        | They came and...      | The fox terriers never yelp.     | j              |
| 4        | Among the terriers... | Elmo weight 100 pounds.          | j              |

* `Trial ID`为**1**的试次所呈现的文章是：“Buck did not read...”，被试按空格后所呈现的问题是：“The heavy dogs are popular.”，这道题的正确答案是“f”。

* `Trial ID`为**2**的试次所呈现的文章是：“The house was...”，被试按空格后所呈现的问题是：“There are 13 boys here in total.”，这道题的正确答案是“f”。

* 以此类推……

## 3.1 建立Datasource

我们如何将这个表格移植到我们的实验程序中呢？

熟悉E-Prime的朋友知道E-Prime的List功能，实际上`Datasource`的逻辑也是类似的。

* `Datasource`实际上是`Sequence`的一个属性，`Sequence`在默认情况下是没有`Datasource`的。

一旦我们给`Sequence`添加了`Datasource`属性，那么`Sequence`也就开启了循环功能，`Datasource`的表格有几行，`Sequence`就会被执行几次。

在先前的实验编程中，我们已经设置了三个`Sequence`——`Block`、`Trial`和`Recording`。那应该让哪个`Sequence`循环呢？

答案很简单，让`Trial`循环。所以我们要把`Datasource`创建到`Trail`下面。

选择`Trial层`的`Sequence`，双击`Properties窗口`的`Data Source`属性。

![eb_open_datasource](/assets/images/eb_open_datasource.gif)

### 3.1.1 Add Column

点击`Add Column`，输入`Column Name`，根据情况设置`Column Type`，点击`OK`即可。

![eb_ds_add_column](/assets/images/eb_ds_add_column.png)

成功添加`Trial_ID`。

![eb_ds_1](/assets/images/eb_ds_1.png)

继续添加其他`Column`。

![eb_ds_2](/assets/images/eb_ds_2.png)

### 3.1.2 Add Row

完成`Column`的添加之后我们来添加行。点击`Add Row`，添加4行。

![eb_ds_add_row](/assets/images/eb_ds_add_row.png)

完成添加。

![eb_ds_3](/assets/images/eb_ds_3.png)

> **⚠️注意⚠️**：总的行数需要和`Datasource`的实际行数完全一致，不能留有空行，否则会报错。

### 3.1.3 添加内容

把准备好的`Datasource`内容复制粘贴进来即可。

![eb_ds_paste](/assets/images/eb_ds_paste.gif)

> **⚠️注意⚠️**：下图中`绿框`和`红框`中的数字虽然一样，但是`绿框`仅代表行的编号。而`红框`中的数字，即`Trial_ID`的值，则是给每个条件的试次一个单独的编号，用作后面的数据分析与统计。

## 3.2 对刺激材料设置引用

`引用`，英文叫做`Reference`，通过`引用`可以让某些内容或者属性动态变化。

此处我们要对随着试次变化的“阅读材料”和“问题”设置引用，引用到对应的`Datasource`上。

### 3.2.1 设置Text_Page的引用（多行文本引用）

打开`Display_TextPage`中的`MultiLine Text Resource Editor`。选中全部文字，点击工具栏中的`Add Reference`按钮。选中左侧列表中`Trial`下面的`Datasource`，此时中间的列表中就回出现`Datasource`中所有`Column`的名称。双击`Text_Page`，看到上方的输入框变为“@parent.parent.parent.Trial_DataSource.Text_Page@”则成功引用，点击`OK`即可。成功引用后，文字的内容将会变成`@`。

![eb_sb_add_ref_mtl](/assets/images/eb_sb_add_ref_mtl.gif)

>注意事项：
>
> * `Datasource`的位置是在`Trial层`下面，因为`Datasource`是`Trial层`这个`Sequence`的一个属性，由于`Datasource`的存在，`Trial`才会重复执行。如果`Datasource`没有再`Trial`下面的话，就说明建立错了位置。
> 
> ![eb_ref_ds_loc](/assets/images/eb_ref_ds_loc.png)
> 
> * 在进行`Reference`操作时，**双击**目标才是正确的引用方法。

完成引用后，我们可以使用预览功能来查看`Reference`的效果。也可以在点击`Preview`之后按`《`和`》`逐一切换。

![eb_mlt_preview_ref](/assets/images/eb_mlt_preview_ref.png)

![eb_sb_ref_preview](/assets/images/eb_sb_ref_preview.gif)

### 3.2.2 设置Question的引用

使用同样的方法对`Question`屏的问题文字进行引用。详细解释略。

![eb_sb_add_ref_mtl_2](/assets/images/eb_sb_add_ref_mtl_2.gif)

## 3.3 再次Test Run

再次执行`Test Run`，我们就可以看到四个试次了。

---

# 4. 添加Message

`Message`类似于脑电数据中的Marker，是一种数据标记。

在`Recording层`中，我们先是呈现了`Text Page`，接着呈现了`Question`。在这个过程中，眼动数据是一直在记录的。如果不在眼动数据中添加一些特殊标记，眼动仪就不知道什么时候看的是`Text Page`，什么时候看的是`Question`。

`Message`就是标记许多特殊事件的标记，内容通常是一些英文字符。

例如用“Text Page Start”、“Text Page End”来标记`Text Page`的开始和结束。

几乎每一个控件都会有一个`Message`属性，在这个控件生效的时候，会将`Message`的内容发送到`.edf文件`中，作为标记。所以我们要将特定的文本内容添加到特定控件的`Message`属性中。

![eb_message_def](/assets/images/eb_message_def.png)

此处我举例设置`Display_TextPage`的`Message`。

![eb_set_display_textpage_message](/assets/images/eb_set_display_textpage_message.png)

完成全部`Message`设置示例如下：

![eb_set_recording_message](/assets/images/eb_set_recording_message.png)

截止目前，完成了非随机的全试次编程。

---

`Test Run`一下试试效果吧！

![eb_show_test_run](/assets/images/eb_show_test_run.png)

以上。 




