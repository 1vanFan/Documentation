
---
title: Adjust the Volume
description: How to adjust volume on Web
platform: Web
updatedAt: Sat Dec 29 2018 07:30:01 GMT+0000 (UTC)
---
# Adjust the Volume
## Introduction
When using the Agora SDK, developers can adjust the recording and playout volumes for customization. For example, you can mute the remote audio by setting the volume to 0.
## Implementation
Before proceeding, ensure that you prepared the development environment. See [Integrate the SDK](../../en/Interactive%20Broadcast/web_prepare.md).

### Adjust the Playout Volume

```
 client.on("stream-subscribed", function(evt){
  var stream = evt.stream;
  // Sets the volume of the remote stream to 50.
  stream.setAudioVolume(50);
 });
```

### Mute the Remote User

```
 client.on("stream-subscribed", function(evt){
  var stream = evt.stream;
  // Mutes the remote stream.
  stream.setAudioVolume(0);
 });
```

## Considerations

- If the volume is set too high, noise may occur on some devices.
