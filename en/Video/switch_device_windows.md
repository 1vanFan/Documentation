
---
title: Test or Select a Media Device
description: 
platform: Windows
updatedAt: Wed Jul 17 2019 04:23:09 GMT+0800 (CST)
---
# Test or Select a Media Device
## Introduction

To ensure smooth communication, we recommend conducting a media device test before joining a channel to check whether the microphone or camera works properly. This function applies to scenarios that have high-quality requirements, such as online education.

## Implementation

### Recording device test

Call the `startRecordingDeviceTest` method to test whether the local audio recording device, such as the microphone, is working properly.

To conduct the test, the user speaks, and the SDK reports the audio volume information in the `onAudioVolumeIndication` callback. A UID of 0 indicates the local user.

When the test finishes, call the `stopRecordingDeviceTest` method to stop the current test.

```C++
// Initialize the parameter object.
RtcEngineParameters rep(*lpRtcEngine);

// Enumerate all audio devices.
AAudioDeviceManager* lpDeviceManager = new AAudioDeviceManager(lpRtcEngine);
IAudioDeviceCollection *lpRecordingDeviceCollection = (*lpDeviceManager)->enumerateRecordingDevices();

UINT lCount = (UINT) lpRecordingDeviceCollection->getCount();

CString strDeviceName;
CString strDeviceID;

for (UINT nIndex = 0; nIndex < lCount; nIndex++){
    int nRet = lpRecordingDeviceCollection->getDevice(nIndex, strDeviceName, strDeviceID);
	...
}

// Select an audio capture device.
lpDeviceManager->setRecordingDevice(strDeviceID); // device ID chosen

// Implement the audio volume callback.
virtual void onAudioVolumeIndication(const AudioVolumeInfo* speakers, unsigned int speakerNumber, int totalVolume) {
        (void)speakers;
        (void)speakerNumber;
        (void)totalVolume;
    }

// Enable the audio volume callback.
rep.enableAudioVolumeIndication(1000, // Callback interval (ms)
	10 // Smoothness
	);

// Start the audio capture device test.
(*lpDeviceManager)->startRecordingDeviceTest(1000);

// Stop the audio capture device test.
(*lpDeviceManager)->stopRecordingDeviceTest();
```

### Playback device test

Call the `startPlaybackDeviceTest` method to test whether the local audio playback device, such as the speaker, is working properly.

To conduct the test, specify an audio file for playback. If you can hear the audio file, the audio playback device works properly.

When the test finishes, call the `stopPlaybackDeviceTest` method to stop the current test.

```C++
// Initialize the parameter object.
RtcEngineParameters rep(*lpRtcEngine);

AAudioDeviceManager* lpDeviceManager = new AAudioDeviceManager(lpRtcEngine);
IAudioDeviceCollection *lpPlaybackDeviceCollection = (*lpDeviceManager)->enumeratePlaybackDevices();

UINT lCount = (UINT) lpPlaybackDeviceCollection->getCount();

CString strDeviceName;
CString strDeviceID;

for (UINT nIndex = 0; nIndex < lCount; nIndex++){
    int nRet = lpPlaybackDeviceCollection->getDevice(nIndex, strDeviceName, strDeviceID);
	...
}

// Select an audio playback device.
lpDeviceManager->setPlaybackDevice(strDeviceID); // device ID chosen

// Start the audio capture device test, and you will hear the audio from the external device.
// You do not need to call `enableAudioVolumeIndication`, because you can directly hear the audio.

#ifdef UNICODE
	CHAR wdFilePath[MAX_PATH];
	::WideCharToMultiByte(CP_UTF8, 0, filePath, -1, wdFilePath, MAX_PATH, NULL, NULL);
	(*lpDeviceManager)->startPlaybackDeviceTest(wdFilePath);
#else
	(*lpDeviceManager)->startPlaybackDeviceTest(filePath);
#endif

// Stop the audio capture device test.
(*lpDeviceManager)->stopPlaybackDeviceTest();
```

### Video device test

The video device tests check the video capture device and the video rendering device.

After calling the `enableVideo` method, call the `startDeviceTest` method to test whether the local video devices, such as the camera and renderer, are working properly.

To conduct the test, specify a window handle that displays the image. If you can see the local video view, the video devices work properly.

When the test finishes, call the `stopDeviceTest` method to stop the current test.

```C++
// Initialize the parameter object.
RtcEngineParameters rep(*lpRtcEngine);

// Enumerate all video capture devices.
AVideoDeviceManager* lpDeviceManager = new AVideoDeviceManager(lpRtcEngine);
IVideoDeviceCollection *lpVideoDeviceCollection = (*lpDeviceManager)->enumerateVideoDevices();

UINT lCount = (UINT) lpVideoDeviceCollection->getCount();

CString strDeviceName;
CString strDeviceID;

for (UINT nIndex = 0; nIndex < lCount; nIndex++){
    int nRet = lpVideoDeviceCollection->getDevice(nIndex, strDeviceName, strDeviceID);
	...
}

// Select a video capture device.
lpDeviceManager->setDevice(strDeviceID); // device ID chosen

// Start the video capture device test. If it succeeds, you will see a preview of the screen.
(*lpDeviceManager)->startDeviceTest(view); // Pass a window handler to it.

// Stop the video capture device test.
(*lpDeviceManager)->stopDeviceTest();
```


### API Reference

* [`enableAudioVolumeIndication`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a59ae67333fbc61a7002a46c809e2ec4f)
* [`enumerateRecordingDevices`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_audio_device_manager.html#a1ea4f53d60dc91ea83960885f9ab77ee)
* [`setRecordingDevice`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_audio_device_manager.html#a723941355030636cd7d183d53cc7ace7)
* [`enumeratePlaybackDevices`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_audio_device_manager.html#aa13c99d575d89e7ceeeb139be723b18a)
* [`setPlaybackDevice`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_audio_device_manager.html#a1ee23eae83165a27bcbd88d80158b4f1)
* [`startRecordingDeviceTest`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_audio_device_manager.html#a9e732d31f179a90d388998f5b86ebf06)
* [`stopRecordingDeviceTest`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_audio_device_manager.html#a796e7b8a58eb303f18f04e1e9d12a94b)
* [`enumerateVideoDevices`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_video_device_manager.html#aef51744162ec544abf2aaf0488ca062d)
* [`startDeviceTest`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_video_device_manager.html#ac148cafcb191841fd4aa7f5b6166b16d)
* [`stopDeviceTest`](https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_video_device_manager.html#ae3fe9f7ad1ddf4d5cda5e30d14b9d321)

## Considerations

- Ensure that your audio or video device is not being used by any third-party app when you conduct the test.
- After the test finishes, ensure that you call the corresponding stop method to stop the current test before calling the `joinChannel` method.
