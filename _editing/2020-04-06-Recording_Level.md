---
title: "Recording层"
categories:
  - Eyelink
tags:
  - Experiment Builder
  - Datasource
  - 引用
  - Test Run
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

至此，单一试次编写完成。

---

# 2. 测试程序

## 2.1 设置Dummy Mode

## 2.2 Test Run试运行

---

# 3. Datasource与引用

## 3.1 添加Datasource

## 3.2 对刺激材料设置引用


---

以上。 




