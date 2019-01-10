
---
title: Release Notes
description: 
platform: Android
updatedAt: Thu Jan 10 2019 17:28:47 GMT+0000 (UTC)
---
# Release Notes
## Introduction
The Agora Android Voice SDK for Interactive Gaming, provides Java and C++ Interfaces to implement the voice function for gaming on the Android platform. 

For the key features included, see [Interactive Gaming Overview](https://docs.agora.io/en/Interactive%20Gaming/product_gaming?platform=All%20Platforms).

## v2.2
Developed on the basis of Agora Native SDK and Agora C++ SDK, the version 2.2 was released on Oct. 31. See below for new features.

### New Features

#### 1. Set the Voice-only Mode

This release adds a `setVoiceOnlyMode` API to set the voice only mode.  Once it is enabled, the SDK transmits only the voice stream, but not other streams, like the sound of keyboard strokes.  This function can effectively optimize the audio quality.

#### 2. Set the Audio Effect Position

This release adds a `setRemoteVoicePosition` to set the position and volume of the audio effects sent from the remote user.

#### 3. Disables/Re-enables the Local Audio Function

When a user joins a channel, the audio function is enabled by default.
To receive audio streams without sending any audio stream after joining a channel, this release adds the `enableLocalAudio` method to disable or re-enable the local audio function.
Once the local audio function is disabled or re-enabled, the SDK returns the `onMicrophoneEnabled` callback function, and the local audio capturing stops.
This method does not affect receiving or playing the remote audio streams.
