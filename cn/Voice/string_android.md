
---
title: 使用 String 型的用户名
description: 
platform: Android
updatedAt: Wed Aug 07 2019 01:26:27 GMT+0800 (CST)
---
# 使用 String 型的用户名
## 场景描述

很多 App 使用 String 类型的 User account。为降低开发成本，Agora 新增支持 String 型的 User account，方便用户使用 App 账号直接加入 Agora 频道。

Agora 的其他接口仍使用 UID 作为参数。Agora Engine 在 SDK 内部维护一个 String user account 和 UID 的映射表，方便随时通过 String user account 获取 UID 或者通过 UID 获取 String user account。

为保证通信质量，频道内所有用户需使用同一数据类型的用户名，即频道内的所有用户名应同为 Int 型或同为 String 型。

## 实现方法
请确保你已完成环境准备、安装包获取等步骤，详见[集成客户端](../../cn/Voice/android_video.md)。

Agora Native SDK 和 Web SDK 通过不同方法支持 String 型的用户名：

- Native SDK：从 v2.8.0 起，新增使用 User Account 来标识用户在频道中的身份

 - `registerLocalUserAccount`：注册本地用户 User Account
 - `joinChannelWithUserAccount`：使用 User Account 加入频道

- Web SDK：从 v2.5.0 起支持将 join 方法中的 `uid` 设为 Number 或 String 型

其中，String 型的用户名最大不可超过 255 字节，且需要确保其在频道内的唯一性。支持的字符集范围如下：

- 26 个小写英文字母 a-z
- 26 个大写英文字母 A-Z
- 10 个数字 0-9
- 空格
- "!", "#", "$", "%", "&", "(", ")", "+", "-", ":", ";", "<", "=", ".", ">", "?", "@", "[", "]", "^", "_", " {", "}", "|", "~", ","

使用 User Account 加入频道的 API 调用时序图如下所示：

![](https://web-cdn.agora.io/docs-files/1562139189320)

其中：

- 在 `registerLocalUserAccount` 和 `joinChannelWithUserAccount` 方法中，`userAccount` 参数均为必填，不可为 null。
- `registerLocalUserAccount` 为选调。你可以注册后再调用 `joinChannelWithUserAccount` 方法加入频道，也可以不注册直接调用 `joinChannelWithUserAccount` 加入频道。我们建议你调用。提前调用 `registerLocalUserAccount` 可以减少调用 `joinChannelWithUserAccount` 加入频道的时间。
- 对于其他接口，Agora SDK 仍沿用 Int 型的 UID 参数标识用户身份。你可以使用 `getUserInfoByUid` 或 `getUserInfoByUserAccount` 获取对应的 User Account 或 UID，无需自己维护映射表。


### API 参考

- [`registerLocalUserAccount`](https://docs.agora.io/cn/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#aa37ea6307e4d1513c0031084c16c9acb)
- [`joinChannelWithUserAccount`](https://docs.agora.io/cn/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a310dbe072dcaec3892c4817cafd0dd88)
- [`getUserInfoByUid`](https://docs.agora.io/cn/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a9a787b8d0784e196b08f6d0ae26ea19c)
- [`getUserInfoByUserAccount`](https://docs.agora.io/cn/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#afd4119e2d9cc360a2b99eef56f74ae22)
- [`onLocalUserRegistered`](https://docs.agora.io/cn/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#aca1987909703d84c912e2f1e7f64fb0b)
- [`onUserInfoUpdated`](https://docs.agora.io/cn/Voice/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_i_rtc_engine_event_handler.html#aa3e9ead25f7999272d5700c427b2cb3d)

## 示例代码

Agora 提供一个[使用 String 型用户名](https://github.com/AgoraIO/Advanced-Video/tree/master/String-Account)的 Github 示例代码，你可以前往下载和体验。

你也可以参考如下代码片段，在项目中实现使用 String 型的 User account 加入频道：

```java
// Java
private void initializeAgoraEngine() {
  try {
    String appId = getString(R.string.agora_app_id);
    mRtcEngine = RtcEngine.create(getBaseContext(), appId, mRtcEventHandler);
    // 初始化后、加入频道前先注册用户名，可以缩短加入频道的时间
    mRtcEngine.registerLocalUserAccount(appId, mLocal.userAccount);
  } catch (Exception e) {
    Log.e(LOG_TAG, Log.getStackTraceString(e));
    
    throw new RuntimeException("NEED TO check rtc sdk init fatal error\n" + Log.getStackTraceString(e));
  }
}

...
  
private void joinChannel() {
  String token = getString(R.string.agora_access_token);
  if (token.isEmpty()) {
    token = null;
  }
  // 使用注册的用户名加入频道
  mRtcEngine.joinChannelWithUserAccount(token, "stringifiedChannel1", mLocal.userAccount);
}
```

## 开发注意事项

- 同一频道内，Int 型的 User ID 和 String 型的 User account 不可混用。如果你的频道内有不支持 String 型 User account 的 SDK，则只能使用 Int 型的 User ID。
- 如果你将用户名切换至 String 型 User account，请确保所有终端用户同步升级。
- 如果使用 String 型的 User account，请确保你的服务端用户生成 Token 的脚本已升级至最新版本。如果使用 String 型 User account 加入频道，请确保使用该 User account 或其对应的 Int 型 UID 来生成 Token。你可以调用 `getUserInfoByUserAccount` 来获取 User account 所对应的 UID。
- 如果频道中 Native SDK 和 Web SDK 互通，请确保该两者使用的用户名的类型一致。其中，Web SDK 中的 `uid` 可以设为 String 型或 Number 型。
