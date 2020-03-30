---
title: "Experiment层"
categories:
  - Eyelink
tags:
  - Experiment Builder
toc: true
comments: true
author_profile: false
sidebar:
  title: "目录"
  nav: EB
---

---

正如我在上一讲中所提到的，在`Experiment Builder`中编写实验程序需要依照下图的层级关系构建实验结构。

本文的内容将围绕如何构建`Experiment`层级展开。

![eb_hierarchical_org](/assets/images/eb_hierarchical_org.jpg)

新建Project，命名为`my_first_exp`。

---

# 0. 配置硬件设备的基本信息

> 如果您准备在自己的电脑上编写实验然后放到其他电脑上进行试验的话，本节内容或许对您有所帮助。
> 
> 如果您仅仅是初学`Experiment Builder`的话，您可以跳过本节内容，直接阅读`1. 指导语`部分。

当我们在`Experiment Builder`中创建实验时，软件会自动读取当前电脑的硬件参数，并且把这些参数用作当前项目的后台设置。

在诸多参数中，我们需要注意`Display Resolution`这个参数。这个参数会决定编辑`DisplayScreen Action`过程中屏幕的工作区大小。如果没有在一开始设置好的话，会给我们后来的排版带来很大麻烦。

设置显示器分辨率的方法如下图所示：

![eb_config_screen_resolution](/assets/images/eb_config_screen_resolution.png)

* **Width** - 显示器宽边的像素值
* **Height** - 显示器短边的像素值
* **Bits Per Pixel** - 位深度（可忽略，默认即可）
* **Refresh Rate** - 显示器的刷新率

我们应当按照实验室电脑的参数来设置这些属性。但应当注意的是，如果您自己的电脑相关配置达不到实验室电脑的水平，将无法进行`Test Run`来试运行实验。

---

# 1. 指导语

指导语内容：

>你好，欢迎参加本实验！
>
>在本实验中，您将阅读四段英文材料，完成阅读后请按空格结束当前文章的浏览。在每一段文章的浏览结束后，都会有一个关于文章的问题。如果问题的答案是“是”，则按“f”键；如果答案是“否”，则按“j”键。
>
>了解实验内容之后，请按“空格”开始。

关于指导语，我们通过两个控件完成设置：

* `DispalyScreen Action` - 呈现指导语
* `Keyboard Trigger` - 确认指导语

![eb_1st_exp_greeting](/assets/images/eb_1st_exp_greeting.png)

那么我们首先在`控件库`中找到`DisplayScreen Action`这个控件。用鼠标左键点中它，按住鼠标左键不放，拖拽到工作区中再松开，就成功地添加到工作区中了。如下图所示：

![2020-03-28 02.49.14](/assets/images/eb_add_a_node.gif)

然后我们选中新添加进来的`Display_Screen`，此时控件会增加绿色的阴影，如下图所示：

![eb_Choosed_n_notChoosed](/assets/images/eb_Choosed_n_notChoosed.png)

选中`Display_Screen`后，到`Prperties面板`中修改`Label`属性，更改为“Greeting”。如下图所示：

![eb_edit_node_labe](/assets/images/eb_edit_node_label.gif)

随后，我们将`Start`和`Greeting`连接起来。在`Start`未被选中的情况下，用鼠标左键点击`Start`，按下去之后不要松开，移动到`Greeting`上之后再松开。这样就从`Start`指向了`Greeting`。如下图所示：

![eb_add_connection](/assets/images/eb_add_connection.gif)

>在选中条件下对控件执行按住鼠标左键拖动的操作是移动位置。

`Start`就是整个实验的开始，开始后先呈现指导语。那么我们来继续编辑指导语的内容。双击`Greeting`进入到显示编辑器。如下图所示，`工作区标签`将会增加一个叫做`Greeting`的标签。

![eb_open_screen_builde](/assets/images/eb_open_screen_builder.gif)

>我们目前打开的这个小窗口叫做`Screen Builder`，即呈现内容的编辑器。我们可以向`Screen Builder`中添加很多的`Resource`，中文称之为素材。这些素材的添加按钮都在`Screen Bulider`的工具栏中，如下图所示：
>
>![eb_screen_builder_toolba](/assets/images/eb_screen_builder_toolbar.png)
>
>可添加的素材种类有：`图片`、`视频`、`单行文本框`、`多行文本框`、`直线`、`矩形`、`椭圆形`、`三角形`和`不规则图形`。
>
>在指导语的编辑中，我们将只对`多行文本框`进行介绍，其余部分将分散到后面的章节中逐一介绍。

## 1.1 添加多行文本

目前`Screen Builder`是空白的，我们需要向里面添加指导语的文字内容。

点击`Screen Builder工具栏`中的`多行文本框`，此时`多行文本框`的按钮将会被染上蓝色的底纹。然后点击`Screen Builder`的空白位置，将跳出`MultiLine Text Resource Editor`。如下图所示：

![eb_add_multiline_text](/assets/images/eb_add_multiline_text.gif)

`MultiLine Text Resource Editor`实际上和我们日常使用的Word软件非常相似。如下图所示，我们可以在工具栏中看到很多似曾相识的按钮。具体按钮的使用我也会在后面的编程中逐一介绍。

![eb_multiline_text_editor_toolba](/assets/images/eb_multiline_text_editor_toolbar.png)

`MultiLine Text Resource Editor`的工作区大小是和被实际的显示器尺寸对应的。白色的区域才是有效的编辑范围。

>但是这里的白色也是相对的，我们目前设置的显示背景色为白色，因此有效范围是白色，如果设置成黑色，则有效范围将会用黑色标注。具体背景颜色的设置在下一部分介绍。

既然`MultiLine Text Resource Editor`和Word很相似，那我们就直接在`MultiLine Text Resource Editor`里面进行文字编辑即可。写入文字后，对文字全选，统一执行格式设置：`宋体`、`18号`、`双倍行距`。

![eb_multiline_text_editor_config_text_style](/assets/images/eb_multiline_text_editor_config_text_style.png)

随后关闭`MultiLine Text Resource Editor`窗口即可。此时我们看到指导语的内容已经显示在屏幕上了。

![eb_screenbuilder_greeting_no_margin](/assets/images/eb_screenbuilder_greeting_no_margin.png)


现在所有的文字都是顶着屏幕边缘呈现的，这并不美观。所以我们来调整下文字的“页边距”。

双击屏幕上的文字，再次打开`MultiLine Text Resource Editor`。点击工具栏第二行的第一个按钮`Margin`。

![eb_multiline_text_editor_margin](/assets/images/eb_multiline_text_editor_margin.png)

四行参数分别对应：

* **Top Margin** - 上边距的像素值
* **Bottom Margin** - 下边距的像素值
* **Left Margin** - 左边距的像素值
* **Right Margin** - 右边距的像素值

我们将`Top Margin`和`Bottom Margin`设置为80，将`Left Margin`和`Right Margin`设置为100。点击`OK`即可看到`MultiLine Text Resource Editor`中，文字的排版已经变成了我想要的样子。

![eb_mutliline_text_editor_after_margin](/assets/images/eb_mutliline_text_editor_after_margin.png)

粉色线框即是目前有效的编辑范围。此时我们关闭`MultiLine Text Resource Editor`，可以看到指导语的内容已经编辑好了。

![eb_screenbuilder_after_margin](/assets/images/eb_screenbuilder_after_margin.png)

## 1.2 设备屏幕背颜色

根据实验设计，我需要将显示器的背景色设置为中性灰色，即`(128, 128, 128)`。

![eb_displayscreen_bg_colo](/assets/images/eb_displayscreen_bg_color.png)

在`工作区标签`面板中选择`Experiment`返回Experiment层级，点击`Greeting`，在`Properties面板`中设置`Background Color`属性。

我们需要先点击一下`Background Color`后面`Value`的部分，此时后面会出现`...`的按钮，点击后会出现`Edit Attribute窗口`。如下图所示：

![eb_call_reference_windo](/assets/images/eb_call_reference_window.gif)

在跳出的窗口中编辑内容为`128, 128, 128, 255`中性灰。最后一位为`Alpha`透明度，设置为`255`完全不透明。

这时我们重新打开`Greeting`，可以看到背景颜色已经变成了灰色。

![eb_greeting_finish](/assets/images/eb_greeting_finish.png)

## 1.3 设置按键确认

根据实验设计，在此`Greeting`呈现之后，被试按`空格`确认指导语内容。

首先我们将`Keyboard Trigger`从`控件库`中拖动到工作区中。`Keyboard Trigger`在`控件库`的`Trigger`中。

![eb_add_keayboard_trigge](/assets/images/eb_add_keayboard_trigger.gif)

同样，首先更改`Keyboard Trigger`的属性

* `Label` -> Keyboard_of_Greeting

这时`Keyboard Trigger`已经更名为`Keyboard_of_Greeting`。

用箭头将`Greeting`和`Keyboard_of_Greeting`连接起来。如下图所示：

![eb_greeting_part](/assets/images/eb_greeting_part.png)

此时我们已经将`Keyboard_of_Greeting`加入到了整个实验序列中，下一步设置触发按键为“空格”。

选中`Keyboard_of_Greeting`，设置`Keys`属性。`Keys`属性为可以让`Keyboard_of_Greeting`触发的按键。设置过程如下图所示：

![eb_set_keyboard_keys](/assets/images/eb_set_keyboard_keys.gif)

确认`Keys`属性为“Space”即设置成功。

---

# 2. 添加Block

![eb_node_lib_sequence](/assets/images/eb_node_lib_sequence.png)

将`Sequence`控件从`控件库`拖拽到工作区中，连接箭头，修改`Label`属性为`Block`。

![eb_add_block_sequence](/assets/images/eb_add_block_sequence.gif)

---

# 3. 添加结束语

结束语内容：

> 实验结束，谢谢您的参与。



---
以上。