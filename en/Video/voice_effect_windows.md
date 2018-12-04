
---
title: Adjust the Pitch and Tone
description: How to adjust pitch and tone on Windows
platform: Windows
updatedAt: Fri Nov 23 2018 08:56:02 GMT+0000 (UTC)
---
# Adjust the Pitch and Tone
## Feature Description 

In social and entertainment scenarios, users often need various voice effects to enhance the interactive experiences. Agora provides methods to flexibly change the users' voice, such as adjusting the pitch and setting the equalization and reverberation modes.

## Implementation
Before proceeding, ensure that you have prepared the development environment. See [Integrate the SDK](../../en/Video/windows_video.md) for more information.

The sample code shows how to change the original voice to Hulk's voice.

```c++
// Initialization
RtcEngineParameters rep(*lpAgoraEngine);

// Set the pitch. The value range is [0.5, 2.0]. The lower the value, the lower the pitch. The default value is 1.0, which is the original pitch.
int nRet = rep.setLocalVoicePitch(0.5);

// Set the local voice equalization
// The first parameter sets the band frequency. The value range is [0, 9], each value representing the center frequency of the band: [31, 62, 125, 250, 500, 1k, 2k, 4k, 8k, 16k] Hz
// The second parameter set the gain of each band. The value range is [-15,15], in dB, and the default is 0.
nRet = rep.setLocalVoiceEqualization(AUDIO_EQUALIZATION_BAND_31, -15);
nRet = rep.setLocalVoiceEqualization(AUDIO_EQUALIZATION_BAND_62, 3);
nRet = rep.setLocalVoiceEqualization(AUDIO_EQUALIZATION_BAND_125, -9);
nRet = rep.setLocalVoiceEqualization(AUDIO_EQUALIZATION_BAND_250, -8);
nRet = rep.setLocalVoiceEqualization(AUDIO_EQUALIZATION_BAND_500, -6);
nRet = rep.setLocalVoiceEqualization(AUDIO_EQUALIZATION_BAND_1K, -4);
nRet = rep.setLocalVoiceEqualization(AUDIO_EQUALIZATION_BAND_2K, -3);
nRet = rep.setLocalVoiceEqualization(AUDIO_EQUALIZATION_BAND_4K, -2);
nRet = rep.setLocalVoiceEqualization(AUDIO_EQUALIZATION_BAND_8K, -1);
nRet = rep.setLocalVoiceEqualization(AUDIO_EQUALIZATION_BAND_16K, 1);

// The level of the dry signal in dB. The value ranges between -20 and 10.
nRet = rep.setLocalVoiceReverb(AUDIO_REVERB_DRY_LEVEL, 10);

// The level of the early reflection signal (wet signal) in dB. The value ranges between -20 and 10.
nRet = rep.setLocalVoiceReverb(AUDIO_REVERB_WET_LEVEL, 7);

// The room size of the reverberation. A larger romm size means a stronger reverberation. The value ranges between 0 and 100.
nRet = rep.setLocalVoiceReverb(AUDIO_REVERB_ROOM_SIZE, 6);

// The length of the initial delay of the wet signal (ms). The value range between 0 and 200.
nRet = rep.setLocalVoiceReverb(AUDIO_REVERB_WET_DELAY, 124);

// The reverberation strength. The value ranges between 0 and 100. The higher the value, the stronger the reverberation.
nRet = rep.setLocalVoiceReverb(AUDIO_REVERB_STRENGTH, 78);
```

### API References

- [setLocalVoicePitch](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a1fef48b6aa3954d7e76164a43d660b94)
- [setLocalVoiceEqualization](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a3de79ba906e6b254b997eda4d395d052)
- [setLocalVoiceReverb](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#aa00e903b1cc6f2752373afbe556ef456)

## Considerations

The API methods have return values. If the method fails, the return value is < 0.
