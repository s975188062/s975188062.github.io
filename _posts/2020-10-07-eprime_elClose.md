---
title: "elClose脚本(E-Prime with DevKit v1.11.5)"
excerpt: "在 E-Prime 中将眼动数据保存好。"
read_time: false
header:
  overlay_image: /assets/images/eprime_header.png
  overlay_filter: 0.5
  actions:
    - label: "下载示例文件(gsbv)"
      url: "https://pan.baidu.com/s/1dLvtJpazXb8DARSqknLyWg"
categories:
  - Eyelink
  - E-Prime
tags:
  - E-Prime
toc: true
comments: true
author_profile: false
sidebar:
  title: "目录"
  nav: EL_3rd_Eprime
---

---

> 本篇是基于安装了 `1.11.5` 版本 Development Kit 的用户，如果您安装的是 `2.1.1` 版本的 Development Kit ，请点击 [>>>传送门<<<](/eyelink/e-prime/eprime_clClose_devkit_2_1_1/)。
{: .notice--warning}

# 1. 代码讲解

## 1.1 传输 .edf 眼动数据文件

整个实验已经完成，让我们把眼动数据保存下来。

~~~ vb
tracker.setOfflineMode  '将主试机设置为 offline
Sleep 500 '延迟50ms，让主试机做好准备
tracker.closeDataFile '关闭.edf数据文件
tracker.receiveDataFile edfFileName, "" '将数据文件发送到主试机
Set tracker = Nothing '释放 tracker 这个程序对象
Set elutil  = Nothing '释放 elutil 这个链接对象
~~~

复制粘贴，不要犹豫。
{: .notice--info}

## 1.2 Error Handle 错误句柄

~~~ vb
ErrorHandle:
	If Err <> 0 Then 
		Set tracker = Nothing '释放 tracker 这个程序对象
		Set elutil  = Nothing '释放 elutil 这个链接对象
		MsgBox Err.Number & ":" & Err.Description
    End If
~~~

复制粘贴，不要犹豫。
{: .notice--info}

---

# 2. 源代码

~~~ vb
tracker.setOfflineMode  ' set offline mode so we can transfer file 
Sleep 500 ' delay so tracker is ready 
tracker.closeDataFile ' close data file 
tracker.receiveDataFile edfFileName, "" ' get the edf file to display pc
Set tracker = Nothing ' release tracker object
Set elutil  = Nothing ' release eyelink util object

ErrorHandle:
	If Err <> 0 Then 
		Set tracker = Nothing ' release tracker object
		Set elutil  = Nothing ' release eyelink util object
		MsgBox Err.Number & ":" & Err.Description
		'Exit Sub   if this is not the last inline, we need an Exit Sub
    End If
~~~

---

以上。