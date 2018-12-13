
---
title: Join a Channel
description: 
platform: Android
updatedAt: Thu Dec 13 2018 22:53:16 GMT+0000 (UTC)
---
# Join a Channel
Before joining the channel, ensure that you have finished preparing the development environment. See [Integrate the SDK](../../en/Video/android_video.md) for more information.

## Implementation
You need to set the channel profile before the app joins a channel.

### Set the channel profile as Communication
After initializing the Agora engine, call the `setChannelProfile` method to set the channel profile. The Agora engine applies optimization according to the channel profile.

In the `setChannelProfile` method, set the channel profile as Communication. This profile applies to voice or video calls, such as one-to-one or group calls, where all users in the channel can talk freely. The Communication profile is the default setting.

> -   Call the `setChannelProfile` method before joining a channel.
> -   One engine can be specified one profile only. If you want to switch to another profile, destroy the current engine using the `destroy` method and create a new engine before calling the `setChannelProfile` method for setting the new channel profile.


```
mRtcEngine.setChannelProfile(Constants.CHANNEL_PROFILE_COMMUNICATION);
```


### Join a communication channel
Call the `joinChannel` method to join a channel. 

In the `joinChannel` method:

-   Pass a token that identifies the role and privilege of the user. Set token as null for low-security requirements. A token is generated at the server of the app. For how to generate a token, see [Security Keys](../../en/Video/token.md).
-   Pass a channel ID that identifies the channel. Users that input the same channel ID enter into the same channel.
-   Pass a uid that identifies the user. Each user in a channel requires a unique uid. If you want to join the same channel on different devices, ensure that different uids are used for each device.

> Once in a channel, a user must call the `leaveChannel` method to exit the current channel before entering another channel.

```
 private void joinChannel() {
    mRtcEngine.joinChannel(null, "demoChannel1", "Extra Optional Data", 0); // If you do not specify the uid, Agora will assign one.
}
```

## Next Steps
You are now in the channel and can start a voice call with the following step:

- [Publish and Subscrib to Streams](../../en/Video/publish_android.md)

For other functions such as manipulating the audio volume, audio effect, or video resolution, you can refer to the following sections:

- [Adjust the Volume](../../en/Video/volume_android.md)
- [Play Audio Effects/Audio Mixing](../../en/Video/effect_mixing_android.md)
- [Use In-Ear Monitoring](../../en/Video/in-ear_android.md)
- [Adjust the Pitch and Tone](../../en/Video/voice_effect_android.md)
- [Set the Video Profile](../../en/Video/videoProfile_android.md)
