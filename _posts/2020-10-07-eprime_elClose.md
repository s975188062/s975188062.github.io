---
title: "elClose脚本(E-Prime眼动编程)"
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
  nav: EL_3rd
---

---

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