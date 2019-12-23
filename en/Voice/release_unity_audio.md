
---
title: Release Notes
description: 
platform: Unity
updatedAt: Mon Dec 23 2019 07:05:06 GMT+0800 (CST)
---
# Release Notes
This page provides the release notes for the Agora Unity SDK.

## Overview

The Agora Unity SDK supports the following scenarios:

-   Voice Communication
-   Live Voice Broadcast

For the key features included in each scenario, see [Voice Overview](https://docs.agora.io/en/Voice/product_voice?platform=All%20Platforms) and [Audio Broadcast Overview](https://docs.agora.io/en/Audio%20Broadcast/product_live_audio?platform=All_Platforms).

## v2.9.1

Agora Unity SDK is widely used in games, education, AR, VR and other scenarios.

v2.9.1 was released on December 23, 2019.

**Functions and features**

#### 1. Multi-platform support
Supports ios, Android, macOS, Windows x86, and Windows x86_64 platforms.

#### 2. Interoperability with the Agora Web SDK
Provides the `EnableWebSdkInteroperability` method for enabling interoperability with the Agora Web SDK in a live broadcast. 

#### 3. Raw data
Supports raw audio data. You can capture and process the raw data according to your needs.

#### 4. External data source
Provides APIs for accessing to the external data source. You can configure the external audio source, and push the data to the Agora Unity SDK.

#### 5. Encryption
Supports encryption of audio and video streaming. The following table shows the information of the encryption libraries for the Android and iOS platforms. If you do not intend to use this function, you can remove the encryption libraries to decrease the SDK size.

   | Platform | Encryption libraries                          |
   | :------- | :-------------------------------------------- |
   | Android  | libagora-crypto.so                            |
   | iOS      | <ul><li>AgoraRtcCryptoLoader.framework <li> ibcrypto.a</li></ul> |

**Related documentation**

See the following documentation to quickly integrate the SDK and implement real-time voice and video communication in your project.

- [Start a Voice Call](https://docs.agora.io/en/Voice/start_call_audio_unity?platform=Unity) or [Start a Voice Broadcast](https://docs.agora.io/en/Audio%20Broadcast/start_live_audio_unity?platform=Unity)
- [API Reference](https://docs.agora.io/en/Voice/API%20Reference/unity/index.html) 

Agora also provides an open-source [Unity Sample](https://github.com/AgoraIO/Voice-Call-for-Mobile-Gaming/tree/2.9.1.42/Basic-Voice-Call-for-Gaming/Hello-Unity3D-Agora) on GitHub.
