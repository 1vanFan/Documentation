
---
title: 在服务端生成 Token
description: 
platform: Python
updatedAt: Sun Aug 11 2019 03:19:29 GMT+0800 (CST)
---
# 在服务端生成 Token
本页为 Agora Native SDK v2.1+、Agora Web SDK v2.4+ 以及 Agora Recording SDK v2.1+  的用户演示如何使用我们提供的 Demo 快速生成一个伪 Token，并提供 Token 生成相关的 Python API 参考。

## Token 代码仓库说明

你需要在业务服务器自行部署 Token 生成器生成 Token。我们的 [Github 开源仓库](https://github.com/AgoraIO/Tools/tree/master/DynamicKey/AgoraDynamicKey) 为你提供了 Token 生成源码以及使用这些源码生成 Token 的简单示例。我们目前支持以下几种语言：

- CPP
- Java
- Python
- Ruby
- Node.js
- Go

[GitHub 开源仓库](https://github.com/AgoraIO/Tools/tree/master/DynamicKey/AgoraDynamicKey) 的 **./\<language\>/src** 文件夹下包含生成各种版本的 Dynamic key 和 Token 的源码。其中：**AccessToken** 和 **RtcTokenBuilder** 用于为以下 SDK 生成 Token：

- Agora RTC SDK (Java, Objective-C, C++, Electron) v2.1+
- Agora Web SDK v2.4+
- Agora Recording SDK v2.1+ 
- Agora RTSA SDK

> Ruby 语言的源码在 **./ruby/lib** 文件夹下。

我们推荐使用 **RtcTokenBuilder** 而不是 **AccessToken** 生成 Token。**AccessToken** 实现了底层的核心算法，**RtcTokenBuilder** 实际上对 **AccessToken** 又进行了一层封装，提供了更为简化易懂的 Token 生成接口。

开源仓库的 **./\<language\>/sample** 文件夹下包含用于演示 Token 生成的示例代码。其中， **RtcTokenBuilderSample** 是我们基于 **RtcTokenBuilder** 编写的一个简单的 Token 生成器示例程序。你可以根据自己的业务逻辑对我们的示例程序做相应调整。

## API 参考

源码： [../python/src/RtcTokenBuilder.py](https://github.com/AgoraIO/Tools/blob/master/DynamicKey/AgoraDynamicKey/python/src/RtcTokenBuilder.py)

你可以通过调用 **RtcTokenBuilder.h** 提供的公开方法创建自己的 Token 生成器。请注意，**RtcTokenBuilder.h** 既支持 int 型 uid 也支持 string 型 userAccount，请根据需要选择合适的生成方法。


### buildTokenWithUid

```Python
   def buildTokenWithUid(appId, appCertificate, channelName, uid, role, privilegeExpiredTs):
```

该方法支持用 int 型 uid 生成 Token。

| **参数**    | **描述**                                              |
| ---------------- | ------------------------------------------------------------ |
| `appID`          | Agora 为应用程序开发者签发的 App ID。详见 [获取 App ID](https://docs.agora.io/cn/Agora%20Platform/token/#app-id). | 
| `appCertificate` | Agora 为应用程序开发者签发的 App Certificate。启用 App Certificate 后你必须使用 Token 才能加入频道，详见 [开启 App Certificate](https://docs.agora.io/cn/Agora%20Platform/token?platform=All%20Platforms#app-certificate). |
| `channelName`    | 标识通话的频道名称，长度在64字节以内的字符串。以下为支持的字符集范围（共89个字符）: <li>a-z,<li>A-Z,<li>0-9,<li>空格,<li>"!", "#", "$", "%", "&", "(", ")", "+", "-", ":", ";", "<", "=", ".", ">", "?", "@", "[", "]", "^", "*_", " {", "}", "\|", "~", ","。 |
| `uid`            | 用户ID，32位无符号整数。建议设置范围：1到 (2<sup>32</sup>-1)，并保证唯一性。 |
| `role`           | <li> `Role_Publisher = 1` ：（推荐）直播模式下的主播（BROADCASTER）。<li>`Role_Subscriber = 2`：直播模式下的观众（AUDIENCE）。 |
| `privilegeExpiredTs`      | 时间戳。自 1970 年 1 月 1 日零时起经过的秒数。比如你希望将权限设为 Token 生成后 10 分钟，那么你要在这里把 privilegeExpiredTs 设为当前 timestamp 再加 600 (秒)。如果权限始终不过期，请填 0。 |

### buildTokenWithUserAccount

```Python
  def buildTokenWithAccount(appId, appCertificate, channelName, account, role, privilegeExpiredTs):
```

该方法支持用 string 型 userAccount 生成 Token。

| **参数**    | **描述**                                             |
| ---------------- | ------------------------------------------------------------ |
| `appID`          | Agora 为应用程序开发者签发的 App ID。详见 [获取 App ID](https://docs.agora.io/cn/Agora%20Platform/token?platform=All%20Platforms#app-id). |
| `appCertificate` | Agora 为应用程序开发者签发的 App Certificate。启用 App Certificate 后你必须使用 Token 才能加入频道，详见 [开启 App Certificate](https://docs.agora.io/cn/Agora%20Platform/token/#app-certificate). |
| `channelName`    | 标识通话的频道名称，长度在64字节以内的字符串。以下为支持的字符集范围（共89个字符）: <li>a-z,<li>A-Z,<li>0-9,<li>空格,<li>"!", "#", "$", "%", "&", "(", ")", "+", "-", ":", ";", "<", "=", ".", ">", "?", "@", "[", "]", "^", "*_", " {", "}", "\|", "~", ","。 |
|`userAccount` | 用户 User Account。该参数为必需，最大不超过 255 字节，不可为 null。请确保加入频道的 User Account 的唯一性。 以下为支持的字符集范围（共 89 个字符）：<li>26 个小写英文字母 a-z；<li>26 个大写英文字母 A-Z；<li>10 个数字 0-9；<li>空格；<li>"!", "#", "$", "%", "&", "(", ")", "+", "-", ":", ";", "<", "=", ".", ">", "?", "@", "[", "]", "^", "*_", " {", "}", "\|", "~", ","。 |
| `role`           | <li> `Role_Publisher = 1` ：（推荐）直播模式下的主播（BROADCASTER）。<li>`Role_Subscriber = 2`: 直播模式下的观众（AUDIENCE）。|
| `privilegeExpiredTs`      | 时间戳。自 1970 年 1 月 1 日零时起经过的秒数。比如你希望将权限设为 Token 生成后 10 分钟，那么你要在这里把 privilegeExpiredTs 设为当前 timestamp 再加 600 (秒)。如果权限始终不过期，请填 0。|
