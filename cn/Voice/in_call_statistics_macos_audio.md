
---
title: 检测通话质量
description: v1
platform: macOS
updatedAt: Mon Jul 01 2019 07:41:56 GMT+0800 (CST)
---
# 检测通话质量
通话质量检测功能是在 SDK 加入频道后通过每 2 秒触发一次的回调实现。

通话质量检测包括：

- 用户上下行网络质量打分
- 远端用户音频质量统计
	- 端到端质量统计
	- 网络传输层质量统计

## 用户上下行网络质量打分

![](https://web-cdn.agora.io/docs-files/1546918095508)

### 功能描述

加入频道后，如果想检测通话中每个用户的网络上下行 last mile 质量报告，则可以通过回调 `networkQuality` 得到。

`networkQuality` 携带 `uid` 信息，如果频道内有多个用户，SDK 会多次触发该回调。打分包括：

- `txQuality`：基于当前设备到边缘服务器的上行网络质量打分（EXCELLENT～VBAD）<sup>[1]</sup>。打分依赖项：
  - 上行实际发送码率与目标发送码率的比率，比率越高，通话质量越高；
  - 上行丢包率；
  - 平均往返延时（RTT）；
  - 上行网络抖动（Jitter）。

- `rxQuality`：基于边缘服务器到当前设备的下行网络质量打分（EXCELLENT～VBAD）<sup>[1]</sup>。打分依赖项：
  - 下行丢包率；
  - 平均往返延时（RTT）；
  - 下行网络抖动（Jitter）。

<a name ="table"></a>
> [1] 质量打分对照表如下：
>
> | 质量打分  | 说明                                                         |
> | -------- | :----------------------------------------------------------- |
> | 0        | UNKNOWN：网络质量未知。                                        |
> | 1        | EXCELLENT：网络质量极好。                                      |
> | 2        | GOOD：用户主观感觉和 EXCELLENT 差不多，但码率可能略低于 EXCELLENT。 |
> | 3        | POOR：用户主观感受有瑕疵但不影响沟通。                       |
> | 4        | BAD：勉强能沟通但不顺畅。                                    |
> | 5        | VBAD：网络质量非常差，基本不能沟通。                         |
> | 6        | DOWN：网络连接断开，完全无法沟通。                         |

### API 参考

[`networkQuality`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:networkQuality:txQuality:rxQuality:)

```objective-c
- (void)rtcEngine:(AgoraRtcEngineKit *_Nonnull)engine networkQuality:(NSUInteger)uid txQuality:(AgoraNetworkQuality)txQuality rxQuality:(AgoraNetworkQuality)rxQuality
```

### 注意事项

用户上下行网络质量打分 `networkQuality` 不同于通话前的网络质量检测 `lastmileQuality`：
- `lastmileQuality` 在用户加入频道前通过主动调用 `enableLastmileTest` 方法后才会触发；`networkQuality` 在用户加入频道后自动触发。
- `lastmileQuality` 反映的是本地用户设备到边缘服务器的网络上下行质量；`networkQuality` 反映的是所有用户的设备到边缘服务器的网络上下行质量（如果频道内有多个用户，该回调会相应多次触发）。

## 远端用户音频质量统计

![](https://web-cdn.agora.io/docs-files/1546918106760)

### 功能描述

如上图所示，本功能反映远端音频流全链路的音频质量以及传输层网络状态。涉及回调：

- `remoteAudioStats` 2 秒自动回调：侧重反映通话中远端音频流的全链路音频质量，更贴近用户主观感受。主要返回信息：
  - `quality`：音频接收质量打分（Excellent～VeryBad）<sup>[2]</sup>。
  - `networkTransportDelay`：网络传输层延时（毫秒），networkTransportDelay = Delay 2 + Delay 3 + Delay 4。
  - `jitterBufferDelay`：接收端网络抖动延时（毫秒），jitterBufferDelay = Delay 5。
  - `audioLossRate`：音频丢帧率。

- `audioTransportStatsOfUid` 2 秒自动回调：侧重反映通话中远端音频流的传输层网络状态，数据更为客观。主要返回信息：
  - `delay`：网络传输层延时（毫秒），delay = Delay 2 + Delay 3 + Delay 4。
  - `lost`：传输层音频丢包率（%），lost = (packetLoss 2 + packetLoss 3 + packetLoss 4)/totalPacketsSent。
  - `rxKBitRate`：音频接收码率（Kbps）。

> [2] 参见上文[质量打分对照表](#table)。

### API 参考

- [`remoteAudioStats`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:remoteAudioStats:)

```objective-c
- (void)rtcEngine:(AgoraRtcEngineKit *_Nonnull)engine remoteAudioStats:(AgoraRtcRemoteAudioStats *_Nonnull)stats
```

- [`audioTransportStatsOfUid`](https://docs.agora.io/cn/Voice/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:audioTransportStatsOfUid:delay:lost:rxKBitRate:)

```objective-c
- (void)rtcEngine:(AgoraRtcEngineKit *_Nonnull)engine audioTransportStatsOfUid:(NSUInteger)uid delay:(NSUInteger)delay lost:(NSUInteger)lost rxKBitRate:(NSUInteger)rxKBitRate
```

### 注意事项

`audioTransportStatsOfUid` 与 `remoteAudioStats` 回调的区别在于：

- `audioTransportStatsOfUid` 用于描述用户通话中从边缘服务器到边缘服务器的网络状态，通过音频包计算，展示当前网络状态，体现真实网络客观数据，比如丢包、网络延迟。
- `remoteAudioStats` 反映的是全链路的音频质量，更贴近用户的主观感受。比如，当网络发生丢包时，由于 FEC（Forward Error Correction，向前纠错码）或重传恢复的应用，最终的音频丢帧率不高，则可以认为整个质量较好。 


