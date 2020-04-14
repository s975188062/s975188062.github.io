---
title: "设置随机"
excerpt: "EB Basic系列的随机设置，主要涉及试次随机、实验内分组（Block）和分割实验等。"
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

# 1 为什么要设置随机

通过随机或者拉丁方等方法，可以避免因试次运行顺序引入的不可控的变量。设置随机的理论可以很简单，也可以非常复杂。这里不是实验设计的教程，因此这部分暂且略过。

我们这里使用一个比较简单的方法——让所有试次的出现顺序随机。

--

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

* 在正式实验之前，希望添加一个联系的部分，让被试熟悉实验流程，我们称之为练习的Block；

* 当试验整体时间过长的时候，我们可以通过一些设置，将实验分割成多个Block，让被试在每个Block之间得以休息。




## 2.3 设置分割实验

# 