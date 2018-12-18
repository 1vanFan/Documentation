
---
title: Set the Stereo/High-fidelity Audio Profile
description: How to set high-quality audio
platform: Android
updatedAt: Tue Dec 18 2018 02:58:32 GMT+0000 (UTC)
---
# Set the Stereo/High-fidelity Audio Profile
## Feature Description 
High-fidelity audio is essential for professional audio scenarios, such as for podcasts and singing competitions. For example, podcasts require stereo and high-fidelity audio. High-fidelity audio refers to an audio profile with a 48-KHz sampling rate and a 192-Kbps bitrate. 


## Implementation
Ensure that you prepared the development environment. See [Integrate the SDK](../../en/Interactive%20Broadcast/android_video.md).

The Agora SDK provides the [setAudioProfile](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a34175b5e04c88d9dc6608b1f38c0275d) method for developers to set appropriate audio profiles according to the scenarios. This method has two parameters:

- `profile` sets the sampling rate, bitrate, encoding mode, and the number of channels.
- `scenario` sets the audio application scenario. For example, entertainment, education, or live gaming. The SDK optimizes the noise control and audio quality based on the scenarios.

Besides the stereo and high-fidelity audio qualities, the following sample code shows the frequently used parameter settings for your reference.

```java
// FM high-fidelity
rtcEngine.setAudioProfile(Constants.AUDIO_PROFILE_MUSIC_HIGH_QUALITY_STEREO, Constants.AUDIO_SCENARIO_SHOWROOM);

// Gaming
rtcEngine.setAudioProfile(Constants.AUDIO_PROFILE_SPEECH_STANDARD, Constants.AUDIO_SCENARIO_CHATROOM_GAMING);

// Entertainment
rtcEngine.setAudioProfile(Constants.AUDIO_PROFILE_MUSIC_STANDARD, Constants.AUDIO_SCENARIO_CHATROOM_ENTERTAINMENT);

// KTV
rtcEngine.setAudioProfile(Constants.AUDIO_AUDIO_PROFILE_MUSIC_HIGH_QUALITY, Constants.AUDIO_SCENARIO_CHATROOM_ENTERTAINMENT);
```

## Considerations

- Call the [setAudioProfile](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a34175b5e04c88d9dc6608b1f38c0275d) method before joining the channel.
- The `scenario`  parameter takes effect only when the channel profile is live broadcast.
