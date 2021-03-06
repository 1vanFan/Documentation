
---
title: Start a Video Call
description: 
platform: Unity
updatedAt: Fri Sep 18 2020 04:47:05 GMT+0800 (CST)
---
# Start a Video Call
Use this guide to quickly start a basic video call with the Agora SDK for Unity.

## Video tutorial

The following video demonstrates how to build an app that implements the Agora video call from scratch.

<video src="https://web-cdn.agora.io/docs-files/1600344344579"  poster="https://web-cdn.agora.io/docs-files/1600344523125" controls width = 100% height = auto>Your browser does not support the <code>video</code> tag.</video>

## Sample project

We provide an open-source [Hello-Video-Unity-Agora](https://github.com/AgoraIO/Agora-Unity-Quickstart/tree/master/video/Hello-Video-Unity-Agora) sample project on GitHub that implements the basic one-to-one video call. You can also find how to run the sample project in the following links:
- [Run video chat within your Unity application on Android](https://medium.com/agora-io/run-video-chat-within-your-unity-application-android-add6949f6078)
- [Run video chat within your Unity application on iOS](https://medium.com/agora-io/run-video-chat-within-your-unity-application-ios-425db335a325)
- [Run video chat within your Unity application on macOS](https://medium.com/@jake_agora.io/mac-run-video-chat-within-your-unity-application-e001091db62f)
- [Run video chat within your Unity application on Windows](https://medium.com/@jake_agora.io/windows-run-video-chat-within-your-unity-application-f400f9056749)

## Prerequisites

- Unity 2017 or later

- Operating system and IDE requirements:

  | Target Platform | Operating system version | IDE version                 |
  | :-------------- | :----------------------- | :-------------------------- |
  | Android         | Android 4.1 or later     | Android Studio 2.0 or later |
  | iOS             | iOS 8.0 or later         | Xcode 8.0 or later          |
  | macOS           | macOS 10.0 or later      | Xcode 8.0 or later          |
  | Windows         | Windows XP SP3 or later  | Visual Studio 2013 or later |

- A valid [Agora account](https://docs.agora.io/en/Agora%20Platform/sign_in_and_sign_up) and an [App ID](https://docs.agora.io/en/Agora%20Platform/token?platform=All%20Platforms#get-an-app-id)

<div class="alert note">Open the specified ports in <a href="https://docs.agora.io/en/Agora%20Platform/firewall?platform=All%20Platforms#agora-rtc-sdk">Firewall Requirements</a > if your network has a firewall.</div>

## Set up the development environment

In this section, we create a Unity project and integrate the SDK into the project.

### Create a Unity project

Use the following steps or follow the[ Unity official manual](https://docs.unity3d.com/Manual/GettingStarted.html) to build a project from scratch. Skip to [Integrate the SDK](#Integrate) if you have already created a Unity project.

<details>
	<summary><font color="#3ab7f8">Create a Unity Project</font></summary>
	
1. Ensure that you have downloaded and installed **Unity** before the following steps. If not, click <a href="https://store.unity.com/?_ga=2.210473373.969150697.1571977051-1808388573.1570601452#plans-individual">here</a > to download.
	
2. Open **Unity** and click **New**.
	
3. Fill in and select the options in the following fields, and click **Create project**.
	 - Project name
	 - Location
	 - Organization
	 - Template: Choose **3D**
</details>

<a name="Integrate"></a>
### Integrate the SDK

Choose either of the following approaches to integrate the Agora Unity SDK into your project.

**Approach 1: Automatically integrate the SDK with Unity Asset Store**

1. Open **Asset Store** in **Unity**, search for **Agora Video SDK for Unity** and click it.
   ![](https://web-cdn.agora.io/docs-files/1576206914656)
2. Click **Download** to download the SDK.
   ![](https://web-cdn.agora.io/docs-files/1576206963608)
3. When the download completes, the button becomes **Import**. Click **Import** to show the packages of the downloaded SDK.
   ![](https://web-cdn.agora.io/docs-files/1576207004091)
4. All packages of the SDK are chosen by default. Uncheck the packages you do not need and click **Import**.
   ![](https://web-cdn.agora.io/docs-files/1576207151013)

**Approach 2: Manually add the SDK files**

1. Go to [SDK Downloads](https://docs.agora.io/en/Agora%20Platform/downloads), download the Agora SDK for Unity under **Video SDK**, and unzip the downloaded SDK package.
2. Create a **Plugins** folder under the **Assets** folder in your project.
3. Copy the following file or subfolder from the **libs** folder of the downloaded SDK to the corresponding directory of your project.

  | Target Platform | File or subfolder                 | Directory of your project |
  | :-------------- | :-------------------------------- | :------------------------ |
  | Android         | /Android/AgoraRtcEngineKit.plugin | /Assets/Plugins/Android   |
  | iOS             | /iOS/AgoraRtcEngineKit.framework  | /Assets/Plugins/iOS       |
  | macOS           | /macOS/agoraSdkCWrapper.bundle    | /Assets/Plugins/macOS     |
  | Windows         | /x86                              | /Assets/Plugins/x86       |
  | Windows  | /x86_64  | /Assets/Plugins/x86_64            |

   <div class="alert note"><ul><li>Android or iOS developers using Unity Editor for macOS or Windows must also copy the SDK file for macOS or the subfolder for Windows X86_64 to the specified directory.<li>Android developers also need to copy the AndroidManifest.xml and project.properties  from the directory of the sample project (./Assets/Plugins/Android/AgoraRtcEngineKit.plugin) to the directory of the Unity project (./Assets/Plugins/Android). The former is for adding project permissions; the latter is for setting project properties.<li>When integrate the SDK with the version earlier than v3.0.1, iOS developers also need to do the following steps: (You can find the sample code logic in <a href="https://github.com/AgoraIO/Agora-Unity-Quickstart/blob/master/video/Hello-Video-Unity-Agora/Assets/Editor/BL_BuildPostProcess.cs">BL_BuildPostProcess.cs</a>, or copy  this file to directory of your Unity project)<ul><li>Link the following libraries: <ul><li>CoreTelephony.framework<li>VideoToolbox.framework<li>libresolv.tbd<li>libiPhone-lib.a<li>CoreText.framework<li>Metal.framework<li>CoreML.framework<li>Accelerate.framework</li></ul><li>Ask for the following permissios: <ul><li>NSCameraUsageDescription<li>NSMicrophoneUsageDescription</div>

## Implement a basic video call

This section provides instructions on using the Agora Video SDK for Unity to implement a basic one-to-one video call, as well as an API call sequence diagram.

![](https://web-cdn.agora.io/docs-files/1576216531722)

### 1. Create the UI

Create the user interface (UI) for a one-to-one call in your project. If you already have one UI in your project, skip to [Get the device permission (Android only)](#permission) or [Initialize IRtcEngine](#initialize).

We recommend adding the following elements to the UI:

- The local video view
- The remote video view
- An end-call button

If you use the **Unity Editor** to build your UI, ensure that you bind **VideoSurface.cs** to the **GameObjects** designated for local and remote videos. In the following example, the **VideoSurface.cs** is applied to the cube for the local video and to the cylinder for the remote video.

![](https://web-cdn.agora.io/docs-files/1576216681040)

<a name="permission"></a>
### 2. Get the device permission (Android only)

If you build for Android, you should add in APIs to check and request the device permission. For all other platforms, this is handled by the engine, and you can skip to [Initialize IRtcEngine](#initialize).

Unity versions later than **UNITY_2018_3_OR_NEWER** do not automatically request camera and microphone permissions from your Android device. If you are using one of these versions, call the `CheckPermission` method to request access to the camera and microphone of your Android device.

```C#
#if(UNITY_2018_3_OR_NEWER) 
using UnityEngine.Android; 
#endif 
void Start () 
{  
#if(UNITY_2018_3_OR_NEWER) 
permissionList.Add(Permission.Microphone);  
permissionList.Add(Permission.Camera);  
#endif  
} 
private void CheckPermission()
{ 
#if(UNITY_2018_3_OR_NEWER) 
foreach(string permission in permissionList) 
{ 
if (Permission.HasUserAuthorizedPermission(permission)) 
{ 
} 
else 
{  
Permission.RequestUserPermission(permission); 
} 
} 
#endif 
} 
void Update () 
{  
#if(UNITY_2018_3_OR_NEWER) 
// Ask for your Android device's permissions.
CheckPermission(); 
#endif  
}
```

<a name="initialize"></a>
### 3. Initialize IRtcEngine

Initialize the `IRtcEngine` object before calling any other Agora APIs.

Call the `GetEngine` method and pass in the App ID to initialize the IRtcEngine object.

<div class="alert note">If you want to exit the app or release the memory of <tt>IRtcEngine</tt>, call <tt>Destroy</tt> to destroy the <tt>IRtcEngine</tt> object. See details in <a href="#destroy">Destroy the IRtcEngine object</a >.</div>

Listen for callback events, such as when the local user joins the channel, and when the first video frame of a remote user is decoded.

```C#
// Pass an App ID to create and initialize an IRtcEngine object.
mRtcEngine = IRtcEngine.GetEngine (appId); 
// Listen for the OnJoinChannelSuccessHandler callback.
// This callback occurs when the local user successfully joins the channel.
mRtcEngine.OnJoinChannelSuccessHandler = OnJoinChannelSuccessHandler; 
// Listen for the OnUserJoinedHandler callback.
// This callback occurs when the first video frame of a remote user is received and decoded after the remote user successfully joins the channel.
// You can call the SetForUser method in this callback to set the remote video.
mRtcEngine.OnUserJoinedHandler = OnUserJoinedHandler; 
// Listen for the OnUserOfflineHandler callback.
// This callback occurs when the remote user leaves the channel or drops offline.
mRtcEngine.OnUserOfflineHandler = OnUserOfflineHandler;
```


### 4. Set the local video

After initializing an `IRtcEngine` object, set the local video before joining a channel so that you can see yourself in the call. Follow these steps to configure the local video:

- Call `EnableVideo` to enable the video module.

- Call `EnableVideoObserver` to get the local video.

  ```C#
	// Enable the video module.
	mRtcEngine.EnableVideo();
	// Get the local video and pass it on to Unity.
	mRtcEngine.EnableVideoObserver();`
	```
	
- Choose an object to display the local video, and drag the **VideoSurface.cs** file to **Script**, so that you can bind the **VideoSurface.cs** file to the object and see the local video. Unity renders 3D objects by default, such as Cube, Cylinder and Plane. To render the object in other types, modify the renderer in the **VideoSurface.cs** file.
  ![](https://web-cdn.agora.io/docs-files/1576208681884)

  ```C#
    // The default renderer is Renderer.
	Renderer rend = GetComponent(); 
	rend.material.mainTexture = nativeTexture; 
	// Change the renderer to RawImage.
	RawImage rend = GetComponent(); 
	rend.texture = nativeTexture;`
	```


### 5. Join a channel

After initializing an `IRtcEngine` object and setting the local video, call `JoinChannelByKey` to join a channel. Set the following parameters when calling this method::

- `channelKey`: The token for identifying the role and privileges of a user. Set it as one of the following values:

  - `NULL`.
  - A temporary token generated in Agora Console. A temporary token is valid for 24 hours. For details, see [Get a temporary token](https://docs.agora.io/en/Agora%20Platform/token#get-a-temporary-token).
  - A token generated at the server. This applies to scenarios with high-security requirements. For details, see [Generate a token from Your Server](https://docs.agora.io/en/Video/token_server).

  <div class="alert note">If your project has enabled the app certificate, ensure that you provide a token.</div>

- `channelName`: The unique name of the channel to join. Users that input the same channel name join the same channel.

- `uid`: Integer. The unique ID of the local user. If you set `uid` as 0, the SDK automatically assigns one user ID and returns it in the `OnJoinChannelSuccessHandler` callback.

```C#
// Join a channel. 
mRtcEngine.JoinChannelByKey(null, channel, null, 0);
```

### 6. Set the remote video

In a video call, you should be able to see remote users too. Call `SetForUser` in **VideoSurface.cs** after joining a channel.

When you are in a channel and a remote user joins this channel, the SDK triggers the `OnUserJoinedHandler` callback, and you can get the user’s uid.

- In the **VideoSurface.cs** file, call the `SetForUser` method in the callback, and pass in the `uid` to set the video of the remote user.

- Drag the **VideoSurface.cs** script to the target object, so that you can bind the **VideoSurface.cs** to the object and see the remote video.

- The video capture and render frame rate of Agora Unity SDK is 15 fps by default. Call `SetGameFps` in the **VideoSurface.cs** file to adjust the video refresh rate based on your scenario.

  <div class="alert note">The default value of uid in the SetForUser method is 0. To see the remote user, ensure that you input the uid of the remote user in SetForUser. Otherwise, the SDK uses the default value and you can only see the local video.</div>

```C#
// This callback occurs when the first video frame of a remote user is received and decoded after the remote user successfully joins the channel.
// You can call the SetForUser method in this callback to set up the remote video view.
private void OnUserJoinedHandler(uint uid, int elapsed)
	{
		Debug.Log ("OnUserJoinedHandler: uid = " + uid)
		GameObject go = GameObject.Find (uid.ToString ());
		if (!ReferenceEquals (go, null)) {
			return; 
		}
		go = GameObject.CreatePrimitive (PrimitiveType.Plane);
		if (!ReferenceEquals (go, null)) {
			go.name = uid.ToString ();
			VideoSurface remoteVideoSurface = go.AddComponent<VideoSurface> ();
		  // Set the remote video.
		  remoteVideoSurface.SetForUser (uid);
		  // Adjust the video refreshing frame rate. The video capture and render frame rate of Agora Unity SDK is 15 fps by default.
		  // For example, in the game scenario, the refreshing frame rate of the game is 60 fps, which causes 3 times redundancy compared to the video render frame rate. Call the SetGameFps to adjust the refreshing frame rate of the game to 15 fps.
		  remoteVideoSurface.SetGameFps (60);
		  remoteVideoSurface.SetEnable (true);
		}
		mRemotePeer = uid;
	}
```
  
  To remove the **VideoSurface.cs** script from the object, see the following sample codes.

```C#
private void OnUserOfflineHandler(uint uid, USER_OFFLINE_REASON reason)
    {
        // Remove video stream.
        // Call this in the main thread.
        GameObject go = GameObject.Find (uid.ToString());
        if (!ReferenceEquals (go, null)) {
            Destroy (go);
        }
    }
```
  

### 7. Leave the channel

According to your scenario, such as when the call ends and when you need to close the app, call `LeaveChannel` to leave the current call, and call `DisableVideoObserver` to disable the video.

```C#
public void leave()
	{
		Debug.Log ("calling leave");
		if (mRtcEngine == null)
			return;
		// Leave the channel.
		mRtcEngine.LeaveChannel();
		// Disable the video.
		mRtcEngine.DisableVideoObserver();
	}
```

<a name="destroy"></a>
###  8. Destroy the IRtcEngine object

After leaving the channel, if you want to exit the app or release the memory of `IRtcEngine`, call `Destroy` to destroy the `IRtcEngine` object.

```C#
void OnApplicationQuit()
{
	if (mRtcEngine != null)
	{
	// Destroy the IRtcEngine object.
	IRtcEngine.Destroy();
    mRtcEngine = null;
	}
}
```

## Run the project

Run the project in Unity. You should be able to see both the local and remote video if you successfully start a one-to-one video call.
