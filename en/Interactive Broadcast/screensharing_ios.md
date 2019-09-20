
---
title: Share the Screen
description: 
platform: iOS
updatedAt: Thu Sep 19 2019 08:59:36 GMT+0800 (CST)
---
# Share the Screen
## Introduction
During a video call or live broadcast, **sharing the screen** enhances communication by displaying the speaker's screen on the display of other speakers or audience members in the channel.

Screen sharing is applied in the following scenarios:

- In a video conference, the speaker can share an image of a local file, web page, or presentation with other users in the channel.
- In an online class, the teacher can share the slides or notes with students.

## Implementation

Ensure that you implement a video call or an interactive broadcast in your project. For details, see [Start a Call](../../en/Interactive%20Broadcast/start_call_ios.md) or [Start an Interactive Broadcast](../../en/Interactive%20Broadcast/start_live_ios.md).

Screen sharing on iOS is implemented with the following steps:

- Create a process with Broadcast Upload Extension.
- Record the screen with Apple ReplayKit.
- Use the Agora SDK to transfer the video stream.

```swift
// swift
override func processSampleBuffer(_ sampleBuffer: CMSampleBuffer, with sampleBufferType: RPSampleBufferType) {
        DispatchQueue.main.async {
            switch sampleBufferType {
            case RPSampleBufferType.video:
                AgoraUploader.sendVideoBuffer(sampleBuffer)
                break
            case RPSampleBufferType.audioApp:
                AgoraUploader.sendAudioAppBuffer(sampleBuffer)
                break
            case RPSampleBufferType.audioMic:
                AgoraUploader.sendAudioMicBuffer(sampleBuffer)
                break
            }
        }
    }
```

```objective-c
// objective-c
- (void)processSampleBuffer:(CMSampleBufferRef)sampleBuffer withType:(RPSampleBufferType)sampleBufferType {
    dispatch_async(dispatch_get_main_queue(), ^{
    	switch (sampleBufferType) {
        case RPSampleBufferTypeVideo:
            [agoraUpload sendVideoBuffer:sampleBuffer];
             NSLog(@"RPSampleBufferTypeVideo App~~~~");
            break;
        case RPSampleBufferTypeAudioApp:
            [agoraUpload sendAudioAppBuffer:sampleBuffer];
            NSLog(@"RPSampleBufferTypeAudio App+++");
            break;
        case RPSampleBufferTypeAudioMic:
            [agoraUpload sendMicAppBuffer:sampleBuffer];
            break;
    	}
    })
}
```

We also provide an open-source demo project that implements [Agora Screen Sharing](https://github.com/AgoraIO/Advanced-Video/tree/master/Screensharing/Agora-Screen-Sharing-iOS) on Github. You can try the demo and view the source code.
