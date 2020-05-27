---
title: "设置随机与Block分割"
excerpt: "EB Basic系列的随机设置，主要涉及试次随机、实验内分组（Block）和分割实验等。"
read_time: false
header:
  overlay_image: /assets/images/eb_rand_header.png
  overlay_filter: 0.5
  teaser: /assets/images/eb_spliting_column_runing_window.png
categories:
  - Eyelink
tags:
  - Experiment Builder
  - Datasource
  - Randomization
toc: true
comments: true
author_profile: false
sidebar:
  title: "目录"
  nav: EB
---

---

# 1. 为什么要设置随机

通过随机或者拉丁方等方法，可以避免因试次运行顺序引入的不可控的变量。设置随机的理论可以很简单，也可以非常复杂。这里不是实验设计的教程，因此这部分暂且略过。

我们这里使用一个比较简单的方法——让所有试次的出现顺序随机。

---

# 2. 设置随机

在`Datasource`中打开`Randomization Setting`。

![eb_ds_open_rand_set](/assets/images/eb_ds_open_rand_set.png)

跳出如下图所示的随机设置窗口，具体每个部分的含义我们放到最后来解释，我们先跟随具体的需求一步一步来操作。

![eb_ds_rand_set](/assets/images/eb_ds_rand_set.png)

## 2.1 设置试次随机

试次随机基本上可以说是**最实用**的随机方法了，设置的方法也非常简单，只勾选一个`Check Box`即可。

![eb_ds_rand_trial_rand](/assets/images/eb_ds_rand_trial_rand.png)

以本实验为例，一共4个试次。

* 如果不`Enable Trial Randomization`的话，每个被试来做实验的时候都是从第1个试次到第4个试次依次进行的。

* 如果`Enable Trial Randomization`的话，那么每次运行试验的时候，4篇文章的出现顺序将会被随机打乱。

## 2.2 设置分割Block

分割Block的时候，通常是以下两种情况：

* 在正式实验之前，希望添加一个练习的部分，让被试熟悉实验流程，我们称之为练习的Block；

* 当试验整体时间过长的时候，我们可以通过一些设置，将实验分割成多个Block，让被试在每个Block之间得以休息。

### 2.2.1 分割练习Block

此处我们忽略实验设计的合理性，将仅有的4篇文章分1篇出来用作练习实验。我们应该如何设置呢？

首先在`Datasource`中新建一个Column用来标记哪些试次是练习部分，哪些试次是正式实验部分：

![eb_ds_with_type](/assets/images/eb_ds_with_type.png)

如上图所示，我新建了一个名为“Type”的Column，其中值为“T”的试次是练习部分（Training Part），值为“F”的部分为正式实验部分（Formal Part）。

接下来我们要进行属性设置，稍微有些复杂。拆开来看，我们需要做三件事：

`设置Block层的循环次数` -> `设置每个Block执行多少个Trial` -> `设置Datasource分割`

* 首先，设置`Block层`的循环次数。

>练习部分是1个Block，正式实验部分是1个Block，一共两个Block。那么就是让`Block层`执行2次，第一次执行练习的1个试次，第二次执行正式实验的3个试次。
>
>`Sequence控件`有一个属性叫做`Iteration Count`，即重复次数。因此我们将`Block层`的`Iteration`属性设置为“2”。
>
>![eb_set_block_interation](/assets/images/eb_set_block_interation.png)

* 设置每个Block执行多少个Trial

>选中`Trial层`，我们可以看到`Iteration Count`和`Split by`两个属性。
>
>`Iteration Count`目前的值为4，其值默认与`Datasource`中的数据行数相同。即重复运行4次`Trial层`，每次运行`Datasource`中的1行。
>
>`Split by`属性默认是空，意为一次性执行完全部4个`Trial`。我们的需求是将4个Trial分成两次执行，因此将`Split by`属性设置为“[1, 3]”。
>
>![eb_set_trial_split](/assets/images/eb_set_trial_split.png)

**⚠️注意⚠️**：“[1, 3]”中的逗号必须是英文逗号，不能是中文。
{: .notice--warning}

* 设置`Datasource`分割

>在`Datasource`中打开`Randomization Setting`，设置`Block Levels`为“Type”，注意后面的`Randomize`**不要勾选**。
>
>![eb_ds_rand_split_T-F](/assets/images/eb_ds_rand_split_T-F.png)
>
>结合前面对于`Block层`的`Iteration Count`和`Trial 层`的`Split by`，我们已经成功将实验分割成了1个试次的练习Block和3个试次的正式Block。

### 2.2.2 分割正式实验的Block

问题继续，如果正是实验部分的时间非常长，试次非常多，那么我们应该如何操作才能让被试在中间休息呢？

忽略实验设计的合理性，我们现在将正是实验部分的3个Trial拆分成3个Block。

* 修改`Block层`的执行次数

> 因为将正是实验的1个Block拆分成了3个Block，算上练习Block的话，总的Block数变成了4个。因此，首先修改`Block层`的`Iteration Count`为“4”。
> 
> ![eb_set_block_iteration_count_2](/assets/images/eb_set_block_iteration_count_2.png)

* 修改`Trial层`的`Split by`属性。

> 由于Block数量的变化，每个Block执行的Trial数量也有变化，根据实际情况进行分割，即4个Block，各分别执行1个Trial。因此修改`Trial层`的`Split by`属性为“[1, 1, 1, 1]”。
> 
> ![eb_set_trial_splitby_2](/assets/images/eb_set_trial_splitby_2.png)

这部分操作的逻辑其实是这样的：

在`Randomization Setting`中的`Block Level`设置了`Type Column`，则运行的时候就回先寻找`Type Column`里面第一行的值，即`T`。

执行完`T`的部分就回寻找第二种类型，即`F`。但是由于`Split by`的限制，每次只执行1个Trial，共执行三次。就是这样讲3个Trial划分成了3个Block。

由于`Randomization Setting`设置了`Enable Trial Randomization`，所以`F`部分的执行顺序是随机的。相当于把所有`F`部分一起先做随机，再分批次执行。

## 2.3 设置分割实验

继续深入，有些时候我们还会碰到这样的一个需求：

我的实验设计会将我的实验分成两种情况，分别给两组不同的人做。但是实验结构是相同的，只是内容上有差异，该如何处理呢？

方法其实很简单，我们将所有的素材全部放入`Datsource`中，新建一个命名为`Group`的`Column`来标记两组素材。当然，根据情况也可以更多组。

* 修改`Datasource`

> 新建一个命名为`Group`的`Column`来标记两组素材。
> 
> 点击`Add Rows`，根据情况添加行数，将`Datasource`粘贴进来。此处我直接复制了上面的`Datasource`。
> 
> 将两组`Datasource`的`Group`值进行修改，我这里分别赋值“A”和“B”。
> 
> ![eb_ds_add_group_column](/assets/images/eb_ds_add_group_column.png)

* 修改`Randomization Setting`设置

> 这部分我们使用`Randomization Setting`中的`Splitting Column`功能。
> 
> 如下图所示，设置`Splitting Column`功能为“Group”。则系统会自动识别“Group”这个`Column`里面的内容，将其分割为不同的实验。
> 
> ![eb_ds_rand_set_splitting_column](/assets/images/eb_ds_rand_set_splitting_column.png)
> 
> 在我们运行试验的时候，就回出现下图的提示窗口，让我们选择运行`Datasource`中的哪部分。
> 
> ![eb_spliting_column_runing_window](/assets/images/eb_spliting_column_runing_window.png)

---

`Test Run`一下试试效果吧！

![eb_show_test_run](/assets/images/eb_show_test_run.png)

以上。
