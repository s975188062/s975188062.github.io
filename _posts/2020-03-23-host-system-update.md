---
title: "主试机系统更新教程"
categories:
  - Eyelink
tags:
  - 主试机
  - 实验室设置
toc: true
---

---

截止2020年3月23日，最新的系统版本号如下：

* `Eyelink 1000` -> `4.594`
* `Eyelink 2000` -> `4.594`
* `Eyelink 1000Plus` -> `5.15`
* `Eyelink Portable Duo` -> `6.12`

您可以打开主试机，在`Offline界面`的右下角，查看当前系统版本号，如下图所示：

![hsot_1kp_offline](/assets/images/hsot_1kp_offline-1.jpg)

我们可以看到当前主试机的系统版本号为“5.00.50”。

---

# 1. Eyelink 1000/2000

`桌面式`根据红外发光器的位置，在下图中二选一下载；
`塔式`、`核磁式`、`灵长类动物式`下载任意一个均可；
`LCD Arm`请下载红外发光器在右侧的版本。

* 红外发光器在左侧：[Github](https://raw.githubusercontent.com/s975188062/s975188062.github.io/master/_source/Host%20PC%20Upgrade%20Installiation/elcl_4.594_left.zip)，[百度网盘(z52o)](https://pan.baidu.com/s/10IoW79--cSgtJgkLcM7wpA)；
* 红外发光器在右侧：[Github](https://raw.githubusercontent.com/s975188062/s975188062.github.io/master/_source/Host%20PC%20Upgrade%20Installiation/elcl_4.594_right.zip)，[百度网盘(xi2x)](https://pan.baidu.com/s/1kGxr_IbWez46tFMZUCkEwg)。

Eyelink 1000/2000的主试机使用的是DOS系统，在更新操作系统时需要进入到`Windows`系统中来操作。

开机，在`Boost Manager`界面进入`Microsoft Windows Vista`。

![EL_1k_host_boost](/assets/images/EL_1k_host_boost.jpg)

进入`Microsoft Windows Vista`后打开`我的电脑`，可以看到除Windows系统分区外还有一个硬盘分区，根据主试机版本不同，可能叫做`D:/`或`Eyelink`等不同名字。

无论硬盘分区叫什么名字，该分区下都有一个名为`ELCL`的文件夹。

另一边，我们将下载好的更新压缩包解压保存到U盘中。将U盘插到需要更新的主试机上，我们可以看到解压好的更新压缩包也叫做`ELCL`。此时我们只需要将U盘中的`ELCL`文件夹拖拽至Eyelink系统盘中，覆盖原来的`ELCL`文件夹即可。

重新启动电脑，在`Boost Manager`进入`Eyelink`，敲入“t”，按回车即可启动主试机。这个时候我们可以在`Offile界面`看到已经更新到最新系统了。

---

# 2. Eyelink 1000Plus

`Eyelink 1000Plus`系列主试机系统的最新版本为`5.15`.

下载链接：[Github](https://github.com/s975188062/s975188062.github.io/raw/master/_source/Host%20PC%20Upgrade%20Installiation/elcl-5.15.zip)，[百度网盘(2zef)](https://pan.baidu.com/s/1AkHcBcP-OohzHz62zG0jFw)

下载后将文件直接放到U盘中，**不要解压缩**。

首先，在主试机的`Offline界面`点击`Exit Eyelink`按钮，退出`Track系统`。

![host_offline2boost](/assets/images/host_offline2boost.jpg)

系统将直接进入到如下图所示的`File Manager界面`。如果没有自行跳转，则点击`File Manager按钮`进入到`File Manager界面`。

在电脑上插入已经准备好的U盘。

点击`Configuration按钮`。

![host_filemanager2configuration](/assets/images/host_filemanager2configuration.jpg)

点击`System Update按钮`。

![host_configuration2updateSyste](/assets/images/host_configuration2updateSystem.jpg)

点击`Choose File按钮`。

![host_system_update](/assets/images/host_system_update.jpg)

在窗口上方的地址栏中，选择新插入的U盘，名称可能是`/fs/usb0`。

![host_sysUpdate_choose_drive](/assets/images/host_sysUpdate_choose_drive.jpg)

在文件列表中选择准备好的升级包，点击右下角的`Open`按钮。

![host_sysUpdate_choose_file](/assets/images/host_sysUpdate_choose_file.jpg)

点击`Update`按钮。

![host_sysUpdate_confrim_update](/assets/images/host_sysUpdate_confrim_update.jpg)

系统会自动完成更新，点击工具栏的`Eyelink`按钮即可返回到`Track`系统中。

![host_start_from_filemanage](/assets/images/host_start_from_filemanager.jpg)

---

# 3. Eyelink Portable Duo

`Eyelink Portable Duo`系列主试机系统的最新版本为`6.12`.

下载链接：[Github](https://github.com/s975188062/s975188062.github.io/raw/master/_source/Host%20PC%20Upgrade%20Installiation/elusb-6.12-18-02-01-PortableDUO.duo.zip)，[百度网盘(i87e)](https://pan.baidu.com/s/1HL-TsFR2V48d04APO_Zg2g)

您可以在主试机的`Setup界面`的**左下角**查看主试机的系统版本号。如下图所示：

![host_pd_setup](/assets/images/host_pd_setup.jpg)

下载后将文件直接放到U盘中，**不要解压缩**。

点击屏幕右侧的`Exit按钮`，随后在出现的对话框中点击`Exit Eyelink`即可自动跳转到`File Manager界面`。如果没有自行跳转，则点击`File Manager按钮`进入到`File Manager界面`。

后面的操作和`Eyelink 1000Plus`的升级方法一致。

>在电脑上插入已经准备好的U盘。
>
>点击`Configuration按钮`。
>
>![host_filemanager2configuration](/assets/images/host_filemanager2configuration.jpg)
>
>点击`System Update按钮`。
>
>![host_configuration2updateSyste](/assets/images/host_configuration2updateSystem.jpg)
>
> 点击`Choose File按钮`。
>
> ![host_system_update](/assets/images/host_system_update.jpg)
>在窗口上方的地址栏中，选择新插入的U盘，名称可能是`/fs/usb0`。
>
>![host_sysUpdate_choose_drive](/assets/images/host_sysUpdate_choose_drive.jpg)
>
>在文件列表中选择准备好的升级包，点击右下角的`Open`按钮。
>
>![host_sysUpdate_choose_file](/assets/images/host_sysUpdate_choose_file.jpg)
>
>点击`Update`按钮。
>
>![host_sysUpdate_confrim_update](/assets/images/host_sysUpdate_confrim_update.jpg)
>
>系统会自动完成更新，点击工具栏的`Eyelink`按钮即可返回到`Track`系统中。
>
>![host_start_from_filemanage](/assets/images/host_start_from_filemanager.jpg)

以上。

