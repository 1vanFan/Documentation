
---
title: Implementing IM for Gaming
description: 
platform: Unity_(Android)
updatedAt: Fri Nov 30 2018 09:10:48 GMT+0000 (UTC)
---
# Implementing IM for Gaming
With **Hello-IM-Unity-Agora** Demo, you can implement the following functions:

-   Send / receive point-to-point messages

-   Send / receive chatroom messages

-   Send / receive discussion messages

-   Send / receive voice messages


## Step 1: Prepare the Environment

1.  [Download](https://docs.agora.io/en/Agora%20Platform/downloads) the latest Unity IM kit. See the following structure:

    <img alt="../_images/AMG-Video-Unity3D_0.jpeg" src="https://web-cdn.agora.io/docs-files/en/AMG-Video-Unity3D_0.png" style="width: 840.0px;"/>


2.  Hardware and software requirements:

    -   Unity 5.5 or later

    -   Android Studio 2.0 or later

    -   Two or more Android 4.0 or later devices with video and audio functions

    -   Get an App ID. See [Getting an App ID](../../en/Agora%20Platform/token.md)

    -   Get an App Key。Contact [sales@agora.io](mailto:sales@agora.io) for your App Key


## Step 2: Create a Project

1.  Use Unity to open project **Hello-IM-Unity-Agora** .

    <img alt="../_images/AMG-Video-Unity3D_1.png" src="https://web-cdn.agora.io/docs-files/en/AMG-Video-Unity3D_1.png" />


2.  Fill in App ID and App Key.

    Unity shows error. Click on the red exclamation mark in the status bar and open **MonoDevelop**. Locate the error and fill in the App ID and App Key.

	![AMG-IM-Unity_fill_in_id.jpeg](https://agora-web-cdn.oss-cn-beijing.aliyuncs.com/docs-files/1537412581475)


3.  Customize compile settings.

    1.  Select **File\> Build Settings…**.

        <img alt="../_images/AMG-Video-Unity3D_7.png" src="https://web-cdn.agora.io/docs-files/en/AMG-Video-Unity3D_7.png"/>

    2.  In **Build Settings**:

       - Select **Android**;
       
         <img alt="../_images/AMG-Video-Unity3D_8.png" src="https://web-cdn.agora.io/docs-files/en/AMG-Video-Unity3D_8.png" />

      - Select **Gradle\(New\)** for Build System;

      - Select **Export Project** ;

      - Click on **Export** to export the project to a customized folder:

         <img alt="../_images/AMG-Video-Unity3D_9.png" src="https://web-cdn.agora.io/docs-files/en/AMG-Video-Unity3D_9.png"/>

       - Click on **create**.

4.  Open the command terminal, go to the **Android/RollingIM** directory, and run the following commands to compile the sample code:

    ```
    ./gradlew assembleDebug
    adb install build/outputs/apk/Hello Gaming IM Agora-debug.apk
    ```


## Step 3: Run the Application

1.  Run **Hello-Gaming-IM-Agora** on both devices.

2.  Join the same channel. The default name of the channel is **unity3d**.

	![AMG-IM-Unity-channel-name.jpeg](https://agora-web-cdn.oss-cn-beijing.aliyuncs.com/docs-files/1537412440522)


3.  You’ll see the test interface.

	![AMG-IM-Unity-main-scene.jpeg](https://agora-web-cdn.oss-cn-beijing.aliyuncs.com/docs-files/1537412462354)


1.  Send point-to-point messages.

    1.  Fill in the ID of the receiver in **SenderId**;

    2.  Fill in the text to send in **MessageText**;

    3.  Click on **SendPrivate** to send the message.

2.  Send chatroom messages.

    1.  Fill in the text to send in **MessageText**;

    2.  Click on **SendChannel** to send the message to corresponding chatroom.

3.  Send discussion messages.

    1.  Click on **CreateDiscussion** to create and join a discussion;

    2.  Fill in the text to send in **MessageText**;

    3.  Click on **SendDiscussion** to send the message to corresponding discussion.

4.  Send voice messages.

    1.  Click on **StartRecording** to start recording;

    2.  Click on **StopRecording** to stop recording;

    3.  Click on **SendVoice** to send the recorded voice message.

5.  Receive text message。

    The received point-to-point messages, chatroom messages and discussion messages are shown in message.

6.  Receive voice message。

    When you see a received voice message, click on **PlayAudio** to play the voice message.


> If there is no video or voice, check the following:
> 
> -   If the App ID is set correctly.
> 
> -   If the network is in good condition.
> 
> -   If the permissions of network and camera are authorized.



