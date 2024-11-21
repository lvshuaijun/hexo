---
title: 如何成功抓取到https:SSL/TSL交互请求信息
date: 2019-07-23 18:24:01
tags:
- Android
- 请求
- Charles
- MacOs

header_image: /intro/simptab-wallpaper-20190723115814.png

---

### 使用设备

- 手机型号：Meizu M6 Note
- Android版本：7.1.2
- Mac 
- charles
- postern
- VirtualXposed

### 文章简介

主要是因为现在市面上很多app都采用的HTTPS+SSL的交互，导致我们使用charles配合其证书根本抓取不到请求内容，然而这篇文章就可以让你抓到一些之前不存在的请求和之前出现"SSL: Unrecognized SSL message, plaintext connection?"的请求！


### 工具介绍

首先简单介绍下主要工具的作用：

**postern**: 作用是手机连接电脑的vpn，这样能够不使用charles的代理直接转接过来请求，单单使用这个方式可以抓到app中一些之前不存在的请求。虽然我具体不清楚是为什么会抓到.... 

**VirtualXposed**: 这个软件是一个基于VirtualApp的，免root、免解锁BL、免刷机使用Xposed框架的APP，我的理解就是它模拟了一个全新的android环境，并且这个环境中自带可以使用的xposed和免重启功能等。 

**前置要求**：使用postern配合charles抓取包的时候，有一个要求是必须是android7.0一下的机器，以上的貌似都不可以，但是如果在android7.0以上的话，可以使用VirtualXposed去抓包。（简单介绍下方式：先把postern装在手机原环境下，打开并且配置好，打开virtualxposed，安装要抓包的app，mac上开启charles，手机原环境下开启postern，在virtualxposed中打开app，就可以抓包啦～）


本文未完, 敬请期待...




