---
title: "反应时和正确率的计算与记录"
excerpt: "EB Basic系列的行为信息记录，主要涉及Variable、Update Attribute、Conditional、ResultsFile和AddToResultsFile五个控件的用法。"
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
  - Result File
  - Add to Result File
toc: true
comments: true
author_profile: false
sidebar:
  title: "目录"
  nav: EB
---

---

> 2020年8月14日 更新
> 
> 近日收到多位老师反应，刺激屏幕的按键存在延迟，按键后不会立即显示下一屏的内容。借此机会向大家解释一下问题的原因，顺便加深大家对 DisplayScreen 这个控件的理解。
> 
> 您可以点击下面的传送门快速导航到更新的内容，但请保证在**完全掌握**本节内容的前提下再去查看下面传送门的内容。
> 
> [>>>传送门<<<](/eyelink/calcuate_rt_n_acc-supplement_20200814/)


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

**⚠️注意⚠️**：所有的Trigger都有两个Time，第一个Time是Trigger开始等待被触发的时间，第二个在Trigger Data下面的Time才是被触发时的时间。大多数的时候我们都是使用Trigger Data里面的Time做计算。
{: .notice--warning}

## 1.3 计算Question反应时

同前一部分类似，用相同的方法计算`Question`屏的回答反应时。

`RT_of_Question` = `Time_of_Keyboard_of_Question_Triggered` - `Time_of_Question_Displayed`。

定义一个名为`RT_of_Question`的`Variable`并声明变量类型为“Double”。

![eb_def_RE_of_Question](/assets/images/eb_def_RE_of_Question.png)

双击`Calculate_RT`打开`Attribute-Value List for Calculate_RT窗口`，对`RT_of_Question`进行赋值运算。

![eb_calculate_rt_of_question](/assets/images/eb_calculate_rt_of_question.png)

---

# 2. 记录Question回答结果

还是先说逻辑，记录回答结果的思路有很多：

* 第一种是直接记录被在作答时的按键是什么，直接记录“f”或者“j”。这种记录方法在编程时的操作相对简单，但是在数据处理的时候就需要对每个试次的答案进行正确比对，后期需要进行一些简单但是机械费时的工作；
* 第二种方法则是在实验运行过程中让计算机代替手动，直接根据已知的正确答案判断每道题的回答是否正确。在处理数据的时候则友好很多。
* ……

本文中我们将采用第二种方法来进行回答结果的记录。大致流程如下：

`判断回答的对错` -> `记录对错信息` -> `保存到Result File`

## 2.1 判断回答的对错

这里要用到一个控件`Conditional Trigger`。首先我们先将一个`Conditional Trigger`添加到工作区中，修改`Label属性`为“Judge”。并连接到实验序列的最后。

![eb_add_conditional](/assets/images/eb_add_conditional.png)

`Conditional Trigger`这个空间其实就是一个判断器。在使用时，我们会实现在`Conditional Trigger`中预置一个条件，这个条件可能是一个等式，也可能是一个包含的逻辑关系。如果我们提前预置的这个逻辑关系成立，则从✔︎的一侧向后执行，反之则从×的一侧向下执行。如下图所示：

![eb_conditional_explain](/assets/images/eb_conditional_explain.png)

我们首先要在`Judge`里面预置逻辑关系表达式，通过这个逻辑关系表达式来判断回答是否正确。详细解释一下则是将`Keyboard_of_Question`中的按键（f/j）与`Datasource`中已经预置的正确答案做对比，如果相同则回答正确，不相同则回答错误。所以我们预置的逻辑关系表达式是：

`@Keyboard_of_Question.TriggeredData.Key@` == `@Trail_Datasource.Correct_Answer@`

选中`Judge`，修改`Evaluation 1`中的`Attribute`为在`Keyboard_of_Question`中的按键，修改`Evaluation 1`中的`Value`为`Datasource`中的“Correct_Answer”这个Column。

![eb_config_judge_properties](/assets/images/eb_config_judge_properties.gif)

设置完成后应如下图所示：

![eb_set_conditional_judge_answer](/assets/images/eb_set_conditional_judge_answer.png)

## 2.2 记录对错信息

在2.1的部分我们已经完成了对于对错的识别，在这一部分我们将完成对错的记录。

首先我们创建一个名为“ACC“的`Variable`。如果回答正确的话，我们让`ACC`等于1，如果回答错误的话，我们让`ACC`等于0。

![eb_def_acc](/assets/images/eb_def_acc.png)

> 注意，在声明`ACC`变量类型的时候，我没有用0，而是用了2。这是因为0代表错误，1代表正确，初始化的时候应使用不同于前两者的数字来进行初始化声明，目的是防止出现回答超时等特殊情况来进行标记。

在`Judge`分别链接两个`UpdateAttribute`控件——`Mistake`和`Correct`，如果在`Judge`中判断回答正确，执行`Correct`让`ACC`等于1，如果回答错误，则执行`Mistake`让`ACC`等于0。

![eb_config_acc](/assets/images/eb_config_acc.png)

## 2.3 保存到Result File

这一部分我们将使用两个新的控件：`ResultsFile`和`AddToResultsFile`。

![eb_show_resultsfile_n_add2resultsfile](/assets/images/eb_show_resultsfile_n_add2resultsfile.png)

`ResultsFile`在控件库的Other分类下，只需将`ResultsFile`拖入工作区，不用链接任何控件，即可命令EB创建一个.txt格式的Results File。在这个Results File中我们可以保存每个试次的运行条件和行为反馈等信息，例如我门在本章前一部分介绍的反应时和正确率。

`AddToResultsFile`则是命令EB软件将我们指定的信息写入Results File。

> `Results File`的功能是为了non-Eyelink实验准本的，因为非眼动实验不会生成`.edf`文件，数据无法保存在`.edf`文件当中，就只能新建一个`.txt`的结果文件来保存这些信息。在实际使用过程中，有些用户发现用`Results File`的功能来记录眼动数据中的行为信息也很方便，于是就这样延续了下来。

所以我门首先将一个`ResultsFile`控件添加至工作区。`ResultsFile`的`Label`属性即为`.txt`文件的文件名，可以直接使用默认值。

随后我们向工作区中添加一个`AddToResultsFile`控件，保持默认的`Label`，将其添加至实验序列的末端。

![eb_add_add2resultsfile](/assets/images/eb_add_add2resultsfile.png)

选中`AddToResultsFile`，修改其`Results File`属性为刚刚添加的`ResultsFile`。

![eb_config_add2resultsfile_resultsfile](/assets/images/eb_config_add2resultsfile_resultsfile.png)

这样在运行试验之后就会在`.edf`文件相同的路径下生成一个制表符构成的`.txt`结果文件，可以直接用excl读取。具体`ResultsFile`中包含的信息内容有哪些，可以在`ResultsFile`控件中的`Columns`属性中查看。

![eb_reslutsfile_contains](/assets/images/eb_reslutsfile_contains.png)

---

`Test Run`一下试试效果吧！

![eb_show_test_run](/assets/images/eb_show_test_run.png)

以上。

