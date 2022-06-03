---
title: "elClose脚本(E-Prime with DevKit v2.1.1)"
excerpt: "断开 E-Prime 与眼动仪的链接，传输眼动数据。"
read_time: false
header:
  overlay_image: /assets/images/eprime_header.png
  overlay_filter: 0.5
  actions:
    - label: "下载示例文件(提取码：vo0m)"
      url: "https://pan.baidu.com/s/1A8QKilCfR1CNSBa_xsqKeA"
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

> 本篇是基于安装了 `2.1.1` 版本 Development Kit 的用户，如果您安装的是 `1.11.5` 版本的 Development Kit ，请点击 [>>>传送门<<<](/eyelink/e-prime/eprime_overview/)。您可以在 “开始菜单 - SR Research - SR Version” 中查看 `eyelink_core.dll` 的版本来确定您的 Development Kit 版本。
{: .notice--warning}

# 1. 脚本讲解

> 复制粘贴不要犹豫。

在实验过程中，眼动数据均记录在主试机上。在实验结束时，需要将眼动数据传输到被试机上保存。

~~~ vb
'-------------------------------------------------------------------------------

' 此部分代码将命令主试机停止数据记录关闭数据文件并发送一份给被试机，随后关闭与眼动仪的链接。

' 清空主试机显示
' clear_screen 命令使主试机监控屏填满颜色 0 (黑色) 
tracker.sendCommand "clear_screen 0"

tracker.setOfflineMode  ' 使主试机进入 offline 模式以传输数据
Sleep 500 ' 延迟 500 ms 确保主试机可用
tracker.closeDataFile ' 关闭数据文件
'receiveDataFile 用于向被试机发送严冬数据文件的备份
' 使用方法：receiveDataFile <source name> <optional path> <destination file name>
' 注意，若不指定路径，文件将默认保存在 E-Prime 的程序文件夹中
' 下面的例子可以将眼动数据文件保存在项目文件夹中的 Results 文件夹中
If tracker.isConnected <> -1 Then ' dummy mode 下跳过此步骤
 tracker.receiveDataFile edfFileName, "./Results/" & edfShortName & "/" & _
  edfFileName ' 传输数据文件
End If
Set tracker = Nothing ' 释放 tracker 对象
Set elutil  = Nothing ' 释放 elutil 对象

ErrorHandle:
 If Err <> 0 Then 
  Set tracker = Nothing ' 释放 tracker 对象
  Set elutil  = Nothing ' 释放 elutil 对象
  MsgBox Err.Description
  
  ' 如果这不是最后一个 inLine 脚本，则需要添加 Exit Sub 命令
  'Exit Sub   
    
    End If
~~~

---

# 2. 源代码

~~~ vb
'-------------------------------------------------------------------------------

'Here we are telling the Host PC to close the .edf file, send a copy of the .edf
'to the Display PC, then shut down the tracker objects in preparation for the
'end of the project.

'Clear the Host PC screen
'The command "clear_screen" erases the tracker display to color 0 (black) 
tracker.sendCommand "clear_screen 0"

tracker.setOfflineMode  ' set offline mode so we can transfer file 
Sleep 500 ' delay so tracker is ready 
tracker.closeDataFile ' close data file 
'receiveDataFile transfers a copy of the .edf file to the Display PC.
'Usage: receiveDataFile <source name> <optional path> <destination file name>
'Note: if no path is specified, the .edf will save to the project directory.
'The below saves the .edf to /ProjectDirectory/Results/edfname/edfname.edf
If tracker.isConnected <> -1 Then 'Skip file transfer if in dummy mode.
 tracker.receiveDataFile edfFileName, "./Results/" & edfShortName & "/" & _
  edfFileName ' get the edf file to display pc
End If
Set tracker = Nothing ' release tracker object
Set elutil  = Nothing ' release eyelink util object

ErrorHandle:
 If Err <> 0 Then 
  Set tracker = Nothing ' release tracker object
  Set elutil  = Nothing ' release eyelink util object
  MsgBox Err.Description
  'Exit Sub   if this is not the last inline, we need an Exit Sub
    End If
~~~

--- 

以上。