
---
title: Agora Voice Call Overview
description: 
platform: All Platforms
updatedAt: Mon Sep 14 2020 10:31:06 GMT+0800 (CST)
---
# Agora Voice Call Overview
Agora Voice Call enables easy and convenient one-to-one or one-to-many **voice-only** calls with the Agora RTC SDK. With a small SDK package size, the Agora RTC SDK is applicable to a variety of recreational and business activities.

Agora Voice Call is different from [Agora Live Interactive Audio Streaming](https://docs.agora.io/en/Audio%20Broadcast/product_live_audio?platform=All%20Platforms). In a voice call, all users are the same role and can talk to each other freely. In a live audio streaming, users can be the host or audience, where only the host can talk. For details, see this [FAQ](https://docs.agora.io/en/faq/profile_difference).

## Functions and scenarios

Agora Voice Call boasts a flexible combination of functions for different scenarios.

<style> table th:first-of-type {     width: 150px; } th:third-of-type {     width: 170px; }</style>

| Function                                   | Description                                                  | Scenario                                                     |
| ------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Audio mixing                               | Sends the local and online audio with the user's voice to other audience members in the channel. | <li>Online KTV. <li>Interactive music classes.               |
| Play the sound effect files                | Enables developers to play specific sound effect files, adjust the volume, and set the playback position of the sound effect files. | Online chess or card games.                                  |
| Voice changer and reverberation            | Provides multiple presets to easily change the voice and set reverberation effects, also supports adjusting the pitch and using the equalization and reverberation modes flexibly. | <li>Online KTV.<li>To change the voice in an online chatroom. |
| Spatial sound effects                      | Sets the spatial sound effects for remote users to provide immersive experiences. | FPS games.                                                   |
| Enable two-channel/high-quality sound mode | Enables the two-channel and the high-quality sound mode.     | <li>Online music classes.<li> FM applications.               |
| Modify the raw data                        | Enables developers to obtain and modify the raw voice data to create special effects, such as a voice change. | To change the voice in an online voice chatroom.             |

## Key properties

| Property                    | Agora Voice Call Specifications                              |
| --------------------------- | ------------------------------------------------------------ |
| SDK package size            | 3.14 MB to 5.88 MB                                           |
| Audio profile               | <li>Sample rate: 16 kHz to 48 kHz<li>Support for mono and stereo sound |
| Audio anti-packet-loss rate | 80% (uplink and downlink)                                    |

## Compatibility

Agora Voice Call is supported on platforms such as iOS, Android, Windows, macOS, Electron, Unity, and Web, and allows for cross-platform connections. The following is a list of supported platforms and their versions.

| Platform             | Supported Version                                            |
| -------------------- | ------------------------------------------------------------ |
| Android              | 4.1+<br>The Android SDK supports the following ABIs:<li>armeabi-v7a<li>arm64-v8a<li>x86<li>x86_64 |
| iOS                  | 8.0+                                                         |
| Windows              | Windows 7+<br>The Windows SDK supports the following architecture:<li>x86<li>x64                                                      |
| macOS                | 10.0+                                                        |
| Unity                | <p>2017+</p><p>The Unity SDK supports the following platforms:<p><ul><li>Android (armeabi-v7a, arm64-v8a, x86)<li>iOS<li>Windows (x86, x86_64)<li>macOS                                                        |
| Web                  | <li>Chrome 58+<li>Chrome 49 on Windows XP<li>Firefox 56+<li>Safari 11+<li>Opera 45+<li>QQ 10+<li>360 Security Browser 9.1+ |

<div class="alert note">The browser support on the Web platform varies with the device and system. See <a href="https://docs.agora.io/cn/faq/browser_support">Which browsers does the Agora Web SDK support?</a></div>
	

## Reference

[How many users can Agora RTC SDK support at the same time?](https://docs.agora.io/en/faq/capacity)
