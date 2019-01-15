
---
title: 音频相关
description: 
platform: 音频相关
updatedAt: Tue Jan 15 2019 09:07:15 GMT+0000 (UTC)
---
# 音频相关
### iOS 端集成 H5 游戏音量低

**背景信息：**iOS SDK 2.2.0 集成 H5 游戏。

**问题现象：**用 layabox 自带的引擎播放 WkWebview，主播加入声网的频道，游戏里面的声音很低。该问题仅在 iOS 端发现，Android 端表现正常。

**问题原因：**WKWebView 加载的 H5 是另外一个进程通过 AVAudioSession 播放声音，不是通过 SDK 播放的。

**解决方案：**
`self.agoraEngine setAudioProfile:(AgoraRtc_AudioProfile_Default) scenario:(AgoraRtc_AudioScenario_GameStreaming);`  //选择 `GameStreaming` 模式。

**方案缺点：**

* 采用 `GameStreaming` 模式可能会出现一点回声，理论不影响使用，如有必现或大概率复现的情况可联系技术支持。
* H5 的声音不是通过 SDK 播放的，无法消除回声。

### Android 9 设备上应用退到后台或锁屏后，采集不到声音

**背景信息：** Android 9 系统行为变更。

**问题现象：** Android 9 设备锁屏 1 分钟内，音频无声。

**问题原因：** 从 Android 官网来看，这是系统强制限制。原文如下：

> **Limited access to sensors in background**
> Android 9 limits the ability for background apps to access user input and sensor data. If your app is running in the background on a device running Android 9, the system applies the following restrictions to your app:
> * Your app cannot access the microphone or camera.
> * Sensors that use the continuous reporting mode, such as accelerometers and gyroscopes, don't receive events.
> * Sensors that use the on-change or one-shot reporting modes don't receive events.
> If your app needs to detect sensor events on devices running Android 9, use a foreground service.


详见 [Android 行为变更](https://developer.android.com/about/versions/pie/android-9.0-changes-all)。


**解决方案：** 目前 Android 官网没有明确说明后台采集声音应如何处理，但使用**前台服务**可以让应用正常工作。

如果 Android 9 设备用户有锁屏后采集音频的需求，可以在锁屏或退至后台前起一个 Service，并在退出锁屏或返回前台前终止 Service。
关于如何起 Service，请参考 https://developer.android.com/reference/android/app/Service 。

### Android 设备进入频道后，耳机无声/语音路由不正常

**背景信息**：采用 Agora Android Demo 的模式，使用 WorkerThread 来维护 Agora RtcEngine 的实例。

**问题现象**：进频道后安卓设备上耳机无声，路由功能不正常等。

**问题原因**：

这个问题是由于退出 WorkerThread 时，没有销毁 RtcEngine 引起的。

有些 Agora 的 Android 示例程序通过创建一个 WorkerThread 线程来初始化引擎，这个线程的生命周期与示例程序的进程生命周期是一致的。线程里会维护一个全局的 RtcEngine 实例，这个全局的实例随着应用进程的销毁（调用 `destroy` 方法）而消亡。

而在实际开发的应用程序中，开发者应注意自己维护 RtcEngine 的生命周期，在合适的时机销毁引擎。

若开发者想要自定义 WorkerThread 的生命周期（如跟join/leave channel保持一致），则需要主动调用 `destroy` 方法来主动销毁引擎，单纯的退出 WorkerThread 是不会销毁 RtcEngine 的。

RtcEngine 调用 `destroy` 方法会移除所有注册过的系统 Listener （对于音频来说是 PhoneStateListener），这些监听可能会引用线程的 Looper。在相应的 Demo 里，这个 Looper 位于 WorkerThread 中。若退出 WorkerThread 时不主动调用 `destroy`，注册过的 Listener 会持续监听，但其引用的 Looper 会随着 WorkerThread 线程的停止而被置为无效或清空。最终会以 Dead Binder 的形式出错，使 SDK 的一些监听失效。

**解决方案**：

Agora 建议你选择如下一种方法解决该问题：

- 全局只维护一个 WorkerThread 线程
- 退出频道时，还需要调用 `destroy` 方法销毁 RtcEngine
