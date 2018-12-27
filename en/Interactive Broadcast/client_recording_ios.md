
---
title: Record the Audio from the Client
description: 
platform: iOS,macOS
updatedAt: Thu Dec 27 2018 02:59:44 GMT+0000 (UTC)
---
# Record the Audio from the Client
## Introduction

You can record the audio of all users in a call and save it on the client, just like using the recording function on your cell phone to record a call and save it for future replays. 

Agora's Native SDK supports audio recording at the client. You can record the audio of all users in a channel and generate one recording file with the following format: 

- wav: Large file (lossless compression)
- aac: Smaller file (lossy compression)

## Implementation

```swift
// Swift
// Start audio recording.
// Local path of the recording file specified by the user, including the filename and format.
// Audio quality of the recording: LOW, MEDIUM, and HIGH.
agoraKit.startAudioRecording("recording file path", quality: .high)

// Stop audio recording.
agoraKit.stopAudioRecording()
```

```oc
// Objective-C
// Start audio recording.
// Local path to the recording file specified by the user, including the filename and format.
// Audio quality of the recording: LOW, MEDIUM, and HIGH.
[agoraKit startAudioRecording:@"recording file path", quality: AgoraAudioRecordingQualityHigh];

// Stop audio recording.
[agoraKit stopAudioRecording];
```

### API References

- [`startAudioRecording:quality:`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/startAudioRecording:quality:)
- [`stopAudioRecording`](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/stopAudioRecording)

## Considerations

- Only after joining a channel can you start recording the audio.
- Client audio recording is automatically stopped once you leave the channel. 
