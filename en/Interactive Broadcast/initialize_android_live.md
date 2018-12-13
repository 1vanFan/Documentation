
---
title: Create and Initialize an Agora Instance
description: 
platform: Android
updatedAt: Thu Dec 13 2018 22:09:41 GMT+0000 (UTC)
---
# Create and Initialize an Agora Instance
Before creating an RtcEngine instance, ensure that you have finished preparing the development environment. See [Integrate the SDK](../../en/Video/android_video.md) for more information.

## Implementation
The following imports define the interface of the Agora API that provides communication functionality:

-   `io.agora.rtc.Constants`
-   `io.agora.rtc.IRtcEngineEventHandler`
-   `io.agora.rtc.RtcEngine`
-   `io.agora.rtc.video.VideoCanvas`

Create a singleton by invoking `create` during initialization. In this method:

-  Pass the Agora App ID. Only Applications with the same App ID can join the same channel.
-  Specify a reference to the activity’s event handler. The Agora API uses events to inform the application about Agora engine runtime events, such as joining or leaving a channel and adding new participants.

```
import io.agora.rtc.Constants;
import io.agora.rtc.IRtcEngineEventHandler;
import io.agora.rtc.RtcEngine;

...

private void initializeAgoraEngine() {
    try {
        mRtcEngine = RtcEngine.create(getBaseContext(), getString(R.string.agora_app_id), mRtcEventHandler);
    } catch (Exception e) {
        Log.e(LOG_TAG, Log.getStackTraceString(e));

        throw new RuntimeException("NEED TO check rtc sdk init fatal error\n" + Log.getStackTraceString(e));
    }
}
```

## Next Steps
You have now finished creating the RtcEngine instance and can start a live broadcast with the following steps:
* [Join a Channel](../../en/Interactive%20Broadcast/join_live_android.md)
* [Switch the User Role](../../en/Interactive%20Broadcast/role_android.md)
* [Publish and Subscribe to Streams](../../en/Interactive%20Broadcast/publish_android_live.md)

For added requirements on network connection or audio quality, you can also take the following steps before joining a channel:
* [Conduct a Last mile Test](../../en/Interactive%20Broadcast/lastmile_android.md)
* [Set the Stereo/High-fidelity Audio Profile](../../en/Interactive%20Broadcast/audio_profile_android.md)
