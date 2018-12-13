
---
title: Join a Channel
description: 
platform: Android
updatedAt: Thu Dec 13 2018 22:05:52 GMT+0000 (UTC)
---
# Join a Channel
Before joining the channel, ensure that you have finished preparing the development environment. See [Integrate the SDK](../../en/Interactive%20Broadcast/android_video.md) for more information.

## Implementation
You need to set the channel profile before the app joins a channel.

### Set the channel profile as Live Broadcast
After initializing RtcEngine, call the `setChannelProfile` method to set the channel profile. RtcEngine applies optimization according to the channel profile.

In the `setChannelProfile` method, set the channel profile as Live Broadcast. This profile applies to an interactive broadcast scenario. Each channel includes two roles: Host and Audience. The host\(broadcaster\) sends and receives audio and video streams while the audience can only receive the audio and video streams.

> -   Call the `setChannelProfile` method before joining a channel.
> -   One engine can be specified one profile only. If you want to switch to another profile, destroy the current RtcEngine using the `destroy` method and create a new RtcEngine before calling the `setChannelProfile` method to set the new channel profile.

```
mRtcEngine.setChannelProfile(Constants.CHANNEL_PROFILE_LIVE_BROADCASTING);
```

### Join a live broadcast channel
Call the `joinChannel` method to join a channel. 

In the `joinChannel` method:

-   Pass a token that can identify the role and privilege of the user. Set token as null for low security requirements. A token is generated at the server of the application. For how to generate a token, see [Security Keys](../../en/Interactive%20Broadcast/token.md).
-   Pass a channel ID that can identify the channel. Users that input the same channel ID enter into the same channel.
-   Pass a uid that can identify the user. Every user in a channel requires a unique uid. If you want to join the same channel on different devices, ensure that different uids are used for each device.

> Once in a channel, a user must call the `leaveChannel` method to exit the current channel before entering another channel.

```
 private void joinChannel() {
    mRtcEngine.joinChannel(null, "demoChannel1", "Extra Optional Data", 0); // if you do not specify the uid, Agora will assign one.
}
```

## Next Steps
You are now in the channel and can start a live broadcast with the following step:

- [Switch the Client Role](../../en/Interactive%20Broadcast/role_android.md)
- [Publish and Subscrib to Streams](../../en/Interactive%20Broadcast/publish_android_live.md)

For other functions such as manipulating the audio volume, audio effect, or video resolution, you can refer to the following sections:

- [Adjust the Volume](../../en/Interactive%20Broadcast/volume_android.md)
- [Play Audio Effects/Audio Mixing](../../en/Interactive%20Broadcast/effect_mixing_android.md)
- [Use In-Ear Monitoring](../../en/Interactive%20Broadcast/in-ear_android.md)
- [Adjust the Pitch and Tone](../../en/Interactive%20Broadcast/voice_effect_android.md)
- [Set the Video Profile](../../en/Interactive%20Broadcast/videoProfile_android.md)

