
---
title: Set the Video Profile
description: 
platform: Windows
updatedAt: Mon Apr 01 2019 09:33:43 GMT+0800 (CST)
---
# Set the Video Profile
## Introduction

You can set the video profile, either before or after a user joins a channel, for the user to enjoy better video quality during a video call or live broadcast.

## Implementation

Ensure that you prepared the development environment. See [Integrate the SDK](../../en/Interactive%20Broadcast/windows_video.md).

The Agora SDK uses the `setVideoEncoderConfiguration` method to set the video profile. Each video profile corresponds to a set of video parameters, including the resolution, frame rate, bitrate, and video orientation.

The parameters specified in the `setVideoEncoderConfiguration` method are ideal values under ideal network conditions. If the video engine cannot render the video with the specified parameters due to poor network conditions, the parameters further down the list are considered.

```cpp
// Set the video encoder configuration.
// Set the width and height of the video stream. Swapping the two values does not change the video orientation.
VideoEncoderConfiguration (lpVideoConfig(640, 360),
// Set the video frame rate of the video.
FRAME_RATE_FPS_15,
// Set the bitrate of the video in Kbps.
800,
// The orientation mode the video.
ORIENTATION_MODE_ADAPTIVE,
// The degradation preference under limited bandwidth. MIANTAIN_QUALITY means to degrade the frame rate to maintain the video quality.
MAINTAIN_QUALITY
);

lpAgoraEngine->setVideoEncoderConfiguration(lpVideoConfig);
```

### API Reference
* [`setVideoEncoderConfiguration`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/v2.4/classagora_1_1rtc_1_1_i_rtc_engine.html#a9bcbdcee0b5c52f96b32baec1922cf2e)
* For more information on the video orientation mode, see [Rotate the Video](../../en/Interactive%20Broadcast/rotation_guide_android.md).

## Considerations

- Setting [`degradationPreference`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/v2.4/structagora_1_1rtc_1_1_video_encoder_configuration.html#a491316b0de64bf930938404b113f062f) as `MAINTAIN_QUALITY` means that the SDK degrades the frame rate under limited bandwidth so as to maintain the video quality. Developers can set the `minFrameRate` parameter to balance the frame rate and video quality under unreliable network connections:

	- When  `minFrameRate` is relatively low, the frame rate degrades significantly, so the poor network conditions have little impact on the video quality.
	- When `minFrameRate` is relatively high, the frame rate degrades within a limited range, so the poor network conditions can have huge impact on the video quality.

Do not set the `minFrameRate` parameter to a value greater than `frameRate`. The default value of `minFrameRate` is experiment verified and can satisfy most use scenarios. We do not recommend changing it.
- If you do not need to set the video profile after joining the channel, you can call the `setVideoEncoderConfiguration` method before the `enableVideo` method to reduce the render time of the first video frame.
- The Agora SDK may adjust the parameters under poor network conditions. 
-  A live broadcast channel generally requires a higher bitrate for better video quality. Therefore, Agora recommends setting the bitrate in the live broadcast profile to twice of that in the communication profile. See [Set the bitrate](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/structagora_1_1rtc_1_1_video_encoder_configuration.html#af10ca07d888e2f33b34feb431300da69).
- For better video quality during a live broadcast, a stable network connection is recommended.
- Setting parameters in the `setVideoEncoderConfiguration` method may affect your bill. For more information, see [Pricing and Billing](../../en/Agora%20Platform/billing_faq.md).

## Frequently Asked Questions

### How do I choose the video resolution, frame rate, and bitrate?

Video profiles vary from case to case. For example, in a one-to-one online class, the video windows of the teacher and student are both large, which requires higher resolutions, frame rates, and bitrates. While in a one-to-four online class, the video windows of the teacher and students are smaller, so lower resolutions, frame rates, and bitrates are used to accommodate the downward bandwidth.

 The following profiles for different scenarios are recommended:

- One-to-one video call: 
  - Resolution: 320 x 240; frame rate: 15 fps; bitrate: 200 Kbps
  - Resolution: 640 x 360; frame rate: 15 fps; bitrate: 400 Kbps
- One-to-many video call: 
  - Resolution: 160 x 120; frame rate: 15 fps; bitrate: 65 Kbps
  - Resolution: 320 x 180; frame rate: 15 fps; bitrate: 140 Kbps
  - Resolution: 320 x 240; frame rate: 15 fps; bitrate: 200 Kbps 

You can also customize the video parameters with the `setVideoEncoderConfiguration` method, such as increasing the bitrate to ensure the video quality.
