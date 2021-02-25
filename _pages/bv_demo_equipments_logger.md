---
permalink: /bv_demo_equipments_logger/
title: "样机流转记录"
toc: false
class: wide
---

请录入样机流转信息：

<form
  action="https://formspree.io/mbjadrzd"
  method="POST"
>
  <label>
    记录人：
    <input type="text" name="whodidthis">
  </label>
  <label>
    清点日期：
    <input type="text" name="_clearDate">
  </label>
  <label>
    寄出/寄回 及 地址信息：
    <textarea name="address" rows="3"></textarea>
  </label>
  <label>
    客户名称：
    <input type="text" name="_customName">
  </label>
  <label>
    设备型号：
    <input type="text" name="_deviceType">
  </label>
  <label>
    核心部件S/N号：
    <textarea name="SNlist" rows="4"></textarea>
  </label>
  <label>
    设备清单：
    <textarea name="list" rows="8"></textarea>
  </label>

  <button type="submit">发送</button>
</form>