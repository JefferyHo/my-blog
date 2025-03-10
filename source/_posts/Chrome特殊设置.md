---
title: Chrome特殊设置
date: 2024-09-23 15:47:56
description: 关于web开发中用到的Chrome特殊调试设置
tags:
  - chrome
  - web
categories:
  - web前端
cover: https://images.pexels.com/photos/38547/office-freelancer-computer-business-38547.jpeg?auto=compress&cs=tinysrgb&w=1260&h=750&dpr=2

---

# web开发中用到的`Chrome`特殊设置

## 允许http开头的网站访问流媒体

开发者在有些场合需要在非localhost场景下用到`麦克风权限`，这时因为chrome识别http作为not secure，不允许访问，这时这种设置就派上用场了。

chrome浏览器打开网址：`chrome://flags/#unsafely-treat-insecure-origin-as-secure`

在对话框中输入地址，并启用。



## 监控webrtc的上下行日志

在浏览器接收 `webrtc` 流的时候，出现故障时可以查看日志来判定是 *接收端* 还是 *发送端* 。而这种日志的入口就隐藏在chrome地址栏中。

chrome浏览器打开网址：`chrome://webrtc-internals/`

进入目标域名下的页签去查找。这里不赘述如何查看，如果感兴趣的话，后面可以单开一篇来细说。





