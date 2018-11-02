
---
title: Sending Point-to-point Text and Channel Messages from the Client
description: 
platform: macOS
updatedAt: Fri Nov 02 2018 04:02:52 GMT+0000 (UTC)
---
# Sending Point-to-point Text and Channel Messages from the Client
## Section 1: Integration

### Step 1: Download the Agora Signaling SDK.

Download the [Agora Signaling SDK](https://docs.agora.io/en/Agora%20Platform/downloads).

### Step 2: Unpack the downloaded SDK and save the files in the **libs** folder to the corresponding folder of your project.



> The library provided by the Agora Signaling SDK is a FAT image,which includes versions for the 32/64-bit emulator and the 32/64-bit real device.

### Step 3: Add the required framework.

To add the required library:

1.  Choose the current **Target**.

2.  In Xcode, set the search path of the framework to the **Agora SDK lib** folder.

3.  Click **Building Phases** to add the required library.


<img alt="../_images/xcode_4.png" src="https://web-cdn.agora.io/docs-files/en/xcode_4.png" />


1.  Click **Link Binary with Libraries** \> **+** to add the following libraries.


<img alt="Quickstart Guide/ios_project_3.png" src="https://web-cdn.agora.io/docs-files/en/ios_project_3.png"/>


> `AgoraSigKit.framework` is in the **libs** folder of your project. Click **+** \> **Add Other…** to enter the project folder to add the libraries.

<br/>

<img alt="../_images/xcode_5.png" src="https://web-cdn.agora.io/docs-files/en/xcode_5.png" />



### Step 4: Miscellaneous settings.

1.  Choose the current **Target**.

2.  Disable **bitcode**:

    <img alt="../_images/bitcode.png" src="https://web-cdn.agora.io/docs-files/en/bitcode.png" />



### Step 5: Get an App ID and an App Certificate.

1.  For more information see [App ID](../../en/Agora%20Platform/key_signaling.md).

2.  Put your AppID and App Certificate into “Agora-Signaling-Tutorial-macOS-Swift/Agora-Signaling-Tutorial/MainPage/KeyCenter.swift”.


```
let AppId: String = "YOUR APPID"
let AppCertificate: String = "YOUR CERTIFICATE"
```

### Step 6: Generate a token.

Agora recommends generating a token on the server. For more information about generating a token, see: [SignalingToken](../../en/Agora%20Platform/key_signaling.md).

### Step 7: Fill in the developer signature.

Use XCode to open Agora-Signaling-Tutorial.xcodeproj and fill in a valid developer signature.

You are now all set and can call the APIs provided by the Agora Signaling SDK.

## Section 2: Scenario-based Code Snippets

This section provides code snippets related to the following two scenarios:

-   Sending point-to-point messages

-   Sending channel messages


### Sending a Point-to-point Message

#### Sender

```
/* Initialize the SDK. */
let AgoraSignalKit : AgoraAPI = AgoraAPI.getInstanceWithoutMedia(KeyCenter.AppId)
```

```
/* Login to the Agora Signaling system. */
AgoraSignalKit.login2(KeyCenter.AppId, account: account, token: token, uid: 0, deviceID: nil, retry_time_in_s: 60, retry_count: 5)
```

```
/* Set the onLoginSuccess callback. */
AgoraSignalKit.onLoginSuccess = { (uid,fd) -> () in
      /* Your code. */
}
```

```
/* Set the onLoginFailed callback. */
AgoraSignalKit.onLoginFailed = {(ecode) -> () in
      /* Your code. */
}
```

```
/* Send a point-to-point message. */
AgoraSignalKit.messageInstantSend(account, uid: 0, msg: message, msgID: msgID)
```

```
/* Set the onMessageSendSuccess callback. */
AgoraSignalKit.onMessageSendSuccess = { (messageID) -> () in
     /* Your code. */
}
```

```
/* Set the onMessageSendError callback. */
AgoraSignalKit.onMessageSendError = { (messageID, ecode) -> () in
     /* Your code. */
}
```

```
/* Logout of the Agora Signaling system. */
AgoraSignalKit.logout()
```

#### Receiver

```
/* Initialize the Signaling SDK. */
let AgoraSignalKit : AgoraAPI = AgoraAPI.getInstanceWithoutMedia(KeyCenter.AppId)
```

```
/* Login to the Agora Signaling system. */
AgoraSignalKit.login2(KeyCenter.AppId, account: account, token: token, uid: 0, deviceID: nil, retry_time_in_s: 60, retry_count: 5)
```

```
/* Set the onLoginSuccess callback. */
AgoraSignalKit.onLoginSuccess = { (uid,fd) -> () in
      /* Your code. */
}
```

```
/* Set the onLoginFailed callback. */
AgoraSignalKit.onLoginFailed = {(ecode) -> () in
      /* Your code. */
}
```

```
 /* Set the onMessageInstantReceive callback. */
 AgoraSignalKit.onMessageInstantReceive = { (account, uid, msg) -> () in
      /* Your code. */
}
```

```
/* Logout of the Agora Signaling system. */
AgoraSignalKit.logout()
```

### Sending a Channel Message

#### Sender

```
/* Initialize the Signaling SDK. */
let AgoraSignalKit : AgoraAPI = AgoraAPI.getInstanceWithoutMedia(KeyCenter.AppId)
```

```
/* Login to the Agora Signaling system. */
AgoraSignalKit.login2(KeyCenter.AppId, account: account, token: token, uid: 0, deviceID: nil, retry_time_in_s: 60, retry_count: 5)
```

```
/* Set the onLoginSuccess callback. */
AgoraSignalKit.onLoginSuccess = { (uid,fd) -> () in
      /* Your code. */
}
```

```
/* Set the onLoginFailed callback. */
AgoraSignalKit.onLoginFailed = {(ecode) -> () in
      /* Your code. */
}
```

```
/* Join a channel. */
AgoraSignalKit.channelJoin(channelName)
```

```
/* Set the onChannelJoined callback. */
AgoraSignalKit.onChannelJoined = { (channelID) -> () in
     /* Your code. */
}
```

```
/* Set the onChannelJoinFailed callback. */
AgoraSignalKit.onChannelJoinFailed = { (channelID, ecode) -> () in
      /* Your code. */
}
```

```
/* Send a channel message. */
AgoraSignalKit.messageChannelSend(channelName, msg: message, msgID: msgID)
```

```
/* Set the onMessageSendSuccess callback. */
AgoraSignalKit.onMessageSendSuccess = { (messageID) -> () in
      /* Your code. */
}
```

```
/* Set the onMessageSendError callback. */
AgoraSignalKit.onMessageSendError = { (messageID, ecode) -> () in
      /* Your code. */
}
```

```
/* Leave the channel. */
AgoraSignalKit.channelLeave(channelName)
```

```
/* Set the onChannelLeaved callback. */
AgoraSignalKit.onChannelLeaved = { (channelID, ecode) -> () in
      /* Your code. */
}
```

```
/* Logout of the Agora Signaling system. */
AgoraSignalKit.logout()
```

#### Receiver

```
/* Initialize the Signaling SDK. */
let AgoraSignalKit : AgoraAPI = AgoraAPI.getInstanceWithoutMedia(KeyCenter.AppId)
```

```
/* Login to the Agora Signaling system. */
AgoraSignalKit.login2(KeyCenter.AppId, account: account, token: token, uid: 0, deviceID: nil, retry_time_in_s: 60, retry_count: 5)
```

```
/* Set the onLoginSuccess callback. */
AgoraSignalKit.onLoginSuccess = { (uid,fd) -> () in
      /* Your code. */
}
```

```
/* Set the onLoginFailed callback. */
AgoraSignalKit.onLoginFailed = {(ecode) -> () in
      /* Your code. */
}
```

```
/* Join a channel. */
AgoraSignalKit.channelJoin(channelName)
```

```
/* Set the onChannelJoined callback. */
AgoraSignalKit.onChannelJoined = { (channelID) -> () in
     /* Your code. */
}
```

```
/* Set the onChannelJoinFailed callback. */
AgoraSignalKit.onChannelJoinFailed = { (channelID, ecode) -> () in
      /* Your code. */
}
```

```
/* Set the onMessageChannelReceive callback. */
AgoraSignalKit.onMessageChannelReceive = { (channelID, account, uid, msg) -> () in
      /* Your code. */
}
```

```
/* Leave the channel. */
AgoraSignalKit.channelLeave(channelName)
```

```
/* Set the onChannelLeaved callback. */
AgoraSignalKit.onChannelLeaved = { (channelID, ecode) -> () in
      /* Your code. */
}
```

```
/* Leave the Agora Signaling system. */
AgoraSignalKit.logout()
```


