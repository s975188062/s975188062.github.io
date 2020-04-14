---
title: "编程前的实验设计准备"
excerpt: "EB Basic系列的前序文章，提示开始变成之前需要做哪些准备。"
categories:
  - Eyelink
tags:
  - Experiment Builder
  - 编程技巧
  - 实验设计
toc: true
comments: true
author_profile: false
sidebar:
  title: "目录"
  nav: EB
---

---

本节将介绍如何高效地进行编程前的实验设计准备。

预先设定的实验设计如下：

>被试将自定步速浏览4篇英文文章，在此过程中同步采集眼动数据。在被试完成当前文章的浏览后，会向被试提出一个关于文章的问题，以保证被试认真阅读。
>
>我们将检测阅读过程中，每个单词的`First Fixation Duration`、`Gaze Duration`和`Dwell Time`三个指标与词长的关系。

---

# 1. 手绘流程图

> 好记性不如烂笔头。
> 
> 把实验中需要注意的点一项一项写在纸上就不会出错了。

![eb_exp_design](/assets/images/eb_exp_design.png)

我个人习惯于将实验设计的细节全部铺在纸上，通过思维导图的方式把细节全部链接在一起。

编程的过程中需要设置很多细节内容，这是在编程的过程中一步一步设置的。切不能把所有内容编辑好之后一次性回头重来，这样非常容易丢失细节。

如上图所示，我将实验设计中所有的细节信息都罗列了出来。在后面的章节中将一致依照这个思维导图来完成实验编写。

---

# 2. Datasource表格

在编写实验程序的过程中，我们需要创建一个表格来把每个试次呈现的内容装进去。这在我们制作实验材料的时候也是需要的。

例如我目前的实验任务，我需要设置的内容很简单，即每个试次呈现的文字内容即可。此处由于排版的关系，我简化了Text Page的内容


| Trial ID | Text Page             | Question                         | Correct Answer |
|:---------|:----------------------|:---------------------------------|:---------------|
| 1        | Buck did not read...  | The heavy dogs are popular.      | f              |
| 2        | The house was...      | There are 13 boys here in total. | f              |
| 3        | They came and...      | The fox terriers never yelp.     | j              |
| 4        | Among the terriers... | Elmo weight 100 pounds.          | j              |

---

以上。

