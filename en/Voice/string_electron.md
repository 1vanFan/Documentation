
---
title: Use String User Accounts
description: 
platform: Electron
updatedAt: Tue Jul 09 2019 03:42:06 GMT+0800 (CST)
---
# Use String User Accounts
## Introduction

Many apps use string usernames. To reduce development costs, Agora adds support for string user accounts. Users can now directly use their string usernames as user accounts to join the Agora channel.

For other APIs, Agora uses the integer user ID for identification. The Agora engine maintains a mapping table object that contains the string user account and integer user ID. You can get the user ID by passing in the user account, and vice versa.

To ensure smooth communication, all the users in a channel should use the same type of user account, that is, either the integer user ID, or the string user account.

## Implementation

Ensure that you prepare the development environment. See [Integrate the SDK](../../en/Voice/electron_video.md).

The Agora Native SDK and Web SDK support using string user accounts in different ways:

- The Native SDK: Starting with v2.8.0, you can use user accounts to identify the user.
  - `registerLocalUserAccount`: Registers a user account.
  - `joinChannelWithUserAccount`: Joins the channel with the registered user account.
- The Web SDK: Starting with v2.5.0, you can set the `uid` parameter in the `Client.join` method as either a number or a string.

The maximum string length of the user account is 255 bytes. Each user account should be unique in the channel. Supported character scopes are:

- The 26 lowercase English letters: a to z.
- The 26 uppercase English letters: A to Z.
- The 10 numbers: 0 to 9.
- The space.
- "!", "#", "$", "%", "&", "(", ")", "+", "-", ":", ";", "<", "=", ".", ">", "?", "@", "[", "]", "^", "_", " {", "}", "|", "~", ",".

The following diagram shows how to join a channel with a string user account:

![](https://web-cdn.agora.io/docs-files/1562139189320)

**Note**:

- The `userAccount` parameter in the `registerLocalUserAccount` and `joinChannelWithUserAccount` methods is mandatory. Do not set it as null.

- The `registerLocalUserAccount` method is optional. To join a channel with a user account, you can choose either of the following:

  - Call the `registerLocalUserAccount` method to create a user account, and then the `joinChannelWithUserAccount` method to join the channel.
  - Call the `joinChannelWithUserAccount` method to join the channel.

  The difference between the two is that for the former, the time elapsed between calling the `joinChannelWithUserAccount` method and joining the channel is shorter than the latter.

- For other APIs, Agora uses the integer user ID for identification. You can call the  `getUserInfoByUid` or `getUserInfoByUserAccount` method to get the corresponding user ID or user account without maintaining the map.

## API Reference
- [`registerLocalUserAccount`](https://docs.agora.io/en/Voice/API%20Reference/electron/v2.8.0/classes/agorartcengine.html#registerlocaluseraccount)
- [`joinChannelWithUserAccount`](https://docs.agora.io/en/Voice/API%20Reference/electron/v2.8.0/classes/agorartcengine.html#joinchannelwithuseraccount)
- [`getUserInfoByUid`](https://docs.agora.io/en/Voice/API%20Reference/electron/v2.8.0/classes/agorartcengine.html#getuserinfobyuid)
- [`getUserInfoByUserAccount`](https://docs.agora.io/en/Voice/API%20Reference/electron/v2.8.0/classes/agorartcengine.html#getuserinfobyuseraccount)
- `localUserRegistered`
- `userInfoUpdated`

## Considerations
- Do not mix parameter types within the same channel. If you use SDKs that do not support string usernames, only integer user IDs can be used in the channel.
- If you change your app usernames into string user accounts, ensure that all app clients are upgraded to the latest version.
- If you use string user accounts, ensure that the token generation script on your server is updated to the latest version. If you join the channel with a user account, ensure that you use the same user account or its corresponding integer user ID to generate a token. Call the `getUserInfoByUserAccount` method to get the user ID that corresponds to the user account.
- If the Native SDK and Web SDK join the same channel, ensure that the user identification types are the same. For the Web SDK, the `uid` parameter can be set either as a number or as a string.
