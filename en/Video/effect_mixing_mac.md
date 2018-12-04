
---
title: Play Audio Effects/Audio Mixing
description: How to play audio effects and enable audio mixing for iOS
platform: macOS
updatedAt: Tue Dec 04 2018 20:34:45 GMT+0000 (UTC)
---
# Play Audio Effects/Audio Mixing
## Feature Description
In a call or live broadcast, you may need to play custom audio or music files to all users in the channel. For example, adding sound effects in a game, or playing background music. Agora provides two groups of methods for playing audio effect files and audio mixing.

Before proceeding, ensure that you have prepared the development environment. See [Integrate the SDK](../../en/Video/mac_video.md) for more information.
## Play Audio Effect Files

The play audio effect methods can be used to play audio effects, such as clapping and gunshots. You can play multiple audio effects at the same time, and preload the audio effect file for efficiency.

The audio effect file is specified by the file path, but the SDK uses the sound ID to identify the audio effect file. The SDK does not have the rule to define the sound ID. You need to ensure that each audio effect file has a unique sound ID. Common practices include automatically incrementing the ID, and using the hashCode of the audio effect file.

### Implementation

```swift
// swift
// Preloads the audio effect (recommended). Note the file size and preload the file before joining the channel.
// Only mp3, aac, m4a, 3gp, and wav files are supported.
// You may need to record the correlation between the sound IDs and the file paths.
let soundId = 1
let filePath = "your filepath"

// You can preload multiple audio effects files.
agoraKit.preloadEffect(soundId, filePath: filePath)

// Plays an audio effect file.
let soundId = 1 // The sound ID of the audio effect file to be played.
let filePath = "your filepath" // The file path of the audio effect file.
let loopCount = 1 // The number of playback loops. -1 means an infinite loop.
let pitch = 1 // Sets the pitch of the audio effect.
let pan = 1 // Sets the spatial position of the audio effect. 0 means the effect shows ahead.
let gain = 0 // Sets the volume. The value ranges between 0 and 100. 100 is the original volume.
let publish = true // Sets whether to publish the audio effect.
agoraKit.playEffect(Int32(soundId), filePath: filePath, loopCount: Int32(loopCount), pitch: pitch, pan: pan, gain: gain, publish: publish)

// Pauses all audio effects.
agoraKit.pauseAllEffects()

// Gets the volume of the audio effect. The value ranges between 0 and 100.
let volume = agoraKit.getEffectsVolume()

// Ensures that the audio effect's volume is at least 80% of the original volume.
volume = volume < 80 ? 80 : volume
agoraKit.setEffectsVolume(volume)

// Resumes playing the audio effect.
agoraKit.resumeAllEffects()

// Stops playing all audio effects.
agoraKit.stopAllEffects()
```

```objective-c
// objective-c
// Preloads the audio effect (recommended). Note the file size and preload the file before joining the channel.
// Only mp3, aac, m4a, 3gp, and wav files are supported.
// You may need to record the correlation between the sound IDs and the file paths.
int soundId = 1
NSString *filePath = "your filepath"

// You can preload multiple audio effects.
[agoraKit preloadEffect: soundId filePath: filePath];

// Plays an audio effect file.
int soundId = 1; // The sound ID of the audio effect file to be played.
NSString *filePath = "your filepath"; // The file path of the audio effect file.
int loopCount = 1 // The number of playback loops. -1 means an infinite loop.
double pitch = 1 // Sets the pitch of the audio effect.
double pan = 1 // Sets the spatial position of the audio effect. 0 means the effect shows ahead.
double gain = 0 // Sets the volume. The value ranges between 0 and 100. 100 is the original volume.
BOOL publish = true // Sets whether to publish the audio effect.
[agoraKit playEffect: soundId filePath: filePath loopCount: loopCount, pitch: pitch, pan: pan, gain: gain, publish: publish];

// Pauses all audio effects.
[agoraKit pauseAllEffects];

// Gets the volume of the audio effect. The value ranges between 0 and 100.
int volume = [agoraKit getEffectsVolume];

// Ensures that the audio effect's volume is at least 80% of the original volume.
volume = volume < 80 ? 80 : volume
[agoraKit setEffectsVolume: volume];

// Resumes playing the audio effect.
[agoraKit resumeAllEffects];

// Stops playing all audio effects.
[agoraKit stopAllEffects];
```

### API Methods

- [playEffect](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/playEffect:filePath:loopCount:pitch:pan:gain:)
- [preloadEffect](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/preloadEffect:filePath:)
- [pauseAllEffects](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/pauseAllEffects)
- [getEffectsVolume](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/getEffectsVolume)
- [setEffectsVolume](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setEffectsVolume:)
- [resumeAllEffects](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/resumeAllEffects)
- [stopAllEffects](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/stopAllEffects)

### Considerations

- Preloading the audio effect is not mandatory. Agora recommends you preload the audio effect to improve the efficiency or to play the audio effect multiple times.
- The API methods have return values. If the method fails, the return value is < 0.

## Audio Mixing

Audio mixing is playing a local or online music file while speaking, so that other users in the channel can hear the music. The audio mixing methods can be used to play background music. For example, playing music in a live broadcast. Only one music file can be played at one time. If you start playing a second music file during audio mixing, the first music file stops playing.
Agora audio mixing supports the following options:

- Mix or replace the audio: 
	- Mix the music file with the audio captured by the microphone and send it to other users.
	- Replace the audio captured by the microphone with the music file.
- Loop: Sets whether to loop the audio mixing file and the number of times to play the file.

### Implementation

```swift
// swift
// loopback sets whether other users can hear the audio mixing. If loopback is set as true, only the local user can hear the audio mixing.
// replace sets whether the audio captured by the microphone is replaced by the audio mixing file. 
// Setting cycle as -1 means looping the audio mixing file infinitely. Setting cycle as a positive integer means the number of times to play the file.
let filePath = "http://www.hochmuth.com/mp3/Haydn_Cello_Concerto_D-1.mp3"
let loopback = false
let replace = false 
let cycle = 1 
  
// Starts audio mixing.
agoraKit.startAudioMixing(filePath, loopback: loopback, replace: replace, cycle: cycle)
```

```objective-c
// objective-c
// loopback sets whether other users can hear the audio mixing. If loopback is set as YES, only the local user can hear the audio mixing.
// replace sets whether the audio captured by the microphone is replaced by the audio mixing file. 
// Setting cycle as -1 means looping the audio mixing file infinitely. Setting cycle as a positive integer means the number of times to play the file.
NSString *filePath = @"http://www.hochmuth.com/mp3/Haydn_Cello_Concerto_D-1.mp3";
BOOL loopback = NO;
BOOL replace = NO;
NSInteger cycle = 1;

// Starts audio mixing.
[agoraKit startAudioMixing: filePath loopback: loopback replace: replace cycle: cycle];
```



### API Methods

- [startAudioMixing](https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/startAudioMixing:loopback:replace:cycle:)

### Considerations

The API methods have return values. If the method fails, the return value is < 0.
