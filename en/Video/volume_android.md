
---
title: Adjust the Volume
description: How to adjust volume for Android
platform: Android
updatedAt: Thu Jan 17 2019 09:15:45 GMT+0000 (UTC)
---
# Adjust the Volume
## Introduction

When using the Agora SDK, developers can adjust the recording and playback volumes for customization. For example, you can mute the remote audio by setting the volume to 0.

This article describes the scenarios when you need to adjust the volume, the corresponding APIs and considerations in the process from audio recording to playing. 

![](https://web-cdn.agora.io/docs-files/1546416068141)

## Implementation
Ensure that you prepare the development environment. See [Integrate the SDK](../../en/Video/ios_video.md).

### Set the Recording Volume

**Recording** is the process in which audio signals are captured by recorders and transported to signal senders. During this process, you can adjust the volume by changing the volume of recording signals with the Agora SDK.

The value of the volume ranges between 0 and 400. 100 (default) represents the original volume, and 400 is four times the original volume (amplifying the audio signals by four times).

```java
// java
int volume = 200;
// Sets the volume of the recording signal.
rtcEngine.adjustRecordingSignalVolume(volume);
```

#### API Reference

- [`adjustRecordingSignalVolume`](https://docs.agora.io/en/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#af3747f72256eb683feadbca2b742bd05)

### Set the Playback Volume

**Playback** is the process in which audio signal are transported from signal senders to receivers and then to the players. During this process, you can adjust the volume by changing the volume of playback signals with the Agora SDK. 

The value of the volume ranges between 0 and 400. 100 (default) represents the original volume, and 400 is four times the original volume (amplifying the audio signals by four times).

```java
// java
int volume = 200;
// Sets the volume of the playback signal.
rtcEngine.adjustPlaybackSignalVolume(volume);
```

#### API Reference

- [`adjustPlaybackSignalVolume`](https://docs.agora.io/en/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#af7d7f10fc96db2febb9c2590891d071b)

### Set the Audio Mixing Volume

**Audio mixing** is playing local or online music while speaking, so that other users in the channel can hear the speaker and the music simultaneously. See [Audio Mixing](https://docs.agora.io/en/Video/effect_mixing_android?platform=Android#audio-mixing) for enabling this function.

The value of the audio mixing volume ranges between 0 and 100. 100 (default) represents the original volume, and 0 means the audio mixing is muted.

```java
// java
int volume = 50;
// Sets the audio mixing volume for remote users.
rtcEngine.adjustAudioMixingPublishVolume(volume);
// Sets the audio mixing volume for local users.
rtcEngine.adjustAudioMixingPlayoutVolume(volume);
```

You can also call the API `adjustAudioMixingVolume` to set the volume of audio playing for both remote users and local users.

```java
// java
int volume = 50;
// Sets the audio mixing volume for both local and remote users.
rtcEngine.adjustAudioMixingVolume(volume);
```

#### API Reference

- [`adjustAudioMixingPublishVolume`](https://docs.agora.io/en/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a16c4dc66d9c43eef9bee7afc86762c00)
- [`adjustAudioMixingPlayoutVolume`](https://docs.agora.io/en/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a0308c6bc82af433ae8340e0b3cd228c9)
- [`adjustAudioMixingVolume`](https://docs.agora.io/en/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a13c5737248d5a5abf6e8eb3130aba65a)

### Set the Audio Effects Volume

**Audio effects** are certain short-time sounds such as clapping and gunshots. See [Audio Effects](../../en/Video/effect_mixing_android.md) for enabling this function.

The value of the audio effects volume ranges between 0.0 and 100.0. 100 .0 (default) represents the original volume, and 0.0 means the audio effect is muted.

```java
// java
// Gets the global audio effect manager.
IAudioEffectManager manager = rtcEngine.getAudioEffectManager();
...
// Sets the audio effect volume to 50% of the original volume.
double volume = 50.0
// Sets the volume of all audio effect files.
manager.setEffectsVolume(volume);
// Sets the volume of a single audio effect file.
// soundId is ID of the audio effect when you call playEffect.
manager.setVolumeOfEffect(soundId, volume);
```

#### API Reference

- [`setEffectsVolume`](https://docs.agora.io/en/Video/API%20Reference/java/interfaceio_1_1agora_1_1rtc_1_1_i_audio_effect_manager.html#ab758558563b3dd70771e5d44ba1a96f3)
- [`setVolumeOfEffect`](https://docs.agora.io/en/Video/API%20Reference/java/interfaceio_1_1agora_1_1rtc_1_1_i_audio_effect_manager.html#afcd8cd6d733703c0ba153b8e1ac81ec0)

### Set the In-ear Monitoring Volume

In audio recording, mixing and playing, you can use `setInEarMonitoringVolume` to adjust the volume of in-ear monitoring.

The value of the in-ear monitoring volume ranges between 0 and 100. 100 (default) represents the original volume, and 0 means the in-ear monitoring is muted.

```java
// java
// Enables in-ear monitoring.
rtcEngine.enableInEarMoniroting(true);
int volume = 50;
// Sets the in-ear monitoring volume to 50% of original volume.
rtcEngine.setInEarMonitoringVolume(volume);
```

#### API Reference

- [`setInEarMonitoringVolume`](https://docs.agora.io/en/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#af71afdf140660b10c4fb0c40029c432d)

### Get the Data of the Loudest Speaker (Callback Method)

In audio recording, mixing and playing, you can use the following APIs to get the data of the loudest speaker in the channel.

- The loudest speaker at an instant

  This callback gets the ID and volume value of the loudest speaker at an instant. A user ID of 0 indicates it is a local user.

```java
// java
/**
 * Gets the ID of the loudest speakers at an instant.
 * @param speakers is an array that contains uid and volumne of the speaker, volume ranging between 0 and 255.
 * @param totalVolume is the toal volume after audio mixing, ranging between 0 to 255.
 */
public void onAudioVolumeIndication(AudioVolumeInfo[] speakers, int totalVolume) {
}
```

- The loudest speaker during a period

  This callback gets the ID of the loudest speaker for a certain period. A user ID of 0 indicates it is a local user.
	
```java
// java
// Gets the ID of the loudest speaker for a period.
public void onActiveSpeaker(int uid) {
}
```

#### API Reference
- [`onAudioVolumeIndication`](https://docs.agora.io/en/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a4d37f2b4d569fa787bb8c0e3ae8cd424)
- [`onActiveSpeaker`](https://docs.agora.io/en/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a895e965178d808f9d33b387ab3e50300)

## Considerations

- The API methods have return values. If the method fails, the return value is < 0.
- If the volume of the audio signal is set too high, noise may occur on some devices.

