
---
title: Agora Voice Overview
description: 
platform: All Platforms
updatedAt: Tue Apr 30 2019 14:24:26 GMT+0800 (CST)
---
# Agora Voice Overview
The Agora Native SDK for Voice Call enables easy and convenient one-to-one or one-to-many **voice-only** calls. With a small SDK package size, the Agora Native SDK for Voice Call is applicable to a variety of recreational and business activities.

The difference between an Agora Voice Call and [Agora Voice Interactive Broadcast](https://docs.agora.io/en/Interactive%20Broadcast/product_live?platform=All%20Platforms) is: 
* An Agora Voice Call prioritizes smoothness and low latency. All users are the same role and can talk to each other freely. A typical scenario of an Agora Voice Call is a voice conference call for many persons.
* An Agora Voice Interactive Broadcast prioritizes high voice quality. Users can be the host or audience, where only the host can talk. A user who wants to talk must change the role to a host. A typical scenario of the Agora Voice Interactive Broadcast is an online trivia game.

## Functions and Scenarios

The Agora Native SDK for Voice Call boasts a flexible combination of functions for different scenarios.

| Function                              | Description                                                  | Scenario                                                     |
| ----------------- | ------------------------------------------------------------ | --------------------------------------- |
| Audio mixing                          | Sends the local and online audio with the user's voice to other audience members in the channel. | <li>Online KTV. <li>Interactive music classes.    |
| Play the sound effect files          | Enables developers to play specific sound effect files, adjust the volume, and set the playback position of the sound effect files.        | Online chess or card games.                                |
| Voice changer and reverberation     | Provides multiple presets to easily change the voice and set reverberation effects, also supports adjusting the pitch and using the equalization and reverberation modes flexibly.                    | <li>Online KTV.<li>To change the voice in an online chatroom.        |
|Spatial Sound Effects |	Sets the spatial sound effects for remote users to provide immersive experiences.|	FPS games. |
| Enable Two-channel/High-quality Sound Mode | Enables the two-channel and the high-quality sound mode.                               | <li>Online music classes.<li> FM applications.        |
| Modify the Raw Data                    | Enables developers to obtain and modify the raw voice data to create special effects, such as a voice change. | To change the voice in an online voice chatroom. |

## Key Properties

| Property                                          | Agora Voice Call Specifications                          |
| ------------ | ------------------------------------------------------------ |
| SDK Package Size                                  | 3.69 MB to 7.75 MB                                              |
| Audio Profile                                     | <li>Sample rate: 16 KHz to 48 KHz<li>Support for mono and stereo sound |
| Audio Anti-packet-loss Rate                       | 70% (uplink and downlink)                               |

## Compatibility

The Agora Native SDK for Voice Call is supported on platforms such as iOS, Android, Windows, macOS, Linux, Web, and WeChat mini-programs, and allows for cross-platform connections. The following is a list of supported platforms and their versions.

| Platform             | Supported Version                                            |
| -------------------- | ------------------------------------------------------------ |
| Android              | 4.1+<br>The Android SDK supports the following architecture:<li>ARMv7<li>ARM64<li>X86 |
| iOS                  | 8.0+                                                         |
| Windows              | XP SP3+                                                      |
| macOS                | 10.0+                                                        |
| WeChat Mini-Programs | Support                                                      |
| Web                  | <li>Chrome 58+<li>Chrome 49 on Windows XP<li>Firefox 56+<li>Safari 11+<li>Opera 45+<li>QQ 10+<li>360 Security Browser 9.1+ |
