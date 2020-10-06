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
  nav: E-Prime
---

---

# 1. 代码讲解

## 1.1 传输 .edf 眼动数据文件

整个实验已经完成，让我们把眼动数据保存下来。

~~~ vb
tracker.setOfflineMode  '将主试机设置为 offline
{: .notice--info}
~~~

复制粘贴，不要犹豫。
{: .notice--info}

---

# 2. 源代码

~~~ vb
tracker.setOfflineMode  ' set offline mode so we can transfer file 
~~~

---

以上。