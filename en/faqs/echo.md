
---
title: Echo occurs in a call.
description: 
platform: All Platforms
updatedAt: Mon Jul 01 2019 15:09:48 GMT+0800 (CST)
---
# Echo occurs in a call.
Echo is commonly caused by the device of the user who hears the echo. This problem can be fixed by using a headset in most cases, and ensure that the headset does not cause an echo. The Agora SDK supports echo cancellation.

## Step 1: Self-check

Check the following:

- Check if the echo is occasional or continuous. An occasional echo may be caused by CPU overload. If the CPU usage is too high, voice recording and playback will be unstable. You can check this in Agora Analytics in Dashboard.
- Ensure that all users are in separated physical environments.
- Check the SDK version:
	- Android/iOS: v1.6.0+.
	- Windows/macOS: v1.7.0+.
- Check if you enabled an external audio source. In this case, echo cancellation is not supported and the echo may be caused by data loss during the audio transmission to the SDK. 
- In Windows, ensure the `Monitoring Microphone` option is not checked. 
- On iOS, ensure that the `Mixable` option is not enabled. If it is enabled and another app is using the microphone, echo may occur.
- On Android, check whether the app sets the `Game Streaming` scenario in the `setAudioProfile` method. In this scenario, echo cancellation is turned off by default.
- Use a headset:
	- In a one-to-one call, if you hear an echo, ask the other user to use a headset.
	- In a multi-user call, ask users to mute in turn to figure out who causes the echo. The users who cause the echo should use headsets or mute themselves.

## Step 2: Contact Agora Customer Support

If the issue persists, contact Agora customer support and submit the issue with the following information:

<table>
  <tr>
    <th>Information</th>
    <th>Details</th>
  </tr>
  <tr>
    <td>Mandatory</td>
    <td>The name of the channel where the echo occurs.<br>The uids of the users who hear the echo.<br>The uid of the user who causes the echo.<br>The recording files, if available.</td>
  </tr>
  <tr>
    <td>Additional</td>
    <td>The time frame during which the users hear the echo.<br>If the issue exists after rejoining the channel.<br>If the issue exists after the user causing or hearing the echo switches the audio route (such as using a headset).</td>
  </tr>
</table>

## Step 3: Monitor the Quality of Experience in Agora Analytics in Dashboard

You can check the statistics of every call in [Agora Analytics](https://dashboard.agora.io/analytics/call/search) in Dashboard. For more information, see [Agora Analytics Tutorial](https://dashboard.agora.io/analytics/call/tutorial?_ga=2.197716463.1125435494.1542623251-764614247.1539586349).

