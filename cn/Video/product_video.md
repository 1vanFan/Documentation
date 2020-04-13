
---
title: 产品概述
description: 
platform: All Platforms
updatedAt: Thu Apr 02 2020 05:19:36 GMT+0800 (CST)
---
# 产品概述
视频通话 SDK 可实现一对一单聊、多人群聊，同时具备纯语音通话和视频通话功能。

视频通话和[视频互动直播](https://docs.agora.io/cn/Interactive%20Broadcast/product_live?platform=All%20Platforms)不同。视频通话，不分主播和观众，所有用户都可自由发言，默认流畅和低延时优先，画质稍低，典型场景如多人视频会议；互动直播，用户区分主播和观众，只有主播可以自由发言，默认高画质优先，典型场景如互动课堂。

## 功能和场景

<style> table th:first-of-type {     width: 150px; } th:third-of-type {     width: 170px; }</style>

| 主要功能             | 功能描述                                                     | 典型适用场景                                                 |
| -------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 伴奏混音             | 将本地或在线的音频和用户声音，同时发送并播放给频道内其他用户 | <li>在线合唱<li>音乐互动课堂                                         |
| 基础美颜          | 支持基础的美颜功能，包括设置美白、磨皮、祛痘、红润效果。 | 视频聊天美颜                       |
| 屏幕共享     | 把屏幕内容同步展示给频道内的其他用户，支持指定共享某个屏幕或窗口，同时支持指定共享区域。      | 互动课堂          |
| 修改音视频原始数据   | 可支持变声，支持获取媒体引擎的原始语音或视频数据，对原始数据进行处理 | <li>聊天室变声<li>视频聊天美颜                                       |
| 自定义视频源和渲染器 | 支持自定义的视频源和渲染器，可以不使用系统摄像头，使用自己构建的摄像头视频源，屏幕共享视频源，或者文件视频源等，可以更灵活地处理视频，比如添加美颜效果、滤镜等。 | <li>需要使用自定义的美颜库或者前处理库<li>开发者 App 中已经有自己的图像视频模块<li>开发者希望使用非摄像头的视频源，如录屏数据<li>有些系统独占的视频采集设备，为了避免冲突，需灵活的设备管理策略 |

	
## 关键特性

| 特性         | Agora 视频通话指标                                           |
| ------------ | ------------------------------------------------------------ |
| SDK 包体积   | 3.69 M - 7.75 M                                              |
| 多人视频     | 支持 17 人视频通话 |
| 视频属性     | <li>SDK 采集支持 1080p 分辨率，60 fps 帧率 <li>自采集支持 4K |
| 音频属性     | <li>音频采样率：16 kHz - 48 kHz <li>支持单、双声道           |
| 音频抗丢包率 | 上下行抗丢包率 70%                                           |

### 平台兼容

视频通话支持 iOS、Android、Windows、macOS、Electron、Linux、Web、小程序，并支持平台间互通，具体的兼容性要求见下表。

| 平台       | 支持版本                                                     |
| ---------- | ------------------------------------------------------------ |
	| Android    | <p>4.1+</p><p>Android SDK 支持如下 ABI：</p><ul><li>armeabi-v7a<li>arm64-v8a<li>x86<li>x86_64                                                         |
| iOS        | 8.0+                                                         |
	| Windows    | <p>XP SP3+</p><p>Windows SDK 支持如下架构：<p><ul><li>x86<li>x64                                                      |
| macOS      | 10.0+                                                        |
| 微信小程序 | 支持                                                         |
| Web        | <li>Chrome 58+ <li>Chrome 49（仅 Windows XP）<li>Firefox 56+ <li>Safari 11+ <li>Opera 45+ <li>QQ 10+ <li>360 安全浏览器 9.1+ |

<div class="alert note">Web 平台的支持情况还与设备型号及系统版本等有关，详见 <a href="https://docs.agora.io/cn/faq/browser_support">Agora Web SDK 支持哪些浏览器？</a></div>

## Demo 体验

### 视频通话应用 - Agora Video Call

在应用市场下载 Agora Video Call，快速体验多人跨国视频通话。

- [Android](http://android.myapp.com/myapp/detail.htm?apkName=io.agora.vcall)
- [iOS](https://itunes.apple.com/cn/app/agora-video-call/id1080303824)
- [macOS](https://itunes.apple.com/cn/app/agora-video-call/id1112106913)
- [Web](https://videocall.agora.io/#/)
- [Windows](https://download.agora.io/avc/release/AgoraVideoCall_for_windows_3.0.1.zip)
