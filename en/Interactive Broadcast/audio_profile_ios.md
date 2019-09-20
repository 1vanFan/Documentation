
---
title: Set the Audio Profile
description: How to set the high-quality audio for iOS and macOS
platform: iOS
updatedAt: Fri Sep 20 2019 09:56:07 GMT+0800 (CST)
---
# Set the Audio Profile
## Introduction 

High-fidelity audio is essential for professional audio scenarios, such as for podcasts and singing competitions. For example, podcasts require stereo and high-fidelity audio. High-fidelity audio refers to an audio profile with a sample rate of 48 Hz and a bitrate of 192 Kbps.

To obtain high-fidelity audio during real-time communications, you can choose the appropriate audio profile based on the audio quality, channel, and scenario.

## Implementation

The Agora SDK provides the [`setAudioProfile`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setAudioProfile:scenario:) method to set the appropriate audio profile according to the scenario. This method has two parameters:
<table>
<tr>
<th>Parameters</th>
<th>Description</th>
</tr>
<tr>
<td>profile</td>
<td>Sets the sample rate, bitrate, encoding mode, and the number of channels.
	<li>AgoraAudioProfileDefault(0): The default audio profile. In the Communication profile, the default value is AgoraAudioProfileSpeechStandard(1). In the Live-broadcast profile, the default value is AgoraAudioProfileMusicStandard(2).</li>
	<li>AgoraAudioProfileSpeechStandard(1): A sample rate of 32 kHz, audio encoding, mono, and a bitrate of up to 18 Kbps.</li>
	<li>AgoraAudioProfileMusicStandard(2): A sample rate of 48 kHz, music encoding, mono, and a bitrate of up to 48 Kbps.</li>
	<li>AgoraAudioProfileMusicStandardStereo(3): A sample rate of 48 kHz, music encoding, stereo, and a bitrate of up to 56 Kbps.</li>
	<li>AgoraAudioProfileMusicHighQuality(4): A sample rate of 48 kHz, music encoding, mono, and a bitrate of up to 128 Kbps.</li>
	<li>AgoraAudioProfileMusicHighQualityStereo(5): A sample rate of 48 kHz, music encoding, stereo, and a bitrate of up to 192 Kbps.</li>
	</td>
</tr>
<tr>
<td>scenario</td>
<td>Sets the audio scenario. For example, entertainment, education, or live gaming. The Agora SDK optimizes the noise control and audio quality based on the scenarios.
 <li>AgoraAudioScenarioDefault(0): The default scenario.</li>
 <li>AgoraAudioScenarioChatRoomEntertainment(1): The entertainment scenario, supporting voice during gameplay.</li>
 <li>AgoraAudioScenarioEducation(2): The education scenario, prioritizing smoothness and stability.</li>
 <li>AgoraAudioScenarioGameStreaming(3): The live gaming scenario, enabling the gaming audio effects in the speaker mode in a live broadcast scenario. Choose this scenario for high-fidelity music playback.</li>
 <li>AgoraAudioScenarioShowRoom(4): The showroom scenario, optimizing the audio quality with external professional equipment.</li>
 <li>AgoraAudioScenarioChatRoomGaming(5): The gaming scenario.</li>
	</td>
</tr>
</table>

### Parameters and Applications

You can set the profile and scenario parameters based on the requirements of different applications, such as the audio quality and the audio player.

<table>
<tr>
<th>Parameters</th>
<th>Applications</th>
<th>Options</th>
</tr>
<tr>
<td rowspan="3">profile</td>
<td>High-fidelity audio</td>
<td><li>MusicHighQuality</li>
	<li>MusicHighQualityStereo</li></td>
</tr>
	<tr>
<!--<td></td>-->
<td>Standard audio</td>
<td><li>MusicStandard</li>
	<li>MusicStandardStereo</li></td>
</tr>
		<tr>
<!--<td></td>-->
<td>No requirement for audio quality</td>
<td><li>Default</li>
	<li>SpeechStandard</li></td>
</tr>
<tr>
<td rowspan="5">scenario</td>
	<td>Prioritize the audio quality and effects</td>
<td>GameStreaming</td>
	</tr>
		<tr>
<!--<td></td>-->
	<td>Frequently mute/unmute the microphone</td>
<td>ChatRoomEntertainment</td>
</tr>
	<tr>
<!--<td></td>-->
	<td>External audio player</td>
<td>ShowRoom</td>
</tr>
		<tr>
<!--<td></td>-->
	<td>Gaming noise reduction</td>
<td>ChatRoomGaming</td>
</tr>
		<tr>
<!--<td></td>-->
	<td>Stable transmission quality</td>
<td>Default</td>
</tr>
</table>

You can also set the profile and scenario parameters based on different applications.

| Applications | profile | scenario | Features |
| ---------------- | ---------------- | ---------------- | ---------------- |
| One-to-one classroom | Default | Default | Prioritizes the call quality with smooth transmission and high-fidelity audio. |
| Battle royale game | SpeechStandard | ChatRoomGaming | Noise reduction and transmits the voice only. Reduces the transmission rate and suitable for multiplayer games. |
| Murder mystery game | MusicStandard | ChatRoomEntertainment | High-fidelity audio encoding and decoding. No volume or audio quality change when you mute/unmute the microphone. |
| KTV | MusicHighQuality | GameStreaming | High-fidelity audio and effects. Adapts to the high-fidelity audio application. |
| Podcast | MusicHighQualityStereo | ShowRoom | High-fidelity audio and stereo panning. Support for professional audio hardware. |
| Music education | MusicStandardStereo | GameStreaming | Prioritizes audio quality. Suitable for transmitting live external audio effects. |
| Collaborative teaching | MusicStandardStereo | ChatRoomEntertainment | High-fidelity audio and effects. No volume or audio quality change when you mute/unmute the microphone. |

### Sample Code

```objective-c
// swift
// Gaming
agoraKit.setAudioProfile(.speechStandard, scenario: .chatRoomGaming)
// Entertainment
agoraKit.setAudioProfile(.musicStandard, scenario: .chatRoomEntertainment)
// KTV
agoraKit.setAudioProfile(.musicHighQuality, scenario: .chatRoomEntertainment)
// FM high-fidelity
agoraKit.setAudioProfile(.musicHighQuality, scenario: .gameStreaming)
```

```objective-c
// objective-c
// Gaming
[agoraKit setAudioProfile: AgoraAudioProfileSpeechStandard scenario: AgoraAudioScenarioChatRoomGaming];
// Entertainment
[agoraKit setAudioProfile: AgoraAudioProfilemusicStandard, scenario: AgoraAudioScenarioChatRoomEntertainment];
// KTV
[agoraKit setAudioProfile: AgoraAudioProfileMusicHighQuality, scenario: AgoraAudioScenarioChatRoomEntertainment];
// FM high-fidelity
[agoraKit setAudioProfile: AgoraAudioProfilemusicHighQuality, scenario: AgoraAudioScenarioGameStreaming]
```


### API Reference

- [`setAudioProfile`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setAudioProfile:scenario:)

## Considerations

When you set the volume of a device, you can set either the in-call volume or the media volume.
- You can set the in-call volume during an audio or video call.
- You can set the media volume when you play video, games, or background music.

The differences between the two types of volumes are as follows:
- The in-call volume features acoustic echo cancellation, and should always be set above 0.
- The media volume features a higher audio quality, and can be set as 0.

The audio scenarios in the [`setAudioProfile`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setAudioProfile:scenario:) method use different volume settings.

<table>
<tr>
<th> Audio scenario</th>
<th>Volume setting</th>
</tr>
<tr>
<td>GameStreaming</td>
<td>All users use the media volume.</td>
</tr>
	<tr>
<td>Default</td>
<td  rowspan="3">	<li>In the Communication profile, all users use the in-call volume.</li>
	<li>In the Live-broadcast profile, the hosts use the in-call volume, while the audience use the media volume.</li></td>
</tr>
		<tr>
<td>Education</td>
<!--<td></td>-->
</tr>
<tr>
<td>ShowRoom</td>
<!--<td></td>-->
	</tr>
		<tr>
<td>ChatRoomEntertainment</td>
	<td>All users use the in-call volume.</td>
</tr>
	<tr>
<td>ChatRoomGaming</td>
	<td>All users use the in-call volume.</td>
</tr>
</table>

Given the system restrictions, the media volume can be set as 0, but not the in-call volume. Therefore, if you want to adjust the volume to 0, ensure that you set an audio scenario that uses the media volume.
