
---
title: 发版说明
description: 
platform: macOS
updatedAt: Wed Jun 12 2019 08:54:31 GMT+0800 (CST)
---
# 发版说明
本文提供 Agora 视频 SDK 的发版说明。

## **简介**

macOS 视频 SDK 支持两种主要场景:

-   音视频通话
-   音视频直播

点击 [语音通话产品概述](https://docs.agora.io/cn/Voice/product_voice?platform=All%20Platforms)、[视频通话产品概述](https://docs.agora.io/cn/Video/product_video?platform=All%20Platforms)、[音频互动直播产品概述](https://docs.agora.io/cn/Audio%20Broadcast/product_live_audio?platform=All%20Platforms) 及 [视频互动传播产品概述](https://docs.agora.io/cn/Interactive%20Broadcast/product_live?platform=All%20Platforms) 了解关键特性。


#### 已知问题和局限性

macOS 上连接 USB 耳麦，可能会出现听不见声音或者声音显示异常等问题，通常为 USB 设备驱动的问题，macOS 上对普通的 USB 支持都不是很友好，建议购买更优质的 USB 耳麦。

## **2.4.1 版**

该版本于 2019 年 6 月 12 日发布。新增特性、功能改进与修复问题列表详见下文。

### **升级必看**

如下内容涉及 SDK 的行为变更。如果你是有之前版本的 SDK 升级至该版本，升级前请务必阅读。

#### 1. CDN 推流

为提高推流服务的易用性，该版本对推流接口的参数设置进行了如下限制：

| 类**/**接口                 | 参数限制                                                     |
| --------------------------- | ------------------------------------------------------------ |
| [AgoraLiveTranscoding](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraLiveTranscoding.html) 类 | <li>[videoFrameRate](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraLiveTranscoding.html#//api/name/videoFramerate)：设置转码推流的帧率，单位为 fps，默认值为 15，建议不要超过 30<li>[videoBitrate](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraLiveTranscoding.html#//api/name/videoBitrate)：设置转码推流的码率，单位为 Kbps，默认值为 400。用户可以根据 [Video Profile 参考表](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraVideoEncoderConfiguration.html#//api/name/bitrate)中的码率值进行设置。如果设置的码率超出合理范围，服务端会在合理区间内对码率值进行自适应<li>[videoCodecProfile](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraLiveTranscoding.html#//api/name/videoCodecProfile)：设置转码推流的视频编码规格，可设为 **BASELINE**、**MAIN** 或 **HIGH**。若设为其他值，服务端会改为默认值 **HIGH **<li>[size](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraLiveTranscoding.html#//api/name/size)：设置转码推流的视频分辨率。size 的最小值不低于 16 x 16</li> |
| [AgoraImage](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraImage.html) 类           | `url`：字符长度不得超过 **1024** 字节                        |
| [addPublishStreamUrl](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/addPublishStreamUrl:transcodingEnabled:)     | `url`：字符长度不得超过 **1024** 字节                        |
| [removePublishStreamUrl](../../cn/Audio%20Broadcast/release_mac_video.md)  | `url`：字符长度不得超过 **1024** 字节                        |

同时，该版本在 `LiveTranscoding` 类中新增 [audioCodecProfile](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraLiveTranscoding.html#//api/name/audioCodecProfile) 参数，支持设置音频编码的规格。默认规格为 LC-AAC。

此外，该版本还对 [streamPublishedWithUrl](https://docs.agora.io/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/removePublishStreamUrl:) 方法的 `errorCode` 参数新增了五个错误码，方便快速定位与排查问题。

#### 2、RemoteVideoStats 类参数更名

为更精准地表达远端视频流的统计信息，该版本将 [AgoraRtcRemoteVideoStats](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcRemoteVideoStats.html) 类中的 `receivedFrameRate` 参数更名为 [rendererOutputFrameRate](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcRemoteVideoStats.html#//api/name/rendererOutputFrameRate)。

### **新增特性**

#### 1、添加媒体附属信息

常见的直播场景中，主播给观众分发商品链接、优惠券、在线答题等，能构建更为丰富的直播互动方式。为满足该部分社交类直播 App 开发者的需求，该版本新增 [setMediaMetadataSource](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setMediaMetadataDataSource:withType:) 和 [setMediaMetadataDelegate](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setMediaMetadataDelegate:withType:) 接口以及 [AgoraMediaMetadataSource](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraMediaMetadataDataSource.html) 和 [AgoraMediaMetadataDelegate](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraMediaMetadataDelegate.html) 协议，目前允许主播在发出的视频帧中添加 Metadata，发送媒体附属信息。

#### 2、优化屏幕共享

为确保屏幕共享画面完整性，保持共享屏幕的宽高比，避免画面裁剪和拉伸，该版本对屏幕共享的编码策略进行了优化。当共享屏幕的分辨率宽高比与你在 [AgoraScreenCaptureParameters](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraScreenCaptureParameters.html) 中设置的 [dimensions](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraScreenCaptureParameters.html#//api/name/dimensions) 的宽高比不一致时，Agora 按如下策略进行编码。假设 `dimensions` 值为 1920 x 1080，即 2073600 像素：

- 如果屏幕分辨率小于 `dimensions`，如 1000 x 1000，SDK 直接按 1000 x 1000 进行编码
- 如果屏幕分辨率大于 `dimensions`，如 2000 x 1500，SDK 按屏幕分辨率的宽高比 4：3，取 `dimensions` 以内的最大分辨率，即 1440 x 1080， 进行编码

屏幕共享按用户在 [AgoraScreenCaptureParameters](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraScreenCaptureParameters.html) 类下设置的 [dimensions](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraScreenCaptureParameters.html#//api/name/dimensions) 值进行计费。如果用户没有设置 `dimensions` 的值，SDK 会按 1920 x 1080 计费。

同时，为方便用户选择屏幕共享时是否采集鼠标，该版本在 `AgoraScreenCaptureParameters` 类中新增 [captureMouseCursor](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraScreenCaptureParameters.html#//api/name/captureMouseCursor) 参数。该参数默认采集鼠标。

#### 3、本地视频状态回调

为方便开发者了解本地视频状态，该版本新增 [rtcEngineLocalVideoStateChange](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngineLocalVideoStateChange:localVideoState:error:) 回调。该回调下，本地视频有 `Stopped`、`Capturing`、`Encoding` 和 `Failed` 四种状态。当本地视频状态为 `Failed` 时，用户可以参考该回调 `error` 参数返回的错误码进行问题排查。该回调能帮助开发者辨别本地视频故障是由采集还是编码引起的。原有的 `rtcEngineCameraDidReady` 和 `rtcEngineVideoDidStop` 回调在该版本废弃，我们不再推荐。

#### 4、推流状态回调

为方便推流用户了解当前的推流状态，并在推流出错时了解原因排查问题，该版本新增 [rtmpStreamingChangedToState](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:rtmpStreamingChangedToState:state:errorCode:) 回调。该回调下，推流有 `Idle`、`Connecting`、`Running`、`Recovering` 和 `Failure` 五种状态。当推流状态为 FAILURE 时，用户可以参考该回调 `errCode` 参数返回的错误码进行问题排查。原有的 `streamPublishedWithUrl` 和 `streamUnpublishedWithUrl` 回调仍可以使用，但我们不再推荐。

#### 5、网络连接失败原因梳理

为方便开发者更好地排查网络连接相关故障，该版本梳理并整合了网络连接相关的错误码，在原有 [connectionChangedToState](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:connectionChangedToState:reason:) 回调的 `reason` 参数中新增八个导致网络连接失败的原因。新增后，只要网络连接发生错误，SDK 都会返回该回调。同时该版本废弃了原有的警告码 `AgoraWarningCodeLookupChannelRejected(105)` 和错误码 `AgoraErrorCodeTokenExpired(109)`、`AgoraErrorCodeInvalidToken(110)`。

#### 6、本地网络连接类型回调

为方便用户了解本地网络的连接类型，该版本新增 [networkTypeChangedToType](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:networkTypeChangedToType:) 回调。该回调下，本地网络连接有 `Unknown`、`Disconnected`、`Lan` `Wifi`、`2G`、`3G`、`4G` 六种类型。当网络连接短暂中断时，该回调能帮助开发者辨别引起中断的原因是网络切换还是网络条件不好。

#### 7、获取播放伴奏音量

为方便开发者获取伴奏的播放音量，排查音量相关问题，该版本新增 [getAudioMixingPlayoutVolume](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustAudioMixingPlayoutVolume:) 和 [getAudioMixingPublishVolume](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustAudioMixingPublishVolume:) 方法，用以分别获取音乐文件在本地和远端的播放音量。

#### 8、精确回调远端音频首帧解码

为更精准地获取远端用户的出声时间，该版本新增 [firstRemoteAudioDecodedOfUid](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:firstRemoteAudioFrameDecodedOfUid:elapsed:) 回调，用以向 App 层报告 SDK 已完成远端音频首帧解码。在远端用户加入频道后首次发送音频，或远端用户 15 秒不发音频后再次发送时，该回调均会被触发。该回调与 `firstRemoteAudioFrameOfUid` 的区别在于，`firstRemoteAudioFrameOfUid` 在收到首个音频包时触发，先于 `firstRemoteAudioDecodedOfUid`。

### **改进**

#### 1、质量透明

- 该版本在通话相关的统计信息 [AgoraChannelStats](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraChannelStats.html) 类中，新增和 [txPacketLossRate](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraChannelStats.html#//api/name/txPacketLossRate) 和  [rxPacketLossRate](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraChannelStats.html#//api/name/rxPacketLossRate) 参数，分别返回本地客户端到服务器和服务器到本地客户端的丢包率。
- 该版本对 AgoraLocalVideoStats 和 AgoraRemoteVideoStats 类作了如下变动，方便用户更精准地获取本地和远端视频流的统计信息：
  - [AgoraRtcLocalVideoStats](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcLocalVideoStats.html)：新增 [encoderOutputFrameRate](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcLocalVideoStats.html#//api/name/encoderOutputFrameRate) 和 [rendererOutputFrameRate](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcLocalVideoStats.html#//api/name/rendererOutputFrameRate) 参数
  - [AgoraRtcRemoteVideoStats](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcRemoteVideoStats.html)：新增 [decoderOutputFrameRate](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcRemoteVideoStats.html#//api/name/decoderOutputFrameRate) 参数，并将原有的 receivedFrameRate 参数更名为 [rendererOutputFrameRate](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcRemoteVideoStats.html#//api/name/rendererOutputFrameRate)


#### 2、其他改进

- 优化了[AgoraAudioScenario](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Constants/AgoraAudioScenario.html) 为 `GameStreaming` 时的音质效果
- 优化了部分场景下语音和视频的延时
- SDK 包大小降低约 0.5 M
- 提高了用户修改视频属性的码率后，网络质量打分的准确性
- 默认启用音频质量通知回调。开发者无需调用 enableAudioQualityIndication 方法，也可以收到 [remoteAudioStats](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:remoteAudioStats:) 回调
- 提升了视频服务的稳定性

### **问题修复**

#### 视频

- 修复了特殊场景下无法自由切换屏幕共享流和摄像头流的问题

#### 其他

- 修复了用户退出频道后仍然收到 `networkQuality` 回调的问题
- 修复了偶现的崩溃问题，提升了系统稳定性

### **API 变更**

为提升用户体验，Agora 在 v2.4.1 版本中对 API 进行了如下变动：

#### 全平台 C++ 接口行为一致

从该版本起，Native SDK 保证了各平台 C++ 接口的行为一致性，方便用户通过统一的 C++ 接口，在各平台保持业务逻辑一致。同时在 C++ 头文件的 `IRtcEngine` 类中实现了 `RtcEngineParameters` 类下的所有方法。各接口的适用范围及使用注意事项详见 [Agora C++ API Reference for All Platforms 首页](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/cpp/index.html)。

#### 新增

- [getAudioMixingPlayoutVolume](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/getAudioMixingPlayoutVolume)
- [getAudioMixingPublishVolume](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/getAudioMixingPublishVolume)
- [firstRemoteAudioDecodedOfUid](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:firstRemoteAudioFrameDecodedOfUid:elapsed:)
- [rtcEngineLocalVideoStateChange](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngineLocalVideoStateChange:localVideoState:error:)
- [networkTypeChangedToType](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:networkTypeChangedToType:)
- [rtmpStreamingChangedToStats](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:rtmpStreamingChangedToState:state:errorCode:)
- [setMediaMetadataSource](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setMediaMetadataDataSource:withType:) 
- [setMediaMetadataDelegate](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setMediaMetadataDelegate:withType:) 
-  [AgoraMediaMetadataSource](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraMediaMetadataDataSource.html) 
- [AgoraMediaMetadataDelegate](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraMediaMetadataDelegate.html)
- `AgoraLiveTranscoding` 类新增参数 [audioCodecProfile](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraLiveTranscoding.html#//api/name/audioCodecProfile)
- `AgoraScreenCaptureParameters` 类新增参数 [captureMouseCursor](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraScreenCaptureParameters.html#//api/name/captureMouseCursor)（macOS/Windows）
- `AgoraChannelStats` 类新增参数 [txPacketLossRate](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraChannelStats.html#//api/name/txPacketLossRate) 和 [rxPacketLossRate](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraChannelStats.html#//api/name/rxPacketLossRate)
- `AgoraRtcLocalVideoStats` 类新增参数 [encoderOutputFrameRate](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcLocalVideoStats.html#//api/name/encoderOutputFrameRate) 和 [rendererOutputFrameRate](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcLocalVideoStats.html#//api/name/rendererOutputFrameRate)
- `AgoraRtcRemoteVideoStats` 类新增参数 [decoderOutputFrameRate](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcRemoteVideoStats.html#//api/name/decoderOutputFrameRate) 和 [rendererOutputFrameRate](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcRemoteVideoStats.html#//api/name/rendererOutputFrameRate)（替换 receivedFrameRate）

#### 废弃

- `enableAudioQualityIndication`
- `rtcEngineCameraDidReady`
- `rtcEngineVideoDidStop`
- 警告码 `AgoraWarningCodeLookupChannelRejected(105)`
- 错误码 `AgoraErrorCodeTokenExpired(109)`
- 错误码 `AgoraErrorCodeInvalidToken(110)`
- 错误码 `AgoraErrorCodeStartCamera(1003)`

## **2.4.0 版**

该版本于 2019 年 4 月 1 日发布。新增特性、功能改进与修复问题列表详见下文。

### **升级必看**

如果你希望通过 CocoaPods 自动导入库，请确保在运行 `pod install` 前，先运行 `pop update` 更新本地 CocoaPods 库。如果你希望通过指定 SDK 版本号获取最新版，请在 Podfile 中将版本号指定为 `'AgoraRtcEngnine_macOS', '2.4.0.1'`。

### **新增特性**

#### 1. 高级屏幕共享

该版本针对原有的屏幕共享进行了升级，并支持如下功能：

- 多屏环境下共享指定屏幕或屏幕内的部分区域（[`startScreenCaptureByDisplayId`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/startScreenCaptureByDisplayId:rectangle:parameters:)）
- 共享指定窗口或窗口内的部分区域（[`startScreenCaptureByWindowId`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/startScreenCaptureByWindowId:rectangle:parameters:)）
- 根据屏幕共享的内容类型设置运动优先或细节优先（[`setScreenCaptureContentHint`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/setScreenCaptureContentHint:)）
- 单独设置屏幕共享的分辨率、帧率和码率（[`updateScreenCaptureParameters`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/updateScreenCaptureParameters:)）

该版本废弃了原有的 `startScreenCapture` 接口。Agora 推荐你使用新接口实现屏幕共享 。新接口下，用户需要在设计获取 `displayId` 和 `windowId` 的代码逻辑，详情请参考[开始屏幕共享](../../cn/Audio%20Broadcast/screensharing_mac.md)。

#### 2. 变声和混响

在语音聊天室场景中添加变声和混响效果，能有效增强社交的趣味性。该版本在原有音效设置接口的基础上，新增 [`setLocalVoiceChanger`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/setLocalVoiceChanger:) 和 [`setLocalVoiceReverbPreset`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/setLocalVoiceReverbPreset:) 方法，开发者无需手动设置音效参数，直接选择想要的本地语音变声或混响效果。详情请参考[变声与混响](../../cn/Audio%20Broadcast/voice_effect_mac.md)。

#### 3. 听声辨位

该版本新增 [`enableSoundPositionIndication`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/enableSoundPositionIndication:) 和 [`setRemoteVoicePosition`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/setRemoteVoicePosition:pan:gain:) 方法，支持本地用户听声辨位。用户需要在加入频道前调用 [`enableSoundPositionIndication`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/enableSoundPositionIndication:) 开启远端用户的语音立体声，然后在 [`setRemoteVoicePosition`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/setRemoteVoicePosition:pan:gain:) 中设置远端用户声音出现的位置，通过左右耳听到的声音差异，对远端用户的声音产生方位感。在多人在线游戏场景，如射击游戏中，该功能可以增加游戏角色的方位感，模拟真实场景。

#### 4. 通话前 Last-mile 网络探测

在通话前进行 Last-mile 网络探测，可以有效帮助本地用户判断和预测上行网络质量是否良好。该版本新增通话前 Last-mile 网络探测接口 [`startLastmileProbeTest`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/startLastmileProbeTest:)、[`stopLastmileProbeTest`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/stopLastmileProbeTest) 及 [`lastmileProbeResult`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/v2.4/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:lastmileProbeTestResult:)，向用户反馈开始通话前上下行网络的带宽、丢包、网络抖动和往返时延数据。

#### 5. 音频设备回路测试

该版本新增音频设备回路测试接口 [`startAudioDeviceLoopbackTest`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/startAudioDeviceLoopbackTest:) 与 [`stopAudioDeviceLoopbackTest`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/stopAudioDeviceLoopbackTest)，用于测试本地的麦克风和播放设备能否正常工作。该测试在本地进行，不涉及网络传输。

#### 6. 设置用户媒体流优先级

该版本新增接口 [`setRemoteUserPriority`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/setRemoteUserPriority:type:) 用于设置远端用户媒体流的优先级。该方法可以与 [`setRemoteSubscribeFallbackOption`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/setRemoteSubscribeFallbackOption:) 搭配使用。如果开启了订阅流回退选项，弱网下 SDK 会优先保证高优先级用户收到的流的质量。

#### 7. 音乐文件播放状态回调

该版本为播放音乐文件新增回调 [`localAudioMixingStateDidChanged`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/v2.4/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:localAudioMixingStateDidChanged:errorCode:)，方便用户获知音乐文件的播放状态（成功/失败），以及播放出错的原因。同时新增一个警告码 701，当播放音乐文件时，本地音乐文件不存在、文件格式不支持或无法访问在线音乐文件 URL 时，均会触发该警告码。

#### 8. 设置日志文件大小

Agora SDK 有 2 个日志文件，每个文件默认大小为 512 KB。为解决该大小无法满足部分用户需求的问题，该版本新增接口 [`setLogFileSize`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/setLogFileSize:)，用于设置 SDK 输出的日志文件大小。

### **功能改进**

#### 1. 质量测试与透明

- 该版本使用新的 [`startEchoTestWithInterval`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/startEchoTestWithInterval:successBlock:) 接口取代原有的 [`startEchoTest`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/startEchoTest:)，新增参数 `intervalInSeconds`，用于设置返回测试结果的时间间隔。
- 该版本在本地视频流统计信息 [`AgoraRtcLocalVideoStats`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcLocalVideoStats.html) 类中新增 [`sentTargetBitrate`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcLocalVideoStats.html#//api/name/sentTargetBitrate)，[`sentTargetFrameRate`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcLocalVideoStats.html#//api/name/sentTargetFrameRate)，[`qualityAdaptIndication`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcLocalVideoStats.html#//api/name/qualityAdaptIndication) 三个参数，分别反映目标码率、目标帧率与和上次返回的本地视频流统计信息相比，本地视频质量的自适应情况。

#### 2. 视频偏好设置

一般场景下，Agora 默认的视频编码配置能满足需求。对于特定场景，该版本提供如下功能让用户选择视频偏好：

- 弱网下画质或流畅偏好设置。该版本在视频编码属性 [`AgoraVideoEncoderConfiguration`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraVideoEncoderConfiguration.html) 类中新增 2 个参数 [`minFrameRate`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraVideoEncoderConfiguration.html#//api/name/minFrameRate) 和 [`degradationPreference`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraVideoEncoderConfiguration.html#//api/name/degradationPreference)，分别用于设置最低视频编码帧率，以及带宽受限时编码帧率的偏好。这两个参数需要搭配使用，详情请参考[设置视频属性](../../cn/Audio%20Broadcast/videoProfile_mac.md)。

- 采集时预览或性能偏好设置。该版本新增接口 [`setCameraCapturerConfiguration`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/setCameraCapturerConfiguration:)，通过设置摄像头采集偏好，用户可以根据实际场景选择优先保证设备性能还是视频质量。具体场景及参数选择，请参考 [API 文档](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/setCameraCapturerConfiguration:)。

#### 3. 核心质量改进

- 降低了音频延时
- 提升了视频质量和稳定性
- 缩短了远端视频的出图时间
- 改善了屏幕共享直播弱网下视频的流畅性和延迟
- 优化了 CPU 和内存占用

### **问题修复**

#### 音频相关

- 修复了调用 `enableLocalAudio` 接口导致的蓝牙断开的问题
- 新增支持中文字符音乐
- 修正 `pushExternalAudioFrameSamplBuffer` 成功的返回值为 Yes
- 修复了高音声音变弱的问题
- 修复了偶现的声音快放问题
- 修复了使用 `getAudioPlaybackDevices` 方法无法获取虚拟声卡的问题

#### 视频相关

- 通过增加 `renderMode` 的默认值，修复了用户在没有设置的情况 下，窗口和画面比例不符合引发的拉伸问题
- 部分低性能设备上出现的播放视频卡住的问题
- 优化了 SDK 出图时间
- 修复了虚拟摄像头不支持 640 x 480 分辨时，Electron SDK 崩溃的问题
- 修复了共享窗口时，鼠标位置不对的问题

#### 其他

- 修复了转码推流场景下，SEI 信息与媒体流不同步的问题

### **API 整理**

为提升用户体验，Agora 在 v2.4.0 版本中对 API 进行了如下变动：

#### 新增

- [`setBeautyEffectOptions`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/setBeautyEffectOptions:options:)
- [`startScreenCaptureByDisplayId`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/startScreenCaptureByDisplayId:rectangle:parameters:)
- [`startScreenCaptureByWindowId`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/startScreenCaptureByWindowId:rectangle:parameters:)
- [`updateScreenCaptureParameters`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/updateScreenCaptureParameters:)
- [`setScreenCaptureContentHint`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/setScreenCaptureContentHint:)
- [`setLocalVoiceChanger`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/setLocalVoiceChanger:)
- [`setLocalVoiceReverbPreset`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/setLocalVoiceReverbPreset:)
- [`enableSoundPositionIndication`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/enableSoundPositionIndication:)
- [`setRemoteVoicePosition`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/setRemoteVoicePosition:pan:gain:)
- [`startLastmileProbeTest`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/startLastmileProbeTest:)
- [`stopLastmileProbeTest`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/stopLastmileProbeTest)
- [`setRemoteUserPriority`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/setRemoteUserPriority:type:)
- [`startEchoTestWithInterval`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/startEchoTestWithInterval:successBlock:)
- [`startAudioDeviceLoopbackTest`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/startAudioDeviceLoopbackTest:)
- [`stopAudioDeviceLoopbackTest`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/stopAudioDeviceLoopbackTest)
- [`setCameraCapturerConfiguration`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/setCameraCapturerConfiguration:)
- [`setLogFileSize`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraRtcEngineKit.html#//api/name/setLogFileSize:)
- [`localAudioMixingStateDidChanged`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/v2.4/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:localAudioMixingStateDidChanged:errorCode:)
- [`lastmileProbeResult`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/v2.4/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:lastmileProbeTestResult:)

#### 废弃

- `startEchoTest`
- `startScreenCapture`
- `setVideoQualityParameters`

#### 其他

[`AgoraVideoEncoderConfiguration`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraVideoEncoderConfiguration.html) 类中的 [`frameRate`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/v2.4/Classes/AgoraVideoEncoderConfiguration.html#//api/name/frameRate) 参数由 `enum` 型修改为 `int` 型。

## **2.3.3 版**

该版本于 2019 年 1 月 24 日发布。功能改进与修复问题详见下文。

### **改进**

为提升用户体验，屏幕共享做了大量算法优化。针对不同的共享场景提供不同的屏幕共享策略，尤其针对 PPT 翻页和网页浏览场景，提升了屏幕共享过程中的流畅度和清晰度。同时改善了通信模式下屏幕共享开启阶段画面模糊的现象。

### **问题修复**

修复了 `networkQuality` 回调不准确的问题。


## **2.3.2 版**

该版本于 2019 年 1 月 16 日发布。新增特性与修复问题详见下文。

### 升级前必看

2.3.2 除了下文提到的功能和改进外，整体提升直播模式下视频弱网下抗丢包能力，提高流畅度，降低卡顿率。升级前，请了解版本兼容性:

- Native SDK 版本号须大于等于 1.11 版本
- Web SDK 版本号须大于等于 2.1 版本

### **新增功能**

#### 1. 提升直播清晰度

Agora SDK 会根据网络条件进行码率自适应。为满足用户在直播场景下对视频清晰度的要求，该版本在 [setVideoEncoderConfiguration](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setVideoEncoderConfiguration:) 接口中新增 [minBitrate](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraVideoEncoderConfiguration.html#//api/name/minBitrate) 参数，强制视频编码器输出高质量图片。如果将参数设为高于默认值，在网络状况不佳情况下可能会导致网络丢包，并影响视频播放的流畅度。因此如非对画质有特殊需求，Agora 建议不要修改该参数的值。

#### 2. 控制音乐文件的播放音量

为方便用户控制混音音乐文件的播放音量，该版本在已有 [`adjustAudioMixingVolume`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustAudioMixingVolume:) 的基础上新增 [`adjustAudioMixingPlayoutVolume`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustAudioMixingPlayoutVolume:) 和 [`adjustAudioMixingPublishVolume`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustAudioMixingPublishVolume:) 接口，用于分别控制混音音乐文件在本地和远端的播放音量。

该版本梳理了用户在音频采集到播放过程中可能会需要调整音量的场景，及各场景对应的 API，供用户参考使用。详见官网文档[调整通话音量](../../cn/Audio%20Broadcast/volume_mac.md)。

#### 3. 直播中弱网环境下视频自动回退/重开

网络不理想的环境下，直播音视频的质量都会下降。为提升直播效率，Agora 新增了 [`setLocalPublishFallbackOption`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLocalPublishFallbackOption:) 和 [`setRemoteSubscribeFallbackOption`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setRemoteSubscribeFallbackOption:) 两个接口。用户设置这两个接口后，在网络条件差、无法同时保证音视频质量的情况下，SDK 会自动将视频流从大流切换为小流，或直接关闭视频流，从而保证或提高音频质量。同时 SDK 会持续监控网络质量， 并在网络质量改善时恢复音视频流。在推流回退为音频流时，或由音频流恢复为音视频流，触发 [`didLocalPublishFallbackToAudioOnly`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didLocalPublishFallbackToAudioOnly:) 或 [`didRemoteSubscribeFallbackToAudioOnly`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didRemoteSubscribeFallbackToAudioOnly:byUid:) 回调。

#### 4. 按用户返回音视频上下行码率、帧率、丢包率及延迟

为方便统计每个用户的音视频上下行码率、帧率及丢包率，该版本新增 [`audioTransportStatsOfUid`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:audioTransportStatsOfUid:delay:lost:rxKBitRate:) 和 [`videoTransportStatsOfUid`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:videoTransportStatsOfUid:delay:lost:rxKBitRate:) 回调。 通话或直播过程中，当用户收到远端用户发送的音视频数据包后，会周期性地发生该回调上报，频率约为 2 秒 1 次。 回调中包含用户的 UID、音/视频接收码率、丢包率、以及延迟时间（毫秒）。 并在统计频道内通话相关数据的 AgoraChannelStats 类中增加 [`lastmileDelay`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraChannelStats.html?transId=9fa366f0-01e7-11e9-a659-33e4b5b761ac#//api/name/lastmileDelay) 参数，返回客户端到 vos 服务器的延迟。

#### 5. 设置视频编码属性

为满足场景中视频旋转的需要，提升自定义视频源画质，该版本引入 [`setVideoEncoderConfiguration`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setVideoEncoderConfiguration:) 替换原 [`setVideoProfile`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setVideoProfile:swapWidthAndHeight:) 接口，来设置视频编码属性。 新接口中的 [`AgoraVideoEncoderConfiguration`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraVideoEncoderConfiguration.html) 类对应一套视频参数，包含视频的分辨率 `dimension`、帧率 `frame rate`、码率 `bitrate`、最低编码码率 `minBitrate` 以及视频方向 `orientationMode`，其中 Agora 建议保留 `minBitrate` 的默认设置。原接口 `setVideoProfile` 仍可使用，但不再推荐。

#### 6. 虚拟声卡采集

为提升声卡采集易用性，该版本在 [enableLoopbackRecording](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/enableLoopbackRecording:deviceName:) 接口中新增参数 `deviceName`，支持用户使用虚拟声卡进行采集。该参数 NULL 时默认使用当前声卡采集。如需使用虚拟声卡，直接使用虚拟声卡的产品名传参即可。

### **改进**

#### 1. 提供更透明的质量数据统计

为提升质量透明的用户体验，该版本废弃了原有的 `audioQualityOfUid` 回调，并新增 `remoteAudioStats` 回调进行取代。和原来的接口相比，新接口使用更为综合的算法，通过引入音频丢帧率、端到端的音频延迟、接收端网络抖动的缓冲延迟等参数，使回调结果更贴近用户感受。同时，该版本优化了 `networkQuality` 的算法，对上下行网络质量采用不同的计算方法，使评分更精准。

- [`remoteAudioStats`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:remoteAudioStats:)：通话中远端音频流的统计信息回调。用于替换	`audioQualityOfUid`
- [`networkQuality`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:networkQuality:txQuality:rxQuality:)：通话中每个用户的网络上下行 Last mile 质量报告回调。

Agora SDK 计划在下一个版本对如下 API 进行进一步改进：

- [`lastmileQuality`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:lastmileQuality:)：通话前网络上下行 Last mile 质量报告回调

该版本对数据统计相关回调进行了统一梳理，相关回调及算法详见[通话中数据统计](../../cn/Audio%20Broadcast/in_call_statistics_macos.md)。

#### 2. 改进获取 SDK 网络连接状态的生成策略

为提升 SDK 使用数据统计的准确性和合理性，该版本新增如下接口，用以获取 SDK 的网络连接状态，以及连接状态发生改变的原因。

- [`getConnectionState`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/getConnectionState)：获取 SDK 的网络连接状态
- [`connectionChangedToState`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:connectionChangedToState:reason:)：SDK 的网络连接状态已改变回调

该版本废弃了原有的 [`rtcEngineConnectionDidInterrupted`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngineConnectionDidInterrupted:) 和 [`rtcEngineConnectionDidBanned`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngineConnectionDidBanned:) 回调。

在新的接口下，SDK 共有 5 种连接状态：未连接、正在连接、已连接、正在重新建立连接和连接失败。当连接状态发生改变时，都会触发 `connectionChangedToState` 回调。当条件满足时，原有的 `rtcEngineConnectionDidInterrupted` 和 `rtcEngineConnectionDidBanned` 回调也会触发，但 Agora 不再推荐使用。


#### 3. 优化打分反馈机制

为方便用户（开发者）收集最终用户（应用程序使用者）对使用应用进行通话或直播的反馈，该版本将 [`rate`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/rate:rating:description:) 接口中的打分范围由 1 - 10 修改为 1 - 5，减少最终用户的打分干扰。Agora 建议在应用程序中集成该接口，方便应用程序收集用户反馈。


#### 4. 其他改进

- 优化了直播模式下视频弱网抗丢包能力
- 加快了严重拥塞状态视频的恢复速度s
- 优化了 API 的调用线程
- 增加了重新检测耳机插入和蓝牙设备连接的代码
- 降低了音频延时
- 优化了 macOS 设备的视频采集方式，降低了性能消耗


### **问题修复**

#### SDK 相关：

- 修复了 SDK 在 macOS 设备上出现的崩溃问题

#### 音频相关：

- 修复了设备在连接蓝牙的状态下，退出频道后，音频不走蓝牙的问题
- 修复了调用 startAudioMixing 播放音乐文件时出现的崩溃问题
- 修复了麦克风在禁用的状态下，设备插上耳机后，禁用失效的问题
- 修复了外放条件下，上下麦、系统电话打断、Siri 中断、进退频道等多种场景下，出现的无法调节外放音量的问题
- 修复了应用从后台切回前台时，出现的出声音慢的问题

#### 视频相关：

- 修复了因视频编码问题引起的 Native 设备与 Web 端互通时，Web 端看不到 Native 端视频画面的问题
- 修复了 x86 设备上自采集图像时硬件编码器的相关问题
- 修复了视频自采集时的偶现问题


### **API 整理**

为提升用户体验，Agora 在 v2.3.2 版本中对 API 进行了如下变动：

#### 新增

- [`setVideoEncoderConfiguration`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setVideoEncoderConfiguration:)
- [`setLocalPublishFallbackOption`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setLocalPublishFallbackOption:)
- [`setRemoteSubscribeFallbackOption`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setRemoteSubscribeFallbackOption:)
- [`getConnectionState`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/getConnectionState)
- [`adjustAudioMixingPlayoutVolume`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustAudioMixingPlayoutVolume:)
- [`adjustAudioMixingPublishVolume`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/adjustAudioMixingPublishVolume:)
- [`connectionChangedToState`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:connectionChangedToState:reason:)
- [`didLocalPublishFallbackToAudioOnly`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didLocalPublishFallbackToAudioOnly:)
- [`didRemoteSubscribeFallbackToAudioOnly`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didRemoteSubscribeFallbackToAudioOnly:byUid:)
- [`remoteAudioStats`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:remoteAudioStats:)
- [`audioTransportStatsOfUid`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:audioTransportStatsOfUid:delay:lost:rxKBitRate:)
- [`videoTransportStatsOfUid`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:videoTransportStatsOfUid:delay:lost:rxKBitRate:)


#### 废弃

- [`setVideoProfile`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setVideoProfile:swapWidthAndHeight:)
- [`audioQualityOfUid`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:audioQualityOfUid:quality:delay:lost:)
- [`rtcEngineConnectionDidInterrupted`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngineConnectionDidInterrupted:)
- [`rtcEngineConnectionDidBanned`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngineConnectionDidBanned:)

## **2.2.3 版**

该版本于 2018 年 7 月 5 日发布。新增特性与修复问题列表详见下文。

### **升级必看**

为更好地提升用户体验，Agora SDK 在 2.1 版本中对动态秘钥进行了升级。 如果你当前使用的 SDK 是 2.1 之前的版本，并希望升级到 2.1 或更高版本，请务必参考 [动态秘钥升级说明](../../cn/Agora%20Platform/token_migration.md) 。

### **问题修复**

-   修复了特定场景下偶发的线上统计崩溃的问题。

-   修复了直播时特定场景下偶发的崩溃的问题。

-   修复了多人直播连麦时，SDK 内存增长的问题。

-   修复了特定场景下偶发的视频窗口尺寸变化后，视频卡住的问题。

-   修复了偶发的无法正常反馈频道内谁在说话以及说话者的音量的问题。


## **2.2.2 版**

该版本于 2018 年 6 月 21 日发布。修复问题列表详见下文。

### **修复问题**

- 修复了特定场景下偶发的线上统计崩溃的问题
- 修复了特定场景下偶发的视频窗口尺寸变化后，视频卡住的问题
- 修复了偶发的无法正常反馈频道内谁在说话以及说话者的音量的问题

## **2.2.1 版**

该版本于 2018 年 5 月 30 日发布。内部代码优化。

## **2.2.0 版**

该版本于 2018 年 5 月 4 日发布。新增特性与修复问题列表详见下文。

### **新增功能**

本次发版新增如下功能：

#### 1. 音效混响进频道

播放音效 `playEffect` 接口新增了一个 `publish` 参数，用于在播放音效时，远端用户可以听到本地播放的音效。
 

> 如果你的 SDK 是由之前版本升级到 v2.2 版本，请务必关注该接口功能的变动。

#### 2. 服务端部署代理服务器

通过部署 Agora 提供的代理服务器安装包，设有企业防火墙的用户可以设置代理服务器，使用 Agora 的服务。详见 [企业部署代理服务器](../../cn/Quickstart%20Guide/proxy.md) 中的描述。

#### 3. 获取远端视频状态

新增 `onRemoteVideoStateChanged` 接口，以获知远端视频流的状态。

#### 4. 直播添加视频水印

在本地直播及旁路直播中增加水印功能，允许用户将一张 PNG 图片作为水印添加到正在进行的本地直播或旁路直播中。新增 `addVideoWatermark` 和 `clearVideoWatermarks` 接口，以添加或删除本地直播水印；`LiveTranscoding `接口中新增 `watermark` 参数，用于控制旁路直播中水印的添加。

### **改进功能**

本次发版改进如下功能：

#### 1. 当前说话者音量提示

改进 `enableAudioVolumeIndication `接口的功能，无论频道内是否有人说话，都会在回调中按设置的时间间隔返回说话者音量提示。

#### 2. 频道内网络质量监测

根据用户对频道内实时网络质量监测的需求，在 `onNetworkQuality` 中改进了返回数据的准确度。

#### 3. 进入频道前网络条件监测

为方便用户在仅频道前检查当前网络是否能支撑语音或视频通话，在 `onLastmileQuality` 中，由通过恒定码率监测优化为根据用户设定的 Video Profile 的码率进行监测，提高返回数据的准确度。且在网络状态为 unknown 时，依然以 2 秒的间隔返回回调。

#### 4. 提升音乐场景下的音质

提升了用户在播放音乐等场景下的音乐音质。

### **问题修复**

-   修复了 macOS 设备上偶现的 crash 问题

-   修复了大量用户同时连麦直播时，偶发的抖屏现象


## **2.1.3 版**

该版本于 2018 年 4 月 19 日发布。新增特性与修复问题列表详见下文。

### **升级必看**

该版本的 SDK 修改了 `setVideoProfile `方法在直播模式下的码率值，修改后的码率值与 2.0 版本一致。

### **问题修复**

-   修复了 SDK 没有设置 Delegate 时，偶尔收不到 Block 回调的问题。

-   修复了 SDK 的外链符号里，有 NSAssertionHandler 的问题。

-   修复了部分手机上，用户离开频道后，开启自带的录音设备时，偶现录音出错的问题。


### **改进**

改进了通信和直播模式下屏幕共享的效果，缩短了用户从屏幕共享模式切换回普通模式需要的时间间隔。

## **2.1.2 版**

该版本于 2018 年 4 月 2 日发布。新增特性与修复问题列表详见下文。

### **升级必看**

SDK 升级至 2.1.2 的直播模式后，相同分辨率下，视频更清晰，但带宽也会变大。

### **新增功能**

在已有 `setVideoProfile` 接口的基础上，新增一个 `setVideoResolution` 接口。用户可以用此接口，根据自身业务需要，手动设置视频的分辨率、帧率和码率。

### **问题修复**

修复了通信模式的屏幕共享清晰度小于直播模式的问题。

## **2.1.1 版**

该版本于 2018 年 3 月 16 日发布。

请正在或已集成 2.1 SDK 的客户尽快升级更新！ 本次发版修复了一个的系统风险，请尽快升级以免对服务造成影响。

## **2.1.0 版**

该版本于 2018 年 3 月 7 日发布。新增特性与修复问题列表详见下文。

### **新增功能**

本次发版新增如下功能：

#### 1. 开黑

新增了一个游戏开黑场景，用于节省流量和去除杂音，通过调用 API `setAudioProfile` 实现。

#### 2. 音效均衡和音效混响

在直播场景下，主播如果需要通过内置的麦克风美化和定制自己的语音输入，可以通过调用 API `setLocalVoiceEqualization` 和 `setLocalVoiceReverb` 轻易地设置音效均衡和混响来实现所需要的效果。

#### 3. 在线频道信息查询

新增 Restful API 查询用户在频道中的状态信息，查询指定频道内的分角色用户列表，查询厂商频道列表，查询用户是否为连麦用户等。详见:

- 通话场景，详见 [Dashboard RESTful API](../../cn/Video/dashboard_restful_communication.md)
- 互动直播场景，详见 [Dashboard RESTful API](../../cn/Interactive%20Braodcast/dashboard_restful_live.md)

#### 4. 17 人视频

在直播场景下，同一频道内支持 17 位主播同时进行视频直播和连麦，详见文档:

- [实现视频直播](../../cn/Audio%20Broadcast/broadcast_video_mac.md)
- [实现七人以上视频通话](../../cn/Audio%20Broadcast/seventeen_people_iosmac.md)

#### 5. 自定义视频源

Agora SDK 提供了摄像头采集的默认实现，同时允许开发者使用自定义视频源。

#### 6. 自定义渲染器

Agora SDK 提供了默认的渲染器实现，用来显示本地视频图像和对端视频图像。使用默认的渲染器就能满足大部分开发者需求，复杂的业务场景下，Agora 也开放了自定义渲染器接口。

#### 7. 插入外部视频源

直播场景下，可以将采集到的视频添加到正在进行的直播中，直播室里的主播和观众可以一起边看电影、比赛或演出，边进行点评、互动等功能，会让现有的直播话题更广、体验更好。 仅支持拉入一路流，格式包括: RTMP, HLS, FLV。赛事直播最多同时支持 5 人连麦直播。


### **改进**

本次发版改进如下功能：

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>改进</td>
<td>描述</td>
</tr>
<tr><td>卡顿</td>
<td>改善了观众模式下的卡顿现象</td>
</tr>
<tr><td>卡顿</td>
<td>改善了特定设备导致的卡顿现象</td>
</tr>
<tr><td>鉴权模型优化</td>
<td>旧鉴权模型一个密钥只能对应一个服务权限(例如加入频道)，而新的一个 Token 包含了所有的服务权限(例如加入频道，连麦，推流等)</td>
</tr>
<tr><td>计费优化</td>
<td>计费系统针对极小分辨率统一按音频计费，例如 16 * 16</td>
</tr>
</tbody>
</table>



### **问题修复**

-   修复了自采集方案退出频道后 app 录不到声音的问题。

-   修复了偶现的崩溃。

-   修复了偶现的关闭麦克风无法听到声音的问题。

-   修复了偶现的黑屏问题。


## **2.0.2 版**

该版本于 2017 年 12 月 15 日发布。新增特性与修复问题列表详见下文。

### **问题修复**

修复了 ffmpeg 符号冲突问题;

## **2.0 版及之前**
### **2.0 版**

该版本于 2017 年 12 月 6 日发布。新增特性与修复问题列表详见下文。

#### **新增功能**

-   通信场景支持视频大小流功能，新增 API `setRemoteVideoStreamType()` 和 `enableDualStreamMode()` ;

-   伴奏和音效回调更新如下:

    <table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>rtcEngineMediaEngineDidAudioMixingFinish</code></td>
<td>废弃，已经被<code>rtcEngineLocalAudioMixingDidFinish</code>替代</td>
</tr>
<tr><td><code>rtcEngineDidAudioEffectFinish</code></td>
<td>新增，当本地结束音效播放时触发该回调</td>
</tr>
</tbody>
</table>



-   通信和直播场景下支持音频自采集功能，新增以下 API:

    <table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>名称</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>enableExternalAudioSourceWithSampleRate</code></td>
<td>开启外部音频采集</td>
</tr>
<tr><td><code>disableExternalAudioSource</code></td>
<td>关闭外部音频采集</td>
</tr>
<tr><td><code>pushExternalAudioFrameRawData</code></td>
<td>推送外部音频帧</td>
</tr>
</tbody>
</table>



-   通信和直播场景下支持服务端踢人功能。如有需要，请联系 [sales@agora.io](mailto:sales@agora.io) 开通该功能。


### **1.14 版**

该版本于 2017 年 10 月 20 日发布。新增特性与修复问题列表详见下文。

#### **新增功能**

-   新增 API `setAudioProfile`设置音频参数和应用场景。

-   新增 API `setLocalVoicePitch`提供基础变声功能。

-   直播场景: 新增 API `setInEarMonitoringVolume` 提供调节耳返音量功能。


#### **改进**

-   优化了在高码率下的音频体验;

-   秒开: 直播场景下，单流模式时观众加入频道 1 秒内看见主播图像\(均值为 858 ms, 网络状态良好时可达 625 ms\);

-   节省带宽:

    -   1.14 以前: 如果你选择不听某人的音频或不看某人的视频，音视频流会照发。

    -   1.14 开始: 如果你选择不听或不看某人的流，则不会下发，从而节省带宽。

-   精准的码率控制:

    -   1.14 以前: 码率控制不够精准，上下波动幅度较大。波动过大容易造成网络拥塞，增加丢包、丢帧的概率，影响了带宽估计模块的精度，特别是在弱网低码率情况下尤为明显。

    -   1.14 开始: 精准的码率控制，要多少给多少，不多给也不少给，避免波动过大造成的网络拥塞，减少传输延时，有助于减少网络卡顿。


### **1.13.1 版**

该版本于 2017 年 9 月 28 日发布。新增特性与修复问题列表详见下文。

#### **改进**

优化了特定场景下出现的回声问题。

### **1.13 版**

该版本于 2017 年 9 月 4 日发布。新增特性与修复问题列表详见下文。

#### **新增功能**

-   新增 API `didClientRoleChanged `用于提醒直播场景下主播、观众上下麦的回调

-   新增单独关闭语音播放的功能

-   新增功能支持服务端推流失败回调

-   屏幕共享直播场景下采集声卡选项动态开启和关闭


#### **改进**

-   软编情况下，视频属性可控

-   可以在客户端设置推流的 profile

-   屏幕共享: 提升了画质清晰度和流畅度

-   屏幕共享: 通信场景下新增动态更新截图区域


#### **修复问题**

修复了部分机型上偶现的崩溃

### **1.12 版**

该版本于 2017 年 7 月 25 日发布。新增特性与修复问题列表详见下文。

#### **新增功能**

-   直播场景下， 新增 API 方法 `injectStream` 在当前频道内插入一条 RTMP 流。该功能目前为 beta 版

-   在 API 方法 `setEncryptionMode` 里新增加密模式 `aes-128-ecb` 。

-   在 API 方法 `startAudioRecording` 里新增参数 quality 用于设置录音音质。

-   新增一系列 API 管理音效。

-   新增 API `activeSpeaker` 提示当前频道内谁在说话。

-   通信场景下，删除了原有的 API 方法 `setScreenCaptureWindow`，更新 API 方法 `startScreenCapture` 共享整个屏幕、指定窗口或指定区域。

-   通信场景下，启用屏幕共享功能后，在屏幕共享过程中可以显示鼠标。


#### **改进**

通信场景下针对 320 x 180 分辨率提供了以下改进方案:

-   网络和设备状态较差的情况下仍能保证画质流畅度。
-   网络和设备状态良好的情况下可以做到比 180P 更好的画质清晰度。


#### **修复问题**

修复了部分机型上偶现的崩溃问题。


