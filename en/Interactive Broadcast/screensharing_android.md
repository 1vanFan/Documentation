
---
title: Share the Screen
description: 
platform: Android
updatedAt: Thu Mar 12 2020 23:52:49 GMT+0800 (CST)
---
# Share the Screen
## Introduction
During a video call or live broadcast, **sharing the screen** enhances communication by displaying the speaker's screen on the display of other speakers or audience members in the channel.

Screen sharing is applied in the following scenarios:

- In a video conference, the speaker can share an image of a local file, web page, or presentation with other users in the channel.
- In an online class, the teacher can share the slides or notes with students.

## Implementation

Before proceeding, ensure that you implement a basic call or live broadcast in your project. See [Start a Call](../../en/Interactive%20Broadcast/start_call_android.md) or [Start a Live Broadcast](../../en/Interactive%20Broadcast/start_live_android.md) for details.

The Agora SDK does not provide specific methods for screen sharing on Android. With system methods provided by Android, you can enable screen sharing with the following steps:
- Use MediaProjection/VirtualDisplay to get the screen image data.
- Maintain an OpenGL environment, create a SurfaceView and pass the View to VirtualDisplay as the recipient of the screen image data.
- Take the image data retrieved from the SurfaceView callback as the external video source. Call the `pushExternalVideoFrame` method to transfer the image data to the remote user.

```java
MediaProjectionManager projectManager = (MediaProjectionManager) mContext.getSystemService(
Context.MEDIA_PROJECTION_SERVICE);

// Create the intent for screen capture. Call the startActivityForResult method to use the sharing function.
Intent intent = projectManager.createScreenCaptureIntent();
startActivityForResult(intent);

MediaProjection projection;
VirtualDisplay display;

// Override and implement the onActivityResult method of the Activity where you just called startActivityForResult.
@Override
onActivityResult(int requestCode, int resultCode, Intent resuleData) {
    projection = projectManager.getMediaProjection(resultCode, resultData);
    display = projection.createVirtualDisplay(name, width, height, dpi, flags, surface, callback, handler);
}

// The texture retrieved from the Surface will be sent by the SDK.
rtcEngine.pushExternalVideoFrame(new AgoraVideoFrame(...));

// Stop screen sharing.
projection.stop();
```

We also provide an open-souce [Switch External Video](https://github.com/AgoraIO/Advanced-Video/blob/master/Android/sample-switch-external-video) demo project on GitHub. You can go to the repository to download it. You can try the demo and refer to the source code of the `startScreenShare` method in the [`switchVideoInputActivity.java`](https://github.com/AgoraIO/Advanced-Video/blob/master/Android/sample-switch-external-video/src/main/java/io/agora/advancedvideo/switchvideoinput/SwitchVideoInputActivity.java) file.

## Considerations

- Android API Level 21+ is required for API methods like MediaProjection. See [Google MediaProjection API  Reference](https://developer.android.com/reference/android/media/projection/MediaProjection).
- The code snippets on this page are shortened. See [Sample Code](https://github.com/AgoraIO/Advanced-Video/tree/dev/backup/Screensharing/Agora-Screen-Sharing-Android) on the design and implementation.
