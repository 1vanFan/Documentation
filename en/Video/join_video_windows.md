
---
title: Join a Channel
description: 
platform: Windows
updatedAt: Thu Dec 13 2018 22:28:04 GMT+0000 (UTC)
---
# Join a Channel
Before joining a channel, ensure that you prepared the development environment. See [Integrate the SDK](../../en/Video/windows_video.md).

## Implementation
You need to set the channel profile before the app joins a channel.

### Set the channel profile as Communication
After initializing the Agora engine, call the `setChannelProfile` method to set the channel profile. Agora engine applies optimization according to the channel profile.

In the `setChannelProfile` method, set the channel profile as Communication. This profile applies to voice or video calls such as one-to-one or group calls, where all users in the channel can talk freely. The Communication profile is the default setting.

> -   Call the `setChannelProfile` method before joining a channel.
> -   One engine can be specified one profile only. If you want to switch to another profile, destroy the current engine using the `destroy` method and create a new engine before calling the `setChannelProfile` method to set the new channel profile.

```
nRet = m_lpAgoraEngine->setChannelProfile(CHANNEL_PROFILE_COMMUNICATION);
```

### Join a communication channel
Call the <code>joinChannel</code> method to join a channel. 

In the <code>joinChannel</code> method:

-   Pass a token that identifies the role and privilege of the user. Set token as null for low-security requirements. A token is generated at the server of the application. For how to generate a token, see [Security Keys](../../en/Video/token.md).

-   Pass a channel ID that identifies the channel. Users that input the same channel ID enter into the same channel.

-   Pass a uid that identifies the user. Each user in a channel requires a unique uid. If you want to join the same channel on different devices, ensure that different uids are used for each device.


> Once in a channel, a user must call the <code>leaveChannel</code> method to exit the current channel before entering another channel.

```
LPCSTR lpStreamInfo = "{\"owner\":true,\"width\":640,\"height\":480,\"bitrate\":500}";
nRet = m_lpAgoraEngine->joinChannel(lpDynamicKey, lpChannelName, lpStreamInfo, nUID);
```

## Next Steps
You are in the channel and can start a video call with the following step:

- [Publish and Subscribe to Streams](../../en/Video/publish_windows.md)

For more functions, you can refer to the following sections:

- [Adjust the Volume](../../en/Video/volume_windows.md)
- [Play Audio Effects/Audio Mixing](../../en/Video/effect_mixing_windows.md)
- [Adjust the Pitch and Tone](../../en/Video/voice_effect_windows.md)
- [Set the Video Profile](../../en/Video/videoProfile_windows.md)
