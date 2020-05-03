---
title: "计算反应时和正确率"
excerpt: "EB Basic系列的行为信息记录，主要涉及Variable、Update Attribute和Conditional三个控件的用法。"
read_time: false
header:
  overlay_image: /assets/images/eb_calcuate_rt_n_acc_header.png
  overlay_filter: 0.5
categories:
  - Eyelink
tags:
  - Experiment Builder
  - Variable
  - Update Attribute
  - Conditional Trigger
toc: true
comments: true
author_profile: false
sidebar:
  title: "目录"
  nav: EB
---

---

# 1. 计算反应时

有机体对刺激的反应并不能在受到刺激的同时就发生，从刺激的呈现到反应的开始之间会有一段时间间距。反应时间（reaction time, RT）就是指从刺激呈现到有机体做出明显的反应所需要的时间。

## 1.1 执行逻辑

我们会通过一个非常简单的算式来计算各种各样的行为指标，以`TextPage`的阅读用时为例：

假设`Text Page`在第3秒时呈现，被试在第5秒时按下“空格”结束阅读，那么`TextPage`的阅读的阅读时间为 5 - 3 = 2秒，即“阅读用时 = 被试按键的时间 - 刺激出现的时间”。

熟悉编程的人会了解如何通过代码定义和使用变量，大概的过程是：

`定义一个名为“XX”的对象` -> `声明“XX”的类型` -> `对”XX“进行赋值`

## 1.2 计算Text Page阅读用时

### 1.2.1 定义一个名为“RT_of_TextPage”的`Variable`

首先，我们将一个`Variable`控件从控件库中拖入工作区。

![eb_add_variable_component](/assets/images/eb_add_variable_component.png)

修改`Label`属性为“RT_of_TextPage”。

![eb_set_rt_textpage_label](/assets/images/eb_set_rt_textpage_label.png)

### 1.2.2 声明`RT_of_TextPage`的变量类型

常用的变量类型有：

| 变量类型 | 解释 | 声明Value  |
| --- | --- | --- |
| String | 字符型（文字） | a |
| Integer | 整型（整数） | 0 |
| Double | 双浮点型（小数） | 0.0 |
| Boolean | 布尔型（逻辑是/否） | true |
| List | 列表（数组矩阵） | [] |

阅读`TextPage`的时间`RT_of_TextPage`大概是“xx.xx毫秒”，所以`RT_of_TextPage`的数据类型为“Double”。所以我们应当声明`RT_of_TextPage`的变量类型为“Double”。

![eb_init_rt_of_textpage](/assets/images/eb_init_rt_of_textpage.png)

在`Variable`的`Value`属性中输入什么类型的值，就可以将这个入`Variable`声明为什么类型的变量。因此，输入在`RT_of_TextPage`的`Value`属性中输入“0.0”即可声明`RT_of_TextPage`的类型为“Double”。

### 1.2.3 对`RT_of_TextPage`进行赋值计算

对`RT_of_TextPage`进行赋值计算我们需要用到一个新的控件——`Update Attribute`。

![eb_add_update_attribute_component](/assets/images/eb_add_update_attribute_component.png)

首先拖拽一个`Update_Attribute`控件到`Recording`的工作区，更名为“Calculate_RT”。并将其连接到实验序列的最后。

![eb_add_update_attribute_component_to_graph](/assets/images/eb_add_update_attribute_component_to_graph.png)

双击`Calculate_RT`打开`Attribute-Value List for Calculate_RT窗口`，进行详细的赋值设置。

![eb_update_attribute_window](/assets/images/eb_update_attribute_window.png)

我们可以看到，`Attribute-Value List for Calculate_RT窗口`分为左右两列，分别为”Attribute“和“Value”。在这个窗口中，我们可以将右边“Value”中填写的内容赋值给左边“Attribute”中所指向的变量。

已知`RT_of_TextPage` = `Time_of_Keyboard_of_TextPage_Triggered` - `Time_of_TextPage_Displayed`。

即我们希望将“`Time_of_Keyboard_of_TextPage_Triggered` - `Time_of_TextPage_Displayed`”的值赋给`RT_of_TextPage`。所以我们进行如下操作：

首先设置左侧“Attribute”，在列表中找到`RT_of_TextPage`，选中“Value”。意为对`RT_of_TextPage`进行赋值。

![eb_calculate_rt_of_TP_attribute](/assets/images/eb_calculate_rt_of_TP_attribute.gif)

然后设置右边，因为是两个数值相减，所以要写成算式：

![eb_calculate_rt_of_TP_value](/assets/images/eb_calculate_rt_of_TP_value.gif)

>**⚠️注意⚠️**：所有的Trigger都有两个Time，第一个Time是Trigger开始等待被触发的时间，第二个在Trigger Data下面的Time才是被触发时的时间。大多数的时候我们都是使用Trigger Data里面的Time做计算。

## 1.3 计算Question反应时

同前一部分类似，用相同的方法计算`Question`屏的回答反应时。

`RT_of_Question` = `Time_of_Keyboard_of_Question_Triggered` - `Time_of_Question_Displayed`。

定义一个名为`RT_of_Question`的`Variable`并声明变量类型为“Double”。

![eb_def_RE_of_Question](/assets/images/eb_def_RE_of_Question.png)

双击`Calculate_RT`打开`Attribute-Value List for Calculate_RT窗口`，对`RT_of_Question`进行赋值运算。

![eb_calculate_rt_of_question](/assets/images/eb_calculate_rt_of_question.png)

---

# 2. 记录Question回答结果

