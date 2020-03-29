---
title: "指导语和结束语的呈现"
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

# 1. 指导语

>指导语内容：

>你好，欢迎参加本实验！
>
>在本实验中，您将阅读四段英文材料，完成阅读后请按空格结束当前文章的浏览。在每一段文章的浏览结束后，都会有一个关于文章的问题。如果问题的答案是“是”，则按“f”键；如果答案是“否”，则按“j”键。
>
>那么现在，请先帮我完成设备的调整吧。

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

我们目前打开的这个小窗口叫做`Screen Builder`，即呈现内容的编辑器。
