
---
title: Video for 7+ Users
description: 
platform: Android
updatedAt: Tue Dec 10 2019 04:20:42 GMT+0800 (CST)
---
# Video for 7+ Users
## Introduction
A video conference with too many hosts may cause network latency and packet loss.

If we set the subscribing stream to the 1-N Mode (set one stream as high and the rest as low), then a maximum of 17 users can join as hosts in an interactive broadcast without any network latency.

>  A maximum of 17 users is supported on the PC platform, while a maximum of 7 users is supported on the mobile platform.

## Implementation

Before proceeding, ensure that you implement a basic live broadcast in your project. See [Start a Live Broadcast](../../en/Interactive%20Broadcast/start_live_android.md) for details.

Refer to the following steps to enable the video conference of 7+ users:

1. Before joining a channel, call  `setParameters("{"che.audio.live_for_comm":true}")` to enable multi-party live broadcast mode.

> If the version of SDK is v2.3.1 or earlier, you need to call `setParameters("{\"che.video.moreFecSchemeEnable\":true}”)` to enable ULP FEC and improve the reliability of data transmission.  

2. After joining a channel, the hosts call the `enableDualStreamMode` method to enable the [video dual stream mode](https://docs.agora.io/en/Agora%20Platform/terms?platform=All%20Platforms#a-name-dualadual-stream-mode).
	> After calling the method successfully, the SDK automatically sets the parameters of the low-video stream according to the parameters of the high-video stream.
	
3. The hosts call the `setRemoteVideoStreamType` method to set one subscribed video stream as the high-video stream and other susbcribed streams as the low-video streams.
	> Agora does not recommend using video profiles that exceed either a resolution of 640 x 480 or a frame rate of 30 fps.
	> <table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>Resolution</strong></td>
<td><strong>Frame Rate</strong></td>
<td><strong>Bitrate</strong></td>
</tr>
<tr><td>640 x 480</td>
<td>15 fps</td>
<td>500 Kbps</td>
</tr>
<tr><td>640 x 360</td>
<td>15 fps</td>
<td>400 Kbps</td>
</tr>
<tr><td>640 x 360</td>
<td>24 fps</td>
<td>800 Kbps</td>
</tr>
</tbody>
</table>

4. (Optional) Customize the default low-video stream parameters at the application level.
```java
   // Sets the video profile to 320 x 180, 15 fps, and 140 Kbps.
   setParameters("{\"che.video.lowBitRateStreamParameter\":{\"width\":320,\"height\":180,\"frameRate\":15,\"bitRate\":140}}");
   ```
> The aspect ratio of the low-stream video profile (width x height) should be identical to that of the preset video profile. 


### Sample code

We also provide an open-source [Large-Group-Video-Chat](https://github.com/AgoraIO/Advanced-Video/tree/master/Large-Group-Video-Chat) demo project on GitHub.

### API reference

- [`enableDualStreamMode`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a645cb7d0f3a59dda27b157cf130c8c9a)
- [`setRemoteVideoStreamType`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a51756b4d2e7997fbe6481d2deb5c0396)
- [`setParameters`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#ab07f44c88e788ce59ba1858b522dc4ad)

## Considerations
- Agora recommends using a layout with one big window and multiple small windows: Use a high-stream layout for the big window; Use a low-stream layout for small windows.
- When a user is offline and receiving the callback indicating that a `uid` if offline, call the `setupRemoteVideo` method and set the `view` as NULL to release all memory resources of the view used by that user. 
