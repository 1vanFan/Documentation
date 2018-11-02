
---
title: Pushing Streams to the CDN
description: 
platform: Android
updatedAt: Thu Nov 01 2018 10:12:58 GMT+0000 (UTC)
---
# Pushing Streams to the CDN
# Pushing Streams to the CDN

Agora's CDN publishing solution is based on the following APIs to publish streams to the CDN, inject external video streams, transcode, and set the output layout.

-   `addPublishStreamUrl`
-   `removePublishStreamUrl`
-   `setLiveTranscoding`

This solution is flexiible. It allows:

-   Starting or stopping publishing to the CDN.
-   Adding or removing a streaming URL without interrupting the ongoing publishing.
-   Adding extra controls to the ongoing streams.
-   Using callbacks to monitor the status of the publishing.
-   Quick migration from the legacy approach to the new approach.


## Pushing Streams to the CDN

Contact [sales@agora.io](mailto:sales@agora.io) to enable this function.

> You will be able to enable this function in the Dashboard in future releases.
> -   A host can dynamically add or remove a URL after joining the channel.
> -   A host can set transcoding and the layout, for example, the canvas settings and multiple-host window settings, only after joining a channel.

The following figure shows a typical CDN-pushing scenario.

<img alt="../_images/live_ios_publishing_stream_en.png" src="https://web-cdn.agora.io/docs-files/en/live_ios_publishing_stream_en.png"/>


### Sample Code:

```
// CDN transcoding settings.
LiveTranscoding config;
config.audioSampleRate = TYPE_44100;
config.audioChannels = 2;
// config.audioBitrate
config.width = 640;
config.height = 720;
config.videoFramerate = 30;
config.userCount = 1;  // If userCount > 1, set layout for each user.
config.videoCodecProfile = HIGH;

// Sets the output layout for each user.
LiveTranscoding transcoding = new LiveTranscoding();
LiveTranscoding.TranscodingUser user = new LiveTranscoding.TranscodingUser();
user.uid = 123456;
transcoding.addUser(user);
user.x = 0;
user.audioChannel = 2;
user.y = 0;
user.width = 640;
user.height = 720;

rtcEngine.setLiveTranscoding(transcoding);
```

```
// Add a URL to which the host pushes a stream.
rtcEngine.addPublishStreamUrl(url, false);
```

```
// Remove a URL to which the host pushes a stream.
rtcEngine.removePublishStreamUrl(url);
```

## Adjusting the Picture-in-picture Layout

Use transcodingUser to adjust the picture-in-picture layout when a channel has multiple hosts.

> A host can set transcoding and the layout, for example, the canvas settings and multiple-host window settings, only after joining a channel.

### Example 1: Two-host Tile Horizontally

To display the following layout:

<img alt="../_images/sei_2host.png" src="https://web-cdn.agora.io/docs-files/en/sei_2host.png" style="width: 416.0px; height: 240.0px;"/>

Set the parameters as follows:

```
Canvas:
     width: 360
     height: 640
     backgroundColor: #FFFFFF

User0:
      userId: 123
      x: 0
      y: 0
      width: 320
      height: 360
      zOrder: 1
      alpha: 1.0

User1:
      userId: 456
      x: 320
      y: 0
      width: 320
      height: 360
      zOrder: 1
      alpha: 1.0
```

### Example 2: Three-host Tile Vertically

To display the following layout:

<img alt="../_images/sei_3host.png" src="https://web-cdn.agora.io/docs-files/en/sei_3host.png" style="width: 236.0px; height: 416.0px;"/>

Set the parameters as follows:

```
Canvas:
      width: 360
      height: 640
      backgroundColor: #FFFFFF

   User0:
      userId: 123
      x: 0
      y: 0
      width: 360
      height: 640
      zOrder: 1
      alpha: 1.0

   User1:
       userId: 456
       x: 0
       y: 320
       width: 180
       height: 320
       zOrder: 2
       alpha: 1.0

   User2:
       userId: 789
       x: 180
       y: 320
       width: 180
       height: 320
       zOrder: 2
       alpha: 1.0
```

### Example 3: One Full Screen + Floating Thumbnails

To display the following layout:

<img alt="../_images/sei_random.png" src="https://web-cdn.agora.io/docs-files/en/sei_random.png" style="width: 828.0px; height: 416.0px;"/>

Set the parameters as follows:

```
Canvas:
   width: 360
   height: 640
   backgroundColor: #FFFFFF

User0:
   userId: 123
   x: 0
   y: 0
   width: 360
   height: 640
   zOrder: 1
   alpha: 1.0

User1:
    userId: 456
    x: 45
    y: 390
    width: 110
    height: 213
    zOrder: 2
    alpha: 1.0

User2:
    userId: 789
    x: 185
    y: 390
    width: 110
    height: 213
    zOrder: 2
    alpha: 1.0
```


