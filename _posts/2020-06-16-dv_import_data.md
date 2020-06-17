---
title: "向Data Viewer中导入数据"
excerpt: "向Data Viewer中导入您的数据。"
read_time: false
header:
  overlay_image: /assets/images/dv_intro_header.png
  overlay_filter: 0.5
categories:
  - Eyelink
tags:
  - Data Viewer
  - ImportData
toc: true
comments: true
author_profile: false
sidebar:
  title: "目录"
  nav: DV
---

---

> 从难度上描述Data Viewer处理眼动数据的过程，大致可以等同于把大象装冰箱。——Charlie

我们在[>>>认识Data Viewer<<<](/eyelink/DV_Intro/#11-菜单栏)中已经阐述了`.edf`和`.evs`的关系。您直接打开`.edf`数据时只是查看当前数据，和批量处理数据还相差甚远。

首先，让我们打开Data Viewer软件。

![dv_import_data_open_dv](/assets/images/dv_import_data_open_dv.png)

---

# 1. 导入单个.edf数据文件 

您可以在菜单栏“File - Import Data - Eyelink Data File”或者工具栏中找到导入单个`.edf`文件的按钮。

![dv_import_data_toolbar_import_one_data](/assets/images/dv_import_data_toolbar_import_one_data.png)

随后我们只需要在弹出的窗口中选择我们需要导入的`.edf`文件，点击`Load`即可。

![dv_import_data_toolbar_import_one_data_load_edf](/assets/images/dv_import_data_toolbar_import_one_data_load_edf.png)

经过短暂的等待，我们即可在“Data Inspector”中看到导入的数据。

![dv_import_data_show_imported_data](/assets/images/dv_import_data_show_imported_data.png)

以上图中的数据为例，其数据结构大致如下：

```
br                                  # .edf文件名为“br”
├── br: Trial: 1                    # br.edf的第 1 个Trial
|  └── Custom Interest Area Set     # br.edf的第 1 个Trial的兴趣区
├── br: Trial: 2                    # br.edf的第 2 个Trial
|  └── Custom Interest Area Set     # br.edf的第 2 个Trial的兴趣区
├── br: Trial: 3                    # ……
|  └── Custom Interest Area Set
├── br: Trial: 4
|  └── Custom Interest Area Set
├── br: Trial: 5
|  └── Custom Interest Area Set
```

注意：上图中的“第n个Trial”的编号是指试验运行的顺序，和EB中的DataSource第几行没有严格关系。
{: .notice--info}

---

# 2. 导入多个.edf数据文件

往往我们会有几十个上百个数据文件，一个一个导入并不合理，因此我们可以使用更方便的一键导入。

一键导入多个`.edf`的执行逻辑是：我们给定一个实验文件夹的路径(通常是在EB中Deploy出来的实验文件夹)，DV软件会扫描该路径及其子路径中的全部`.edf`文件，将其全部报告出来，根据我们的需要统一导入。

您可以在菜单栏“File - Import Data - Multiple Eyelink Data File”或者工具栏中找到导入多个`.edf`文件的按钮。

![dv_import_data_toolbar_import_multiple_data](/assets/images/dv_import_data_toolbar_import_multiple_data.png)

选择Deploy文件夹，即可扫描全部已经采集的数据。您可以将预实验的数据取消勾选，然后将剩下的所有实验一并导入。

![dv_import_data_import_multiple](/assets/images/dv_import_data_import_multiple.gif)

可以看到，我现在已经将`br.edf`和`dm.edf`导入到了DV中。

![dv_import_data_import_multiple_result](/assets/images/dv_import_data_import_multiple_result.png)

---

# 3. 保存

> 务必养成良好的保存习惯，没事儿多多`Ctrl+S`。
{: .notice--info}

您可以在菜单栏“File - Save”或者工具栏中找到保存的按钮。

![dv_import_data_save_evs](/assets/images/dv_import_data_save_evs.png)

将`.evs`文件保存到您希望的位置，路径中不要有中文字符。

---

以上。