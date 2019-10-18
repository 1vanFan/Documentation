
---
title: 发版说明
description: 
platform: Web
updatedAt: Thu Sep 05 2019 09:57:17 GMT+0800 (CST)
---
# 发版说明
本文提供 Agora Web SDK 的发版说明。

## 概览

Agora Web SDK 是通过 HTML 网页加载的 JavaScript 库。 Agora Web SDK 库在网页浏览器中调用 API 建立连接，控制音视频通话和直播服务。点击 [语音通话产品概述](https://docs.agora.io/cn/Voice/product_voice?platform=All%20Platforms)、[视频通话产品概述](https://docs.agora.io/cn/Video/product_video?platform=All%20Platforms) 以及[互动直播产品概述](https://docs.agora.io/cn/Interactive%20Broadcast/product_live?platform=All%20Platforms)了解关键特性。

### 兼容性说明

下表列出目前 Agora Web SDK 对浏览器的支持情况：

<table>
  <tr>
    <th>平台</th>
    <th>Chrome 58+</th>
    <th>Firefox 56+</th>
    <th>Safari 11+</th>
    <th>Opera 45+</th>
    <th>QQ 浏览器最新版</th>
    <th>360 安全浏览器</th>
    <th>微信浏览器</th>
  </tr>
  <tr>
    <td>Android 4.1+</td>
    <td><font color="green">✔</td>
    <td><font color="red">✘</td>
		<td><b>N/A</b></td>
    <td><font color="red">✘</td>
    <td><font color="red">✘</td>
    <td><font color="red">✘</td>
    <td><font color="red">✘</td>
  </tr>
  <tr>
    <td>iOS 11+</td>
    <td><font color="red">✘</td>
    <td><font color="red">✘</td>
    <td><font color="green">✔</td>
    <td><font color="red">✘</td>
    <td><font color="red">✘</td>
    <td><font color="red">✘</td>
    <td><font color="red">✘</td>
  </tr>
  <tr>
    <td>macOS 10+</td>
    <td><font color="green">✔</td>
    <td><font color="green">✔</td>
    <td><font color="green">✔</td>
    <td><font color="green">✔</td>
    <td><font color="green">✔</td>
    <td><font color="red">✘</td>
    <td><font color="red">✘</td>
  </tr>
  <tr>
    <td>Windows 7+</td>
    <td><font color="green">✔</td>
    <td><font color="green">✔</td>
		<td><b>N/A</b></td>
    <td><font color="green">✔</td>
    <td><font color="green">✔</td>
    <td><font color="green">✔</td>
    <td><font color="red">✘</td>
  </tr>
</table>

<div class="alert info">除上表浏览器外，还有以下支持：
	<li>Agora Web SDK 2.5 及以上版本支持 Windows XP 平台的 Chrome 49 版本浏览器（仅支持 VP8 编码，不能与 Native SDK 互通）。</li>
	<li>Agora Web SDK 2.7 及以上版本支持 Windows 10 平台的 Edge 浏览器，详见 <a href="https://docs.agora.io/cn/faq/browser_support#a-nameedgeaedge">Edge 浏览器支持</a>。</li>
	<li>Agora Web SDK 理论上还支持 360 极速浏览器，但未经过验证，不保证全部功能正常工作。</li>
</div>
<div class="alert note">以下场景中请务必将 Agora Web SDK 升级至 2.6 或更高版本:
	<li>iOS 12.1.4 及以上版本使用 Safari 浏览器</li>
	<li>macOS 上使用 Safari 12.1 及以上版本</li>
	</div>

> 如需实现 Agora Native SDK 与 Agora Web SDK 的互通，必须将 Agora Native SDK 升级至 1.12 及以上版本。

### 已知问题和局限性

- 受浏览器策略影响，在 Chrome 70+ 和 Safari 浏览器上，`Stream.play`，`Stream.startAudioMixing` 以及 `Stream.getAudioLevel` 这三个方法必须由用户手势触发，详情请参考 [Autoplay Policy Changes](https://developers.google.com/web/updates/2017/09/autoplay-policy-changes)。
- 如果在客户端安装了高清摄像头，则 Agora Web SDK 支持最大为 1080p 分辨率的视频流，但取决于不同的摄像头设备，最大分辨率也会受到影响。
- 部分 iOS 设备在通话时将 Safari 浏览器切换至后台再恢复后，视频会出现黑边。
- Agora Web SDK 暂不支持代码二次混淆。

更多问题，详见 [Web 常见问题集](https://docs.agora.io/cn/search?type=faq&platform=Web)。

## 2.9.0 版
该版本于 2019 年 9 月 5 日发布。

**升级必看**

该版本优化了推流服务的性能和稳定性，对管理推流转码的接口 [`LiveTranscoding`](https://docs.agora.io/cn/Video/API%20Reference/web/v2.9.0/interfaces/agorartc.livetranscoding.html?transId=2.9.0) 的参数设置进行了如下限制：

- `videoFramerate`：设置转码推流的帧率，单位为 fps。如果设置的帧率超过 30，Agora 服务端会自动调整为 30。
- `videoBitrate`：设置转码推流的码率，单位为 Kbps。你可以根据[视频分辨率表格](https://docs.agora.io/cn/Video/API%20Reference/web/v2.9.0/interfaces/agorartc.videoencoderconfiguration.html#bitrate)中的码率值进行设置。如果设置的码率超出合理范围，Agora 服务端会在合理区间内对码率值进行自适应。
- `videoCodecProfile`：设置转码推流的视频编码规格，可以设置为 66、77 或 100。如果设置其他值，Agora 会统一设为默认值 100。
- `width` 和 `height`：设置转码推流的视频分辨率。**width x height** 的最小值不低于 **16 x 16**。

**新增功能**

#### 支持 NAT64 网络

该版本新增对 NAT64 的支持，在 NAT64 的网络环境下可以正常使用 Agora Web SDK。

#### 选择前置/后置摄像头

`createStream` 方法新增 `facingMode` 参数，支持在移动端创建本地流时选择使用前置或后置摄像头采集视频。

#### 取消事件绑定

新增 `Client.off` 方法，支持移除通过 `Client.on()` 绑定的事件。

**改进**

- 大规模减少了端口的使用，防火墙内的用户不再需要打开 10000-65535 的 UDP 端口，详见 [Web SDK 防火墙端口](https://docs.agora.io/cn/Agora%20Platform/firewall?platform=All%20Platforms#web-sdk)。
- 减少加入频道的时间以及首帧出图时间。
- 网络传输控制优化，提升下行网络拥塞情况下的音视频体验。
- 进一步完善 NAT 类型支持的兼容性，提升了媒体流连接能力。

- 明确了部分 API 的失败回调中的错误码。
- 每次发布流成功时强制同步 mute 的状态。
- 仅在加入频道后才会触发 `volume-indicator` 回调。
- 优化 SDK 参数检查。

**修复问题**

- 修复连接断开重连后，订阅选项（`Client.subscribe` 中的 `options` 参数）会被重置的问题。
- 修复 `Stream.init` 执行过程中调用 `Stream.close` 导致的异常问题。
- 修复 `Stream.startAudioMixing` 中 `cacheResource` 设为 `false` 不生效的问题。
- 修复断开网络连接后部分行为异常的问题。

**API 变更**

- 新增 `Client.off` 方法
- `AgoraRTC.createStream` 方法新增 `facingMode` 参数

## 2.8.0 版
该版本于 2019 年 7 月 8 日发布。

2.8.0 版本主要优化了对 String 类型的用户名的支持。

一个频道内的所有用户必须使用同样类型的用户名，即必须都为整数或都为字符串。使用字符串类型的用户名与 Agora Native SDK 互通时，请确保 Native SDK 也使用字符串类型的用户名，即 User Account 加入频道，详见[使用 String 型的用户名](../../cn/Video/string_web.md)。
## 2.7.1 版

该版本于 2019 年 7 月 3 日发布。

**问题修复**

修复了调用 `Stream.setScreenProfile` 方法设置屏幕共享的视频属性不生效的问题。

## 2.7.0 版
该版本于 2019 年 6 月 21 日发布。新增功能、改进及修复问题详见下文。

**新增功能**

#### 1. 自定义视频编码配置

新增 `Stream.setVideoEncoderConfiguration` 方法，与原有的 `Stream.setVideoProfile` 方法相比，该方法可以自定义视频分辨率、帧率及码率，更加灵活。更多信息请参考[设置视频属性](../../cn/Video/videoProfile_web.md)。

#### 2. 通知音视频流播放状态

为方便开发者管理音视频流播放状态，该版本新增以下功能：

- `Stream.play` 方法中新增 `callback` 参数，用于通知音视频流的播放结果。如果播放失败，该参数会返回具体的失败原因。
- `Stream.on` 中新增 `"player-status-change"` 回调，当音视频流的播放状态发生变化时可以通过该回调了解具体的播放状态以及状态变化的原因。
- 新增 `Stream.resume` 方法，用于在播放失败时恢复播放。

#### 3. 通知远端音视频首帧解码

`Client.on` 中新增 `"first-audio-frame-decode"` 和 `"first-video-frame-decode"` 事件，在订阅远端流成功后完成第一帧远端音频和视频解码时通知 App。

#### 4. 监控媒体设备状态

`Stream.on` 中新增 `"audioTrackEnded"` 和 `"videoTrackEnded"` 回调，当音视频轨道停止时（如设备拔出、取消授权、计算机休眠）会通知 app，方便监控设备状态。

#### 5. 支持 Edge 浏览器

该版本支持在 Edge 浏览器上实现音视频基本互通，具体支持的功能请参考 [Edge 浏览器支持](https://docs.agora.io/cn/faq/browser_support#a-nameedgeaedge)。

**改进**

该版本支持动态修改视频编码配置，即 `Stream.init` 之前或之后都可以调用 `setVideoProfile` 或 `setVideoEncoderConfiguration` 设置视频编码配置。

> 请勿在发布流时修改视频编码配置。

**问题修复**

- 修复 Firefox 上调用 `Stream.getStats` 获取的部分数据异常的问题。
- 修复断网后调用 `Client.leave` 不生效的问题。
- 修复使用 String UID 时获取的部分通话质量数据异常的问题。
- 修复在混音音乐文件播放时调用 `Stream.getAudioMixingPosition` 获取的结果不准确的问题。
- 修复订阅纯音频流后立即调用 `Stream.unmuteVideo` 导致音频无法播放的问题。

**API 整理**

本次发版 API 变动如下。

#### 新增

- [`Stream.setVideoEncoderConfiguration`](https://docs.agora.io/cn/Video/API%20Reference/web/interfaces/agorartc.stream.html#setvideoencoderconfiguration)
- [`Stream.resume`](https://docs.agora.io/cn/Video/API%20Reference/web/interfaces/agorartc.stream.html#resume)
- [`Client.on`](https://docs.agora.io/cn/Video/API%20Reference/web/interfaces/agorartc.client.html#on) 新增以下事件：
  - `"first-audio-frame-decode"`
  - `"first-video-frame-decode"`
- [`Stream.on`](https://docs.agora.io/cn/Video/API%20Reference/web/interfaces/agorartc.stream.html#on) 新增以下事件：
  - `"audioTrackEnded"`
  - `"videoTrackEnded"`
  - `"player-status-change"`

#### 修改

[`Stream.play`](https://docs.agora.io/cn/Video/API%20Reference/web/interfaces/agorartc.stream.html#play) 新增 `callback` 参数。

## 2.6.1 版

该版本于 2019 年 4 月 11 日发布。改进及修复问题详见下文。

**改进**

创建流（`AgoraRTC.createStream`）时支持 `cameraId` 和 `microphoneId` 使用空字符串。

**问题修复**

- 修复在最新版 iOS Safari 上最多只能同时播放两个视频流的问题。
- 修复在发布流（`Client.publish`）之前调用 `enableDualStream` 方法报错的问题。
- 修复使用 String UID 的通话中调用 `getStats` 返回的部分数据缺失的问题。

## 2.6.0 版

该版本于 2019 年 4 月 3 日发布。新增功能及修复问题详见下文。

**新增功能**

#### 1. 音效文件播放和管理

新增一组方法支持在人声中同时播放效果音，支持同时播放多个音效文件，且支持对音效文件的管理，包括调节音量、暂停/停止播放、预加载音效文件等功能，详见[播放音效/音乐混音](../../cn/Video/effect_mixing_web.md)。

#### 2. Chrome 浏览器无插件屏幕共享

Chrome 72 及以上版本无需插件即可使用屏幕共享功能，详见[进行屏幕共享](../../cn/Video/screensharing_web.md)。

#### 3. 其他新增功能

- 支持 PC 端 Firefox ESR 60.1.0 及以上版本
- 支持手动释放混音缓存：在 `Stream.startAudioMixing` 中新增 `cacheResource` 参数用于设置是否缓存混音文件
- 新增 `AgoraRTC.getSupportedCodec` 方法，可检查浏览器支持的编解码格式。
- `Client.on` 新增 `stream-updated` 回调，当远端用户的音视频流进行 `addTrack` 或 `removeTrack` 操作时会触发该回调。
- 混音功能（`startAudioMixing`）支持使用包含中文字符的 URL 作为音乐文件的地址

**改进**

该版本优化了弱网下的音视频流回退体验，同时在 `Client.on` 中新增 `stream-fallback` 回调，在开启音视频流回退（`setStreamFallbackOption`）的情况下，当订阅的流从音视频流回退为音频流或者从音频流恢复为音视频流时可以收到回调通知。


**问题修复**

- 修复在 Chrome 72 上使用 `switchDevice` 无法切换麦克风的问题
- 修复调用 `addTrack` 后 mute 操作不生效的问题
- 修复停止混音后调用 `switchDevice` 然后再次开始混音，远端听不到混音音乐的问题
- 修复使用 String 类型 UID 的通话/直播无法收到 `active-speaker` 回调的问题
- 修复订阅远端流后立即调用 `muteAudio` 或 `muteVideo` 可能不生效的问题
- 修复在 Windows 平台调用 `replaceTrack` 切换视频轨道后调用 `startAudioMixing` 远端无法听到混音音乐的问题

**API 整理**

本次发版 API 变动如下。

#### 新增

- [`AgoraRTC.getSupportedCodec`](https://docs.agora.io/cn/Video/API%20Reference/web/globals.html#getsupportedcodec)
- [`Stream.playEffect`](https://docs.agora.io/cn/Video/API%20Reference/web/interfaces/agorartc.stream.html#playeffect)
- [`Stream.stopEffect`](https://docs.agora.io/cn/Video/API%20Reference/web/interfaces/agorartc.stream.html#stopeffect)
- [`Stream.pauseEffect`](https://docs.agora.io/cn/Video/API%20Reference/web/interfaces/agorartc.stream.html#pauseeffect)
- [`Stream.resumeEffect`](https://docs.agora.io/cn/Video/API%20Reference/web/interfaces/agorartc.stream.html#resumeeffect)
- [`Stream.setVolumeOfEffect`](https://docs.agora.io/cn/Video/API%20Reference/web/interfaces/agorartc.stream.html#setvolumeofeffect)
- [`Stream.preloadEffect`](https://docs.agora.io/cn/Video/API%20Reference/web/interfaces/agorartc.stream.html#preloadeffect)
- [`Stream.unloadEffect`](https://docs.agora.io/cn/Video/API%20Reference/web/interfaces/agorartc.stream.html#unloadeffect)
- [`Stream.getEffectsVolume`](https://docs.agora.io/cn/Video/API%20Reference/web/interfaces/agorartc.stream.html#geteffectsvolume)
- [`Stream.setEffectsVolume`](https://docs.agora.io/cn/Video/API%20Reference/web/interfaces/agorartc.stream.html#seteffectsvolume)
- [`Stream.stopAllEffects`](https://docs.agora.io/cn/Video/API%20Reference/web/interfaces/agorartc.stream.html#stopalleffects)
- [`Stream.pauseAllEffects`](https://docs.agora.io/cn/Video/API%20Reference/web/interfaces/agorartc.stream.html#pausealleffects)
- [`Stream.resumeAllEffects`](https://docs.agora.io/cn/Video/API%20Reference/web/interfaces/agorartc.stream.html#resumealleffects)
- [`Client.on`](https://docs.agora.io/cn/Video/API%20Reference/web/interfaces/agorartc.client.html#on) 新增以下事件：
  - `stream-fallback`
  - `stream-updated`

#### 修改

- [`Stream.startAudioMixing`](https://docs.agora.io/cn/Video/API%20Reference/web/interfaces/agorartc.stream.html#startaudiomixing) 增加 `cacheResource` 参数

## 2.5.2 版

该版本于 2019 年 2 月 28 日发布。修复问题见下文。


**问题修复**

- 修复了在 Chrome 72 及以上调用 `Stream.switchDevice` 无法切换音频的问题。
- 修复了如果未设置 `Client.subscribe` 的可选参数会报错的问题。

## 2.5.1 版

该版本于 2019 年 2 月 11 日发布。新增功能、改进及修复问题详见下文。

**新增功能**

#### 1. 质量透明

支持获取更多通话质量的统计数据：

- 新增 `Client.getSessionStats` 方法，获取当前通话相关的统计数据，包括在频道内的时长，音视频总发送/接收码率，频道内的人数等。
- 新增 `network-quality` 回调，每 2 秒报告一次本地用户的上下行网络质量。
- `Client.getRemoteAudioStats` 获取的远端音频统计数据中新增以下数据：
  - 音频卡顿总时长
  - 音频播放总时长
- `Client.getRemoteVideoStats` 获取的远端视频统计数据中新增以下数据：
  - 视频渲染分辨率宽度
  - 视频渲染分辨率高度
  - 视频播放总时长
  - 视频卡顿总时长
- `Client.getLocalVideoStats` 获取的本地视频统计数据中新增以下数据：
  - 视频采集分辨率宽度
  - 视频采集分辨率高度
  - 视频采集帧率
  - 视频编码卡顿总时长
  - 视频编码总时长
- `Client.getTransportStats` 获取的与网关的连接统计数据中新增以下数据：
  - 上行可用带宽估计
  - 网络类型

#### 2. 独立订阅音频流/视频流

在 `Client.subscribe` 方法中新增 `options` 参数，用于设置是否接收音频数据和视频数据。

该方法可以根据需要在通话中多次调用，在订阅音频和/或视频之间灵活切换。

#### 3. 直播导入在线媒体流

新增 `Client.addInjectStreamUrl` 方法，支持拉取在线音视频流并发送到频道中，将正在播放的音视频导入到正在进行的直播中。可主要应用于赛事直播、多人看视频互动等直播场景。

相应新增 `streamInjectedStatus` 回调，用于通知导入状态的改变。

#### 4. 设置用户角色

新增 `Client.setClientRole` 方法，支持在直播模式下设置用户的角色为主播或者观众。主播可以发布和接收音视频流，观众只能接收音视频流，无法发布。

相应新增 `client-role-changed` 回调，用于通知用户角色的改变。

#### 5. 获取 SDK 与服务器的连接状态

- 新增 `Client.getConnectionState` 方法，用于获取 SDK 与服务器的连接状态。
- 新增 `connection-state-change` 回调，用于通知连接状态的变化。

#### 6. 其他新增功能

- 新增 `Stream.isPlaying` 方法，用于检测音视频流当前是否在播放状态。
- `Client.on` 中新增以下回调：
  - `peer-online` ：通知应用程序有其他用户/主播上线。
  - `stream-reconnect-start` ：通知应用程序 SDk 开始重新发布/订阅音视频流。
  - `stream-reconnect-end` ：通知应用程序重新发布/订阅音视频流已结束。
  - `exception` ：通知应用程序频道内的异常事件。
- `Stream.on` 中新增以下回调：
  - `audioMixingPlayed` ：通知应用程序混音开始播放。
  - `audioMixingFinished` ：通知应用程序混音结束播放。

**改进**

- `Stream.play` 方法中新增 `muted` 参数，用于规避浏览器自动播放策略。
- `Stream.setAudioMixingPosition` 方法中新增 `callback` 参数，可用于返回方法调用失败的错误信息。
- 修改了部分 `Client.on` 回调事件的名称，以保持风格统一。

**问题修复**

- 修复了 macOS 使用 Safari 浏览器互通时，调用 `getStats` 方法返回的对端视频分辨率为 0 的问题。
- 修复了调用 `disableAudio` 方法后开启混音功能，再调用 `enableAudio` 方法，无法听到麦克风声音的问题。
- 修复了掉线时，日志无法正常上传的问题。
- 修复了调用 `switchDevice` 方法切换音频设备后，调用 `getAudioLevel` 方法获取的音量为 0 的问题。
- 修复了调用 `replaceTrack` 方法后，在本地听到自己声音的问题。
- 修复了 iOS 端调用 `switchDevice` 方法无法二次切换的问题。

**API 整理**

本次发版 API 变动如下。

#### 新增

- [`AgoraRTC.getScreenSources`](https://docs.agora.io/cn/Video/API%20Reference/web/globals.html#getscreensources)
- [`Client.addInjectStreamUrl`](https://docs.agora.io/cn/Video/API%20Reference/web/interfaces/agorartc.client.html#addinjectstreamurl)
- [`Client.removeInjectStreamUrl`](https://docs.agora.io/cn/Video/API%20Reference/web/interfaces/agorartc.client.html#removeinjectstreamurl)
- [`Client.getSessionStats`](https://docs.agora.io/cn/Video/API%20Reference/web/interfaces/agorartc.client.html#getsessionstats)
- [`Client.setClientRole`](https://docs.agora.io/cn/Video/API%20Reference/web/interfaces/agorartc.client.html#setclientrole)
- [`Client.getConnectionState`](https://docs.agora.io/cn/Video/API%20Reference/web/interfaces/agorartc.client.html#getconnectionstate)
- [`Stream.isPlaying`](https://docs.agora.io/cn/Video/API%20Reference/web/interfaces/agorartc.stream.html#isplaying)
- [`Stream.muteAudio`](https://docs.agora.io/cn/Video/API%20Reference/web/interfaces/agorartc.stream.html#muteAudio)
- [`Stream.unmuteAudio`](https://docs.agora.io/cn/Video/API%20Reference/web/interfaces/agorartc.stream.html#unmuteAudio)
- [`Stream.muteVideo`](https://docs.agora.io/cn/Video/API%20Reference/web/interfaces/agorartc.stream.html#muteVideo)
- [`Stream.unmuteVideo`](https://docs.agora.io/cn/Video/API%20Reference/web/interfaces/agorartc.stream.html#unmuteVideo)
- [`Client.on`](https://docs.agora.io/cn/Video/API%20Reference/web/interfaces/agorartc.client.html#on) 新增以下事件：
  - `streamInjectedStatus`
  - `client-role-changed`
  - `peer-online`
  - `network-quality`
  - `connection-state-change`
  - `stream-reconnect-start`
  - `stream-reconnect-end`
  - `exception`
- [`Stream.on`](https://docs.agora.io/cn/Video/API%20Reference/web/interfaces/agorartc.stream.html#on) 新增以下事件：
  - `audioMixingPlayed`
  - `audioMixingFinished`

#### 修改

- [`Client.subscribe`](https://docs.agora.io/cn/Video/API%20Reference/web/interfaces/agorartc.client.html#subscribe) 方法增加 `options` 参数
- [`Stream.setAudioMixingPosition`](https://docs.agora.io/cn/Video/API%20Reference/web/interfaces/agorartc.stream.html#setaudiomixingposition) 方法增加 `callback` 参数
- [`Stream.play`](https://docs.agora.io/cn/Video/API%20Reference/web/interfaces/agorartc.stream.html#play) 方法增加 `muted` 参数
- [`Client.on`](https://docs.agora.io/cn/Video/API%20Reference/web/interfaces/agorartc.client.html?#on) 以下事件名称修改：

| 原事件名               | 现事件名                 |
| ---------------------- | ------------------------ |
| networkTypeChanged     | network-type-changed     |
| recordingDeviceChanged | recording-device-changed |
| playoutDeviceChanged   | playout-device-changed   |
| cameraChanged          | camera-changed           |
| streamTypeChange       | stream-type-changed      |

#### 废弃

- `Client.getNetworkStats`
- `Stream.enableAudio`
- `Stream.disableAudio`
- `Stream.enableVideo`
- `Stream.disableVideo`

## 2.5.0 版

该版本于 2018 年 10 月 30 日发布。新增功能与修复问题列表详见下文。

> <font color="red">如果设置了域名防火墙，使用此版本时需将 `*.agoraio.cn` 和 `*.agora.io` 添加到白名单。</font>

**新增功能**

为更好地与 Agora 其他 SDK 互通，实现更多功能，Web SDK 在本版本中新增了如下功能。详细的方法说明，请参考 [Agora Web SDK API Reference](https://docs.agora.io/cn/Interactive%20Broadcast/API%20Reference/web/index.html)。

#### 1. 质量监控

为方便用户查看应用程序的通话质量，新增如下方法：

- `Client.getNetworkStats` ：获取网络统计数据（网络类型）。
- `Client.getSystemStats` ：获取系统数据（系统电量）。
- `Client.getRemoteAudioStats` ：获取远端音频统计数据。
- `Client.getLocalAudioStats` ：获取本地音频统计数据。
- `Client.getRemoteVideoStats` ：获取远端视频统计数据。
- `Client.getLocalVideoStats` ：获取本地视频统计数据。
- `Client.getTransportStats` ：获取网络连接统计数据。

#### 2. 媒体设备管理

提供灵活的设备管理功能，以及设备状态查询。

- **枚举可用设备**

  新增如下方法：

  - `Client.getRecordingDevices`：枚举音频输入设备，如麦克风。
  - `Client.getPlayoutDevices` ：枚举音频输出设备，如扬声器。
  - `Client.getCameras` ：枚举视频输入设备，如摄像头。

  同时新增如下事件，用来告知应用程序设备状态的变化：

  - `recordingDeviceChanged` ：通知应用程序音频输入设备已改变。
  - `playoutDeviceChanged` ：通知应用程序音频输出设备已改变。
  - `cameraChanged` ：通知应用程序视频输入设备已改变。

- **切换媒体设备**

  - 新增 `Stream.switchDevice` 方法，支持在频道内切换媒体输入设备，如麦克风、摄像头等。
  - 新增 `Stream.setAudioOutput` 方法，支持选择音频输出设备，可以切换麦克风和扬声器。

#### 3. 支持伴奏混音

支持混音功能，混音是指原音（麦克风采集的音频）和伴奏（音频文件声音）混合。

新增如下伴奏混音相关的方法：

- `Stream.startAudioMixing` ：开始播放伴奏。
- `Stream.stopAudioMixing` ：停止播放伴奏。
- `Stream.pauseAudioMixing` ：暂停播放伴奏。
- `Stream.resumeAudioMixing` ：恢复播放伴奏。
- `Stream.adjustAudioMixingVolume` ：调节伴奏音量。
- `Stream.getAudioMixingDuration` ：获取伴奏时长。
- `Stream.getAudioMixingCurrentPosition` ：获取伴奏播放进度。
- `Stream.setAudioMixingPosition` ：设置伴奏音频文件的播放位置。

#### 4. 音视频轨道管理

支持灵活管理音视频频道，新增如下方法：

- `Stream.getAudioTrack` ：获取音频轨道。
- `Stream.getVideoTrack` ：获取视频轨道。
- `Stream.replaceTrack` ：替换音视频轨道。
- `Stream.addTrack` ：添加音视频轨道。
- `Stream.removeTrack` ：移除音视频轨道。

#### 5. 其他新增功能

- 支持两种视频显示模式，可以在 `Stream.play` 方法中设置播放流的显示模式。
- 新增 `Client.enableAudioVolumeIndicator` 方法，允许 SDK 定期向应用程序反馈当前谁在说话，以及说话者的音量。
- 新增 `Stream.setAudioVolume`  方法，支持设置订阅流的音量。
- 新增 `networkTypeChanged` 事件，通知应用程序网络类型已改变。
- 新增 `streamTypeChange` 事件，通知应用程序视频流类型已由大流变为小流，或小流变为大流。
- `Client.join` 方法中，在原来支持整型 `uid` 的基础上，新增对字符串类型的支持。
- 支持 360 安全浏览器 9.1.0.432 及以上版本。
- 支持 Windows XP 平台的 Chrome 49 浏览器。

**问题修复** 

- 修复了手机端使用 Safari 或 Chrome 浏览器进入频道后，在仅有音频通话的情况下对 video codec 的依赖。
- 修复了使用 Safari 浏览器推流后调用 `Stream.close` 关闭流，对端 10 秒后无法收到 `stream-removed` 回调的问题。
- 修复了重置 `Stream.userId` 后，收到 Warning 的问题。

## 2.4.1 版

该版本于 2018 年 9 月 19 日发布。修复问题列表详见下文。

**问题修复**

- 修复了本地 IP 地址变化后无法自动 publish ，导致对端接收不到本地视频流的问题。
- 修复了取消订阅远端流会触发 `stream-removed` 事件的问题。

## 2.4.0 版

该版本于 2018 年 8 月 24 日发布。新增特性与修复问题列表详见下文。

**新增功能**

本次发版新增如下功能：

#### 1. 支持 Token

详见 [密钥说明](../../cn/Agora%20Platform/token.md)。

#### 2. 支持音频信号处理

在 `audioProcessing` 参数中新增声学回声消除属性（AEC）和自动噪声抑制属性（ANS）。

#### 3. 支持音视频前处理

在 `client.createStream` 方法中增加了 `audioSource` 和 `videoSource` 属性，可以指定创建的音视频流要使用的音视频 Track，因此在创建音视频流之前可以对音视频进行处理。

#### 4. 支持设置音频属性

新增 `stream.setAudioProfile` 方法，提供多组音频属性设置（采样率、单双声道、编码码率）。

#### 5. 支持弱网时音视频流回退

在网络环境不理想的情况下，为保证通话体验，新增 `client.setStreamFallbackOption` 接口支持在接收端选择订阅视频小流或者只订阅音频流。

#### 6. 新增通话相关延迟数据

在 `stream.getStats` 的回调信息中新增通话中的延迟数据，包括从发送端到 SD-RTN 的延迟、SD-RTN 到接收端的延迟、发送端到接收端的延迟，以及接收端播放音视频的延迟。

#### 7. 支持日志上传

新增 `AgoraRTC.Logger.enableLogUpload` 接口，支持将 SDK 的日志信息上传到 Agora 的服务器，方便调查问题。

**问题修复**

- 修复了不调用 `stream.setVideoProfile` 接口, `cameraId` 和 `microphoneId` 设置就不生效的问题。
- 修复在 Firefox 浏览器上调用 `setScreenProfile` 设置不生效的问题。
- 修复在 `client.createStream` 方法中同时开启 `audio` 和 `screen` 会出错的问题。
- 修复屏幕共享时设置 `microphoneId` 不生效的问题。
- 修复 `audioProcessing` 设置不生效的问题。
- 修复了 Stream 能够 play 多次的问题。修复后 Stream 对象只能 play 一次，在调用 `stream.stop` 后才可以再次调用 `stream.play`。

## **2.3.1 版**

该版本于 2018 年 6 月 7 日发布。新增特性与修复问题列表详见下文。

**问题修复**

- 修复浏览器安装了 Adblock Plus 插件之后偶发的推流失败的问题。
- 修复了多 IP 跳转不生效的问题。

## **2.3.0 版**

该版本于 2018 年 6 月 4 日发布。新增特性与修复问题列表详见下文。

**新增功能**

本次发版新增如下功能：

#### 1. 新通信模式

为增加 Web SDK 的适用场景，提升与 Native SDK 在通信和直播下的互通质量，在 `createClient` 方法中新增 `mode` 和 `codec` 参数，其中 `mode` 参数支持 rtc 和 live 两种场景，`codec` 参数支持 vp8 和 264 两种编解码方式。 

#### 2. 支持语音自动增益控制

为满足通话或直播中对语音响度进行控制和调整的需要，在 `createStream` 方法中新增 `audioProcessing` 参数。

#### 3. 网页端 Proxy 功能

为解决设有防火墙的企业用户无法直接使用 Agora 服务的问题，新增 2 个接口，通过代理服务器将用户的内网请求转到 Agora SD-RTN 上，从而实现内网访问。使用该功能，用户需要自行部署 Nginx 和 TURN 服务器， 并在加入频道之前就调用相关接口。详见 [企业部署代理服务器](../../cn/Video/proxy_web.md) 中关于代理服务器工作原理、部署步骤以及调用 API 接口的描述。

#### 4. 加密功能

为提高通话或直播安全性，新增加密功能。用户只要在加入频道之前调用接口设置加密密码及加密方案即可。

**问题修复**

已修复：出现 p2plost 后重连一次或几次后不再重连。

## **2.2 版**

该版本于 2018 年 4 月 16 日发布。新增特性与修复问题列表详见下文。

**新增功能**

本次发版新增如下功能：

#### 1. 获取版本信息

支持获取当前使用的 SDK 版本信息。

#### 2. 设置小流参数

新增设置小流参数接口，允许对小流参数进行配置。

#### 3. Firefox 浏览器屏幕共享

新增 Firefox 浏览器屏幕共享功能，通过在` createStream` 方法中增加`mediaSource` 参数实现。详见 [Firefox 屏幕共享](../../cn/Video/screensharing_web.md)。

#### 4. QQ 浏览器支持

新增 Android 设备对 QQ 浏览器的支持。

**问题修复**

解决了 iOS 端使用 Safari 浏览器时，audio-only 模式下没有声音的问题。

## **2.1.1 版**

该版本于 2018 年 3 月 19 日发布。

修复了 macOS 上 Firefox v59.01 浏览器使用 Web SDK 只能看到本地视频看不到对方视频的问题。

## **2.1 版**

该版本于 2018 年 3 月 7 日发布。新增特性与修复问题列表详见下文。

**新增功能**

本次发版新增如下功能：

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>功能</td>
<td>描述</td>
</tr>
<tr><td>质量检测</td>
<td>用户在使用 Web SDK 进行远程通信或直播前，提供回调对当前的浏览器的兼容情况、麦克风及摄像头的使用情况、网络状况进行测试，以保证通话质量;</td>
</tr>
<tr><td>视频大小流</td>
<td>新增方便发送端控制发送大流或小流，以及接收端控制请求大流或小流，并允许对小流进行参数配置;</td>
</tr>
<tr><td>客户端踢人</td>
<td>提供回调通知被踢客户其已被禁止加入频道</td>
</tr>
<tr><td>RTMP 推流(直播)</td>
<td>新增接口支持 RTMP 推流</td>
</tr>
<tr><td>设置日志输出等级</td>
<td>新增接口对日志的输出等级进行设置</td>
</tr>
<tr><td>静音/取消静音</td>
<td>新增接口在通话或直播过程中静音或取消静音</td>
</tr>
</tbody>
</table>



**改进**

本次发版改进如下功能：

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td>功能</td>
<td>描述</td>
</tr>
<tr><td>P2P 连接建立时间优化</td>
<td>从 1.8 秒缩短到 500 毫秒</td>
</tr>
<tr><td>优化丢包对抗</td>
<td>优化 FEC 对抗 20% 丢包和 ULP FEC 扛丢包</td>
</tr>
</tbody>
</table>



**问题修复**

- 修复了 Safari 上视频旋转 90 度的问题
- 修复了 macOS 上 Firefox v59.01 浏览器使用 Web SDK 只能看到本地视频看不到对方视频的问题

## **2.0 版及之前**

### **2.0 版**

该版本于 2017 年 12 月 6 日发布。新增特性与修复问题列表详见下文。

**新增功能**

- 新增了支持 Safari 浏览器。
- 新增了支持 Firefox 浏览器。
- 新增了检查浏览器兼容性功能，在 `createClient`之前新增了 `checkSystemRequirements` ，以检查系统对浏览器是否支持。
- 新增了本地摄像头 / 麦克风权限已获取 microphone/camera permission granted 回调，通知应用程序已获取本地摄像头／麦克风使用权限。
- 新增视频自适应功能，如果客户使用的摄像头不满足配置要求，会自动适配到一个合理的配置要求，以使摄像头设备满足帧率和分辨率要求。

**问题修复**

- 修复了 iOS SDK 与 Web SDK 互通音量小的问题。
- 修复了使用 Chrome 浏览器长时间互通后掉线的问题。
- 修复了 Android 设备切换前后置摄像头时屏幕显示异常的问题。
- 修复了 Android 设备互通时，一方离开后，另一方屏幕显示异常的问题。
- 修复了 Chrome 浏览器未启用摄像头进入频道时，不提示需获取摄像头权限的问题。
- 修复了 iOS 端使用 Safari 浏览器互通时，退到后台后不显示对方画面的问题。

### **1.14 版**

该版本于 2017 年 10 月 20 日发布。

新增了屏幕共享功能，在 `createStream` 里修改了参数 `screen`, 并新增了参数 `extensionId` 。

### **1.13 版**

该版本于 2017 年 9 月 4 日发布。新增特性与修复问题列表详见下文。

**新增功能**

- 支持 Web 端 CDN 推流，进行旁路支持，详见 `API configPublisher` 描述
- 在 Android 上支持最新版 Chrome

### **1.12 版**

该版本于 2017 年 7 月 25 日发布。。新增特性与修复问题列表详见下文。

新增和更新了以下 API:

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>API</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>createClient</code></td>
<td>[新增] 创建纯 Web 端或 Web 互通版客户端对象，取决于你设置了哪种模式</td>
</tr>
<tr><td><code>renewChannelKey</code></td>
<td>[新增] 之前 Channel Key 过期后，需调用该方法更新 Channel Key</td>
</tr>
<tr><td><code>active-speaker<code></td>
<td>[新增] 提示当前频道内谁正在说话</td>
</tr>
<tr><td><code>setRemoteVideoStreamType</code></td>
<td>[新增] 当发送端发送了双流(视频大流和小流)时，本地用户调用该方法可以选择接收双流中的一路流</td>
</tr>
<tr><td><code>setVideoProfile</code></td>
<td>[更新] 设置视频属性，默认值为 480p_1</td>
</tr>
<tr><td><code>init</code></td>
<td>[更新] 初始化客户端对象</td>
</tr>
<tr><td><code>join</code></td>
<td>[更新] 允许用户加入 Agora 频道</td>
</tr>
</tbody>
</table>



更新了错误代码和解释。
