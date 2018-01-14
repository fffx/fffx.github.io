---
layout: post
title: "启动电源按钮损坏的安卓手机（红米note3)"
date:   2018-01-14 09:11:37 +0800
categories: android
---
自己的红米note3电源按钮坏了，开不了机，于是我想了一些办法。首先小米手机可以设置音量键唤醒屏幕（开机状态下），我开启了，然后又设置了定时开机，但是定时开机只能设置一个时间，导致我有时手机很久不用没充电又没法开机。最后我想能不能充电自动开机呢，毕竟充电的时候屏幕都亮了，于是搜索了关键词 `android power on cahrge`,果然可以，我看了下这个[视频](https://www.youtube.com/watch?v=Zp9G6A6EFlA&t=154s)。

## 然后开始动手
### 准备

- 下载[adb, fastbot工具](https://developer.android.com/studio/releases/platform-tools.html)
- 小米官网下载小米手机助手，这样会自动装好驱动，
- 然后将红米手机插上usb连接笔记本，然后一直按住音量下键不放，手机间断的震动几次会进入fastboot模式
- 设置充电启动`fastboot oem off-mode-charge 0`，这里会报错 not support on security, 我猜是没解锁的原因，由于我忘记自己的小米帐号了，也懒得去申请解锁，所以我我打算找找其他方案，先`fastboot continue` 开机再说。

心有不干，电源键坏了实在不方便，于是又再百度搜了下红米note3电源键修复，果然找到了（http://www.miui.com/thread-8784407-1-1.html）， 按照贴主说的拆机，用刀片刮，果然可以了 ：）。



参考：
https://stackoverflow.com/questions/26294258/auto-boot-when-wall-charger-is-plugged  
http://www.miui.com/thread-8784407-1-1.html
