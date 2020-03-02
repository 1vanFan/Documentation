
---
title: 视频流回退
description: 
platform: macOS,iOS
updatedAt: Mon Mar 02 2020 08:06:02 GMT+0800 (CST)
---
# 视频流回退
## 功能描述

网络不理想的环境下，直播音视频的质量都会下降。为提升直播效率，Agora 新增了 `setLocalPublishFallbackOption` 和 `setRemoteSubscribeFallbackOption` 两个接口。用户设置这两个接口后，在网络条件差、无法同时保证音视频质量的情况下，SDK 会自动将视频流从大流切换为小流，或将媒体流回退为音频流，从而保证或提高音频质量。

## 实现方法

在实现视频流回退机制前，请确保已在你的项目中实现基本的音视频功能。详见[开始互动直播](../../cn/Video/start_live_ios.md)。

参考如下步骤，在你的项目中实现视频流回退功能：

1. 加入频道后，主播调用 `enableDualStreamMode` 方法开启[双流模式](https://docs.agora.io/cn/Agora%20Platform/terms?platform=All%20Platforms#a-name-duala双流模式)。

2. 主播调用 `setLocalPublishFallbackOption` 方法，并设置 `AudioOnly (2)` 开启本地发布的媒体流在弱网环境下回退。当网络不好时，SDK 会自动断开视频流，只发布音频流。本地发布的流从媒体流切换到音频流，或从音频流切换到媒体流时，SDK 会触发 `didLocalPublishFallbackToAudioOnly` 回调，主播可以了解当前发布流的状态。

   > 旁路推流直播的场景下，主播设置 `AudioOnly (2)` 可能导致 CDN 观众听到的声音有所延迟。Agora 不建议主播使用视频流回退机制。

3. 频道内用户调用 `setRemoteSubscribeFallbackOption` 方法设置弱网环境下接收媒体流的回退选项。

   - 设置 `StreamLow (1)` ，则下行网络较弱时，只接收主播的视频小流。
   - 设置 `AudioOnly (2)`，则下行网络较弱时，先尝试只接收主播的视频小流。如果网络环境无法显示视频，则只接收主播的音频流。

   用户接收到的流从媒体流切换到音频流，或从音频流切换到媒体流时，SDK 会触发 `didRemoteSubscribeFallbackToAudioOnly` 回调，该用户可以了解当前接收流的状态。当接收到的流从大流切换到小流，或从小流切换到大流时，SDK 会触发 `remoteVideoStats` 回调，该用户可以了解当前接收到的视频流类型。

### 示例代码


```swift
//Swift
// 开启视频双流模式。
agoraKit.enableDualStreamMode(true)

// 发送端的配置。网络较差时，只发送音频流。
agoraKit.setLocalPublishFallbackOption(.audioOnly)

// 接收端的配置。弱网环境下先尝试接收小流；若当前网络环境无法显示视频，则只接收音频流。
agoraKit.setRemoteSubscribeFallbackOption(.audioOnly)
```

```objective-c
//Objective-C
// 开启视频双流模式。
agoraKit.enableDualStreamMode(true);

// 发送端的配置。网络较差时，只发送音频流。
[agoraKit setLocalPublishFallbackOption: AgoraStreamFallbackOptionAudioOnly];

// 接收端的配置。弱网环境下先尝试接收小流；若当前网络环境无法显示视频，则只接收音频流。
[agoraKit setRemoteSubscribeFallbackOption: AgoraStreamFallbackOptionAudioOnly];
```

### API 参考

- [`enableDualStreamMode`](https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/enableDualStreamMode:)
- [`setLocalPublishFallbackOption`](https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLocalPublishFallbackOption:)
- [`setRemoteSubscribeFallbackOption`](https://docs.agora.io/cn/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setRemoteSubscribeFallbackOption:)
- [`didLocalPublishFallbackToAudioOnly`](https://docs.agora.io/cn/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didLocalPublishFallbackToAudioOnly:)
- [`didRemoteSubscribeFallbackToAudioOnly`](https://docs.agora.io/cn/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didRemoteSubscribeFallbackToAudioOnly:byUid:)
- [`remoteVideoStats`](https://docs.agora.io/cn/Video/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:remoteVideoStats:)
