
---
title: Implementing Voice for Gaming
description: 
platform: iOS_CPP
updatedAt: Fri Nov 02 2018 04:20:25 GMT+0000 (UTC)
---
# Implementing Voice for Gaming
In this quickstart, you will learn how to use the Interactive Gaming Voice C++ SDK to make a voice call on Android platform.

## Prerequisites

1.  See [Setting up Your Development Environment](https://docs.agora.io/en/Voice/ios_audio?platform=iOS) for the prerequisites, environment setup and SDK integration.

2.  When you use the SDK, you need to *add SDK* and *add Framework Search Paths*:

-   Add SDK:

     -   Download the [Interactive Gaming Audio Only SDK](https://docs.agora.io/en/Agora%20Platform/downloads).
     -   Add the `AgoraAudioKit.framework` file into your build configuration file.
     -   Add Agora framework. The Agora framwork has the following library dependencies:

      * `AgoraAudioKit.framework`
      * `Accelerate.framework`
      * `SystemConfiguration.framework`
      * `libc++.tbd`
      * `libredolv.tbd`
      * `CoreMedia.framework`
      * `AudioToolbox.framework`
      * `CoreTelephony.framework`
      * `AVFoundation.framework`
      * `CFNetwork.framework`
  
-   Add Framework Search Paths:

3.  Import the iOS C++ SDK into the build configuration file:

  
## Quickstart

1.  Create an IRtcEngine object and fill in your App ID.

	```
	m_lpAgoraEngine = (IRtcEngine *)createAgoraRtcEngine();
	RtcEngineContext ctx;

	// inherit the event handler m_EngineEventHandler from IRtcEngineEventHandler
	ctx.eventHandler = &m_EngineEventHandler;

	// Fill in your App ID
	ctx.appId = APP_ID;

	m_lpAgoraEngine->initialize(ctx);
	```

2.  Create and join a channel.

	```
	m_lpAgoraEngine->joinChannel(NULL, "Your Cahnnel Name", NULL, 0);
	```

You can now start 1 on 1 chat or group chat! The only difference is that the number of people joining the same channel.


