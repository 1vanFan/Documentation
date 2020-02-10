
---
title: Agora Video Overview
description: 
platform: All Platforms
updatedAt: Tue Dec 17 2019 10:36:58 GMT+0800 (CST)
---
# Agora Video Overview
The Agora Native SDK for Video Call enables easy and convenient one-to-one or one-to-many calls and supports voice-only and video modes.

The difference between an Agora Video Call and [Agora Video Interactive Broadcast](https://docs.agora.io/en/Interactive%20Broadcast/product_live?platform=All%20Platforms) is: 
* An Agora Video Call prioritizes smoothness and low latency. All users are the same role and can talk to each other freely. A typical scenario of an Agora Video Call is a video conference call for many persons. 
* An Agora Video Interactive Broadcast prioritizes high voice quality. Users can be the host or audience, where only the host can talk. A user who wants to talk must change the role to a host. A typical scenario of an Agora Video Interactive Broadcast is an interactive online class.

## Functions and Scenarios

The Agora Native SDK for Video Call boasts a flexible combination of functions for different scenarios.

<style> table th:first-of-type {     width: 150px; } th:third-of-type {     width: 170px; }</style>

| Function                              | Description                                                  | Scenario                                                     |
| ----------------- | ------------------------------------------------------------ | --------------------------------------- |
| Audio mixing          | Sends the local and online audio with the user's voice to other users in the channel. | <li>Online KTV. <li>Interactive music classes. |
| Screen sharing             | Enables the local user to share the screen to other users in the channel. Supports specifying which screen or which window to share, and supports specifying the sharing region.            | Interactive online classes.     |
| Basic image enhancement     | Sets basic beauty effects, including skin smoothening, whitening, and cheek blushing. | Image enhancement in a video call.    |
| Modify the raw data   | Enables developers to obtain and modify the raw voice or video data and to create special effects, such as a voice change. | <li>To change the voice in an online chatroom. <li>Image enhancement in a video call.                  |
| Customize the video source and renderer | Enables customization of the video sources and renderers. This allows users to use self-built cameras and videos from screen sharing or files to process videos, such as for image enhancement and filtering. | <li>To use a customized image enhancement library or pre-processing library.<li>To customize the application's built-in image and video modules.<li>To use other video sources, such as a recorded video.<li>To provide flexible device management for exclusive video capture devices to avoid conflicts with other services. |

## Key Properties

| Property                                          | Agora Video Call Specifications                          |
| ------------ | ------------------------------------------------------------ |
| SDK Package Size                                  | 3.69 MB to 7.75 MB                                              |
| Capacity     | Seven users (To support more users, use [Agora Interactive Broadcast](https://docs.agora.io/en/Interactive%20Broadcast/product_live?platform=All%20Platforms)) |
| Video Profile                                     | <li>SDK video source: Up to 1080p @ 60 fps<li>Custom video source: Up to 4K |
| Audio Profile                                     | <li>Sample rate: 16 kHz to 48 kHz<li>Support for mono and stereo sound |
| Audio Anti-packet-loss Rate                       | 70% (uplink and downlink)                               |

## Compatibility

The Agora Native SDK for Video Call is supported on platforms such as iOS, Android, Windows, macOS, Electron, Linux, Web, and WeChat Mini-programs, and allows for cross-platform connections. The following is a list of supported platforms and their versions.

| Platform             | Supported Version                                            |
| -------------------- | ------------------------------------------------------------ |
| Android              | 4.1+<br>The Android SDK supports the following ABIs:<li>armeabi-v7a<li>arm64-v8a<li>x86<li>x86_64 |
| iOS                  | 8.0+                                                         |
| Windows              | XP SP3+<br>The Windows SDK supports the following architecture:<li>x86<li>x64                                                      |
| macOS                | 10.0+                                                        |
| WeChat Mini-Programs | Support                                                      |
| Web                  | <li>Chrome 58+<li>Chrome 49 on Windows XP<li>Firefox 56+<li>Safari 11+<li>Opera 45+<li>QQ 10+<li>360 Security Browser 9.1+ |
