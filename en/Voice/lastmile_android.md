
---
title: Conduct a Last Mile Test
description: 
platform: Android
updatedAt: Fri Dec 07 2018 16:26:26 GMT+0000 (UTC)
---
# Conduct a Last Mile Test
## Introduction

You can conduct a last mile network quality test before a call to check if the network supports the audio bitrate or target bitrate of the chosen video profile before a user joins a channel. The `onLastmileQuality` callback reports the test results once every two seconds. The test results are based on the network quality ratings determined by the packet-loss rates and network jitter, which reflect the uplink network quality of the client.

> The audio SDK uses a fixed bitrate of 48 Kbps. The video SDK adjusts the actual bitrate according to the chosen video profile.



## Implementation 

Initialize rtcEngine before running the following sample code.

```Java
	// Call the enableLastmileTest function before joining a channel. 
	rtcEngine.enableLastmileTest();

	// The onLastmileQuality callback is in the global IRtcEngineEventHandler.
	public void onLastmileQuality(int quality) {
 		// quality means the detected network quality type. You can use it for related logic. 
		// ⑴ You can choose to end the last mile test in the callback. 
		rtcEngine.disableLastmileTest();
	}

	// ⑵ You can also choose to end the last mile test at any time. Before the test ends, the onLastmileQuality callback can be returned multiple times. 
	rtcEngine.disableLastmileTest();
```

## API Methods

- [RtcEngine.enableLastmileTest()](https://docs.agora.io/en/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a35d045b585649ca89377ed82e9cf0662)
- [RtcEngine.disableLastmileTest()](https://docs.agora.io/en/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a35d045b585649ca89377ed82e9cf0662)
- [IRtcEngineEventHandler.onLastmileQuality()](https://docs.agora.io/en/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#a2887941e3c105c21309bd2643372e7f5)

## Considerations

- You can conduct a last mile test only before joining a channel.
- The `onLastmileQuality` callback may return UNKNOWN the first time it is triggered. Subsequent callbacks will return the test results. 
