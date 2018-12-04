
---
title: In-ear Monitoring
description: How to enable in-ear monitoring and adjust the volume
platform: iOS
updatedAt: Tue Dec 04 2018 19:02:47 GMT+0000 (UTC)
---
# In-ear Monitoring
In-ear monitoring provides a mix of the audio sources (for example, a mix of the vocals and music) to the host with low latency, commonly used in professional scenarios, such as in concerts.
The Agora SDK supports the in-ear monitoring function and volume adjustment of the in-ear monitor.

## Implementation
Before proceeding, ensure that you have prepared the development environment. See [Integrate the SDK](../../en/Interactive%20Broadcast/ios_video.md) for more information.

```swift
// swift
// Enables in-ear monitoring. The default value is false.
agoraKit.enable(inEarMonitoring: true)

// Sets the volume of the in-ear monitor. The value ranges between 0 and 100. The default value is 100, which represents the original volume captured by the microphone.
agoraKit.setInEarMonitoringVolume(50)
```

```objective-c
// objective-c
// Enables in-ear monitoring. The default value is NO.
[agoraKit enableInEarMonitoring:YES];

// Sets the volume of the in-ear monitor. The value ranges between 0 and 100. The default value is 100, which represents the original volume captured by the microphone.
[agoraKit setInEarMonitoringVolume: 50];
```

## Considerations

The API methods have return values. If the method fails, the return value is < 0.
