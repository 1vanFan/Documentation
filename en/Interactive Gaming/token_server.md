
---
title: Generate a Token from Your Server
description: Guide on how to generate tokens on the server side
platform: Server
updatedAt: Wed Jul 24 2019 01:51:27 GMT+0800 (CST)
---
# Generate a Token from Your Server
This page provides Agora RTC SDK v2.1+, Agora Web SDK v2.4+, and Agora Recording SDK v2.1+ users with  a quick guide on generating a pseudo-token using the **RtcTokenBuilderSample** demos we provide, as well as token-generating API references in C++. 

## An introduction to Agora's token repository

Your token needs to be generated on your own server, hence you are required to first deploy a token generator on the server. In our [GitHub Repository](https://github.com/AgoraIO/Tools/tree/master/DynamicKey/AgoraDynamicKey), we provide source codes and token generator demos in the following programming languages:

- CPP
- Java
- Python
- Ruby
- Node.js
- Go

The **./\<language\>/src** folder of each language holds source codes for generating different types of dynamic keys and tokens. Note that both AccessToken and SimpleTokenBuilder can generate a token for the following SDKs:

- Agora RTC SDK (Java, Objective-C, C++, Electron) v2.1+
- Agora Web SDK v2.4+
- Agora Recording SDK v2.1+ 

However, we recommend using **RtcTokenBuilder** instead of **AccessToken**.  **AccessToken** implements all the core algorithms for generating a token, whilst **RtcTokenBuilder** is a wrapper of **AccessToken** and provides much more simplified Interfaces. 

The **./\<language\>/sample** folder of each language holds token generator demos we create for demonstration purposes.  Built upon **RtcTokenBuilder**, **RtcTokenBuilderSample** is  a demo for generating a token for the Agora RTC SDK, Agora Web SDK, or Agora Recording SDK. You can customize it based on your real business needs. 

## Generate a token using **RtcTokenBuilderSample**

We take **RtcTokenBuilderSample.cpp** as an example:

1. Ensure that you have installed **openssl**. If you are using macOS, try the following command:
    `brew install openssl`
2. Synchronize the GitHub repository to your local drive.
3. Navigate to the **/cpp/sample/** folder and open **RtcTokenBuilderSample.cpp**. 
> Our demo provides pseudo-App ID, appCertificate, channelName, uid, and userAccount for demonstration purposes.
4. Replace the pseudo-App ID, appCertificate, and channelName with your own. For information about getting an App ID and an App certificate, see [Token Security](https://docs.agora.io/en/Agora%20Platform/token?platform=All%20Platforms#app-id).
    - If you use an int uid to join a channel, comment out the following code block:
```C++
  result = SimpleTokenBuilder::buildTokenWithUserAccount(
      appID, appCertificate, channelName, userAccount, UserRole::Role_Attendee,
      privilegeExpiredTs);
 std::cout << "Token With UserAccount:" << result << std::endl;
```    
    - If you use a string userAccount to join a channel, comment out the following code block:
```C++
  result = SimpleTokenBuilder::buildTokenWithUid(
      appID, appCertificate, channelName, uid, UserRole::Role_Attendee,
      privilegeExpiredTs);
 std::cout << "Token With Int Uid:" << result << std::endl;
```
> Skip this step if you just want to take a quick look at how a token is generated.
5. Open your terminal and navigate to the local folder holding **RtcTokenBuilderSample.cpp**.
6. Run the following command:
    `g++ -std=c++0x -O0 -I../../ RtcTokenBuilderSample.cpp -lz -lcrypto -o RtcTokenBuilderSample`
 *An executable file <b>RtcTokenBuilderSample</b> appears in the folder.*
7. In your terminal, run `./RtcTokenBuilderSample` to generate a Token. 
 *Your token is printed in your terminal window.*
		
> Ensure that you have correctly set the environment variant of **openssl**. Suppose you are using macOS and if `fatal error: 'openssl/hmac.h' file not found` appears, try the followling command to debug:
>1. Run the `which openssl` command in your terminal.
>     *The terminal prints: `/usr/bin/openssl`*
>2. `cd /usr/local/include`
>3. `ln -s ../opt/openssl/include/openssl .`   


## API Reference

Source code:  [../cpp/src/SimpleTokenBuilder.h](https://github.com/AgoraIO/Tools/blob/master/DynamicKey/AgoraDynamicKey/cpp/src/SimpleTokenBuilder.h)

You can create your own token generator using the public methods that **RtcTokenBuilder.h** provides. Note that **RtcTokenBuilder.h** supports both int uid and string userAccount. Ensure that you chose the right method. 

### buildTokenWithUid



```c++
   static std::string buildTokenWithUid(
       const std::string& appId,
       const std::string& appCertificate,
       const std::string& channelName,
       uint32_t uid,
       UserRole role,
       uint32_t privilegeExpiredTs = 0);
```

This method build a token with your int uid.

| **Parameter**    | **Description**                                              |
| ---------------- | ------------------------------------------------------------ |
| `appID`          | The App ID issued to you by Agora. Apply for a new App ID from Agora Dashboard if it is missing from your kit. See [Get an App ID](https://docs.agora.io/en/Agora%20Platform/token/#app-id). |
| `appCertificate` | Certificate of the application that you registered in the Agora Dashboard. See [Get an App Certificate](https://docs.agora.io/en/Agora%20Platform/token/#app-certificate). |
| `channelName`    | Unique channel name for the AgoraRTC session in the string format. The string length must be less than 64 bytes. Supported character scopes are: <li>The 26 lowercase English letters: a to z.<li>The 26 uppercase English letters: A to Z.<li>The 10 digits: 0 to 9.<li>The space.<li>"!", "#", "$", "%", "&", "(", ")", "+", "-", ":", ";", "<", "=", ".", ">", "?", "@", "[", "]", "^", "_", " {", "}", "\|", "~", ",". |
| `uid`            | User ID. A 32-bit unsigned integer with a value ranging from 1 to (2<sup>32</sup>-1). optionalUid must be unique. |
| `role`           | `Role_Attendee = 0`: Deprecated. An attendee in the communcation profile. It has the same privilege as `Role_Publisher`.`Role_Publisher = 1`: A broadcaster (host) in a live-broadcast profile.`Role_Subscriber = 2`: (Default) A audience in a live-broadcast profile.`Role_Admin = 101`: Deprecated. It has the same privilege as `Role_Publisher`. |
| `privilegeExpiredTs`      | Time represented by the number of seconds elapsed since 1/1/1970. If, for example, you want to access the Agora Service within 10 minutes after the token is generated, set expireTimestamp as the current timestamp + 600 (seconds). |


### buildTokenWithUserAccount



```c++
  static std::string buildTokenWithUserAccount(
      const std::string& appId,
      const std::string& appCertificate,
      const std::string& channelName,
      const std::string& userAccount,
      UserRole role,
      uint32_t privilegeExpiredTs = 0);
```

This method build a token with your string userAccount.

| **Parameter**    | **Description**                                              |
| ---------------- | ------------------------------------------------------------ |
| `appID`          | The App ID issued to you by Agora. Apply for a new App ID from Agora Dashboard if it is missing from your kit. See [Get an App ID](https://docs.agora.io/en/Agora%20Platform/token/#app-id). |
| `appCertificate` | Certificate of the application that you registered in the Agora Dashboard. See [Get an App Certificate](https://docs.agora.io/en/Agora%20Platform/token/#app-certificate). |
| `channelName`    | Unique channel name for the AgoraRTC session in the string format. The string length must be less than 64 bytes. Supported character scopes are: <li>The 26 lowercase English letters: a to z.<li>The 26 uppercase English letters: A to Z.<li>The 10 digits: 0 to 9.<li>The space.<li>"!", "#", "$", "%", "&", "(", ")", "+", "-", ":", ";", "<", "=", ".", ">", "?", "@", "[", "]", "^", "_", " {", "}", "\|", "~", ",". |
| `userAccount`    | The user account. The maximum length of this parameter is 255 bytes. Ensure that you set this parameter and do not set it as null. Supported character scopes are: <li>The 26 lowercase English letters: a to z.<li>The 26 uppercase English letters: A to Z.<li>The 10 digits: 0 to 9.<li>The space.<li>"!", "#", "$", "%", "&", "(", ")", "+", "-", ":", ";", "<", "=", ".", ">", "?", "@", "[", "]", "^", "_", " {", "}", "\|", "~", ",". |
| `role`           | `Role_Attendee = 0`: Deprecated. An attendee in the communcation profile. It has the same privilege as `Role_Publisher`.`Role_Publisher = 1`: A broadcaster (host) in a live-broadcast profile.`Role_Subscriber = 2`: (Default) A audience in a live-broadcast profile.`Role_Admin = 101`: Deprecated. It has the same privilege as `Role_Publisher`. |
| `privilegeExpiredTs`      | Time represented by the number of seconds elapsed since 1/1/1970. If, for example, you want to access the Agora Service within 10 minutes after the token is generated, set expireTimestamp as the current timestamp + 600 (seconds). |

