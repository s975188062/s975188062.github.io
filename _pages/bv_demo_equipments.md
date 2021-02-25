---
permalink: /bv_demo_equipments_logger/
title: "样机流转记录"
toc: false
---


<form
  action="https://formspree.io/mbjadrzd"
  method="POST"
>
  <label>
    清点日期：
    <input type="text" name="_clearDate">
  </label>
  <label>
    寄出/寄回 及 地址信息：
    <input type="text" name="_customAddress">
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
    <input type="text" name="_mainSN">
  </label>
  <label>
    设备清单：
    <textarea name="list"></textarea>
  </label>

  <button type="submit">发送</button>
</form>