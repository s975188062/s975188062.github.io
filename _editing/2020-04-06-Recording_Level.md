---
title: "Recording层"
categories:
  - Eyelink
tags:
  - Experiment Builder
  - Datasource
  - 引用
  - Test Run
  - Message
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

# 1. 构建单试次结构

本阶段中用到的所有控件其实已经在之前的部分介绍过了。我在这里会做一个最基本的叙述作为复习。

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

## 1.2 Keyboard_of_TP

* 添加一个新的`Keyboard Trigger`，连接箭头，修改`Label`为“Keyboard_of_TP”。设置触发按键为`Space`。

![eb_add_key_TP](/assets/images/eb_add_key_TP.gif)

## 1.3 Display_Question

* 添加一个新的`DisplayScreen Action`空间，连接箭头，修改名称为“Display_Question”。

* 在`Screen Builder`中添加`MultiLine Text Resource`，设置`Margin`，添加文本。

* 设置`黑色 - 15号 - Courier New - 竖直居中对齐`。

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

## 3.1 建立Datasource

## 3.2 对刺激材料设置引用

## 3.3 再次Test Run

---

# 4. 添加Message

---

以上。 




