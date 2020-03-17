
---
title: Inject Online Media Stream
description: 
platform: iOS,macOS
updatedAt: Wed Dec 11 2019 10:31:52 GMT+0800 (CST)
---
# Inject Online Media Stream
## Introduction

**Injecting an online media stream** is the action of adding an external audio or video stream to an ongoing live-broadcast channel. It enables the hosts and audience in the channel to hear and see the additional stream while interacting with each other.

### Applicable scenarios

- Live sports: The host and audience can watch and simultaneously commenting on events.
- Music concerts, movies, and other entertainments: The hosts and audience can participate in real-time discussions while watching them.
- Additional perspectives: The host can inject video streams captured by drones or network cameras into a live broadcast.

### Working principles

The host in a live-broadcast channel pulls an online media stream and pushes it through the Video Inject Server to the Agora Software-Defined Real-time Network (SD-RTN™) and the channel.

![](https://web-cdn.agora.io/docs-files/1576059890625)

- The host and audience in the channel can hear/see the media stream.
- If the host enables Content Delivery Network (CDN) live streaming, the injected media stream is also pushed to the CDN so that the CDN audience can hear/also the media stream.

>- Only one online media stream can be injected into the same channel at the same time.
>- Supported codec type: AAC for audio, H.264 for video.
>- Audio-only streams are also supported.
>- Only the host (broadcaster) can inject and remove an injected media stream. Neither the delegated host nor the audience can do that.


## Implementation

Before proceeding, ensure that you implement a basic live broadcast in your project. See [Start a Live Broadcast](../../en/Audio%20Broadcast/start_live_ios.md) for details.

> Ensure that you enable the RTMP Converter service before using this function. See [Prerequisites](../../en/Audio%20Broadcast/cdn_streaming_apple.md).

Refer to the following steps to inject an online media stream:

1. The host in a channel calls the `addInjectStreamUrl` method to inject an online media stream to the live broadcast channel. You can modify the parameter values of `config` to set the resolution, bitrate and frame rate of the injected stream. See [`AgoraInjectStreamConfig`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraLiveInjectStreamConfig.html).
	> Only one online media stream can be injected into the same channel at the same time.

	If the method call is successful, SDK triggers the `didJoinedOfUid (uid:666)` callback to all the users in the channel, and triggers the `streamInjectedStatusOfUrl` callback to the local host.
	> The local host can troubleshoot with [API Reference](#api) when exceptions occur.
	
2. The host in a channel calls the `removeInjectStreamUrl` method to remove the injected media stream.
	If the method call is successful, SDK triggers the `didOfflineOfUid (uid:666)` callback to all the users in the channel.
	> Do not need to call the `removeInjectStreamUrl` method if the host has left the channel.


### Sample code


```swift
// Swift
// Inject an online media stream.
let config = AgoraLiveInjectStreamConfig()
  config.size = CGSize(width: 640, height: 360)
  config.videoGop = 30
  config.videoBitrate = 400
  config.videoFramerate = 15
  config.audioSampleRate = 48000
  config.audioBitrate = 48
  config.audioChannels = 1

  agoraKit.addInjectStreamUrl("media stream url", config: config)

// Remove an online media stream.
let urlPath = "Some online RTMP/HLS url path"
  agoraKit.removeInjectStreamUrl(urlPath)
```

```objective-c
// Objective-C
// Inject an online media stream.
AgoraLiveInjectStreamConfig *config = [[AgoraLiveInjectStreamConfig alloc] init];
  config.size = CGSizeMake(640, 360);
  config.videoGop = 30
  config.videoBitrate = 400
  config.videoFramerate = 15
  config.audioSampleRate = 48000
  config.audioBitrate = 48
  config.audioChannels = 1

  [agoraKit addInjectStreamUrl: @"media stream url" config: config];

// Remove an online media stream.
NSString *urlPath = @"Some online  RTMP/HLS url path";
  [agoraKit removeInjectStreamUrl: urlPath];
```

We also provide an open-source [Live-Streaming-Injection](https://github.com/AgoraIO/Advanced-Interactive-Broadcasting/tree/master/Live-Streaming-Injection) demo project on GitHub.

<a name="api"></a>
### API reference

- [`addInjectStreamUrl`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/addInjectStreamUrl:config:)
- [`removeInjectStreamUrl`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/removeInjectStreamUrl:)
- [`streamInjectedStatusOfUrl`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:streamInjectedStatusOfUrl:uid:status:)
- [`didJoinedOfUid`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didJoinedOfUid:elapsed:)
- [`didOfflineOfUid`](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/oc/Protocols/AgoraRtcEngineDelegate.html#//api/name/rtcEngine:didOfflineOfUid:reason:)

## Considerations
To receive the injected media stream, the audience need to subscribe to the host.
