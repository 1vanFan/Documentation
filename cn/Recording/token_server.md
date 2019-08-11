
---
title: 在服务端生成 Token
description: Guide on how to generate tokens on the server side
platform: C++
updatedAt: Sun Aug 11 2019 02:48:23 GMT+0800 (CST)
---
# 在服务端生成 Token
本页为 Agora Native SDK v2.1+、Agora Web SDK v2.4+ 以及 Agora Recording SDK v2.1+  的用户演示如何使用我们提供的 Demo 快速生成一个伪 Token，并提供 Token 生成相关的 C++ API 参考。

~ef963130-a956-11e9-9e5e-256c0a74561a~

## 快速生成 Token

下面我们以 **sample_builder.cpp** 为例演示 Token 生成的过程：

1. 由于我们的示例代码依赖 **openssl**, 请确保已安装 **openssl** 库。macOS 平台可通过以下命令安装：
     `brew install openssl` 
3. 将 GitHub 仓库同步到本地。
4. 打开 **/cpp/sample/RtcTokenBuilderSample.cpp** 。
5. 用你自己的 App ID、App Certificate 以及 Channel Name 替换实例代码中的伪码。关于如何获取 App ID 和 App Certificate，详见 [校验用户权限](https://docs.agora.io/cn/Agora%20Platform/token?platform=All%20Platforms#app-id)
    - 如果你使用 int 型 uid 加入频道，请注释掉以下代码段：
```C++
  result = SimpleTokenBuilder::buildTokenWithUserAccount(
      appID, appCertificate, channelName, userAccount, UserRole::Role_Attendee,
      privilegeExpiredTs);
 std::cout << "Token With UserAccount:" << result << std::endl;
```
    - 如果你使用 string 型 userAccount 加入频道，请注释掉以下代码段：
```C++
  result = SimpleTokenBuilder::buildTokenWithUid(
      appID, appCertificate, channelName, uid, UserRole::Role_Attendee,
      privilegeExpiredTs);
 std::cout << "Token With Int Uid:" << result << std::endl;
```
5. 打开你的本地终端，cd 进入到 **RtcTokenBuilderSample.cpp** 所在文件夹。
6. 运行指令： 
    `g++ -std=c++0x -O0 -I../../ RtcTokenBuilderSample.cpp -lz -lcrypto -o RtcTokenBuilderSample` 
    *相同文件夹下会生成一个可执行文件 <b>RtcTokenBuilderSample</b>* 。
7. 在你的本地终端运行 `./RtcTokenBuilderSample` 生成 Token。
     *新生成的伪 Token 会在你的本地终端显示。*
		 
> 假设你用的是 macOS 系统并遇到以下提示：fatal error: 'openssl/hmac.h' file not found ，你的 **openssl** 相关环境变量可能设置有误。可以在你的终端通过以下命令行排查问题：
>  `which openssl`
>  `/usr/bin/openssl`
>  `cd /usr/local/include`
>  `ln -s ../opt/openssl/include/openssl`


## API 参考

源码： [../cpp/src/SimpleTokenBuilder.h](https://github.com/AgoraIO/Tools/blob/master/DynamicKey/AgoraDynamicKey/cpp/src/SimpleTokenBuilder.h)

你可以通过调用 **RtcTokenBuilder.h** 提供的公开方法创建自己的 Token 生成器。请注意，**RtcTokenBuilder.h** 既支持 int 型 uid 也支持 string 型 userAccount，请根据需要选择合适的生成方法。


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

该方法支持用 int 型 uid 生成 Token。

| **参数**    | **描述**                                              |
| ---------------- | ------------------------------------------------------------ |
| `appID`          | Agora 为应用程序开发者签发的 App ID。详见 [获取 App ID](https://docs.agora.io/cn/Agora%20Platform/token/#app-id). | 
| `appCertificate` | Agora 为应用程序开发者签发的 App Certificate。启用 App Certificate 后你必须使用 Token 才能加入频道，详见 [开启 App Certificate](https://docs.agora.io/cn/Agora%20Platform/token?platform=All%20Platforms#app-certificate). |
| `channelName`    | 标识通话的频道名称，长度在64字节以内的字符串。以下为支持的字符集范围（共89个字符）: <li>a-z,<li>A-Z,<li>0-9,<li>空格,<li>"!", "#", "$", "%", "&", "(", ")", "+", "-", ":", ";", "<", "=", ".", ">", "?", "@", "[", "]", "^", "*_", " {", "}", "\|", "~", ","。 |
| `uid`            | 用户ID，32位无符号整数。建议设置范围：1到 (2<sup>32</sup>-1)，并保证唯一性。 |
| `role`           | <li>`Role_Attendee = 0`: 已废弃。通信模式的通话方。享有权限与角色 `Role_Publisher` 相同。<li> `Role_Publisher = 1` ：直播模式下的主播（BROADCASTER）。<li>`Role_Subscriber = 2`: (默认) 直播模式下的观众（AUDIENCE）。<li>`Role_Admin = 101` ：已废弃。享有权限与角色 `Role_Publisher` 相同。 |
| `privilegeExpiredTs`      | 时间戳。自 1970 年 1 月 1 日零时起经过的秒数。比如你希望将权限设为 Token 生成后 10 分钟，那么你要在这里把 privilegeExpiredTs 设为当前 timestamp 再加 600 (秒)。如果权限始终不过期，请填 0。 |

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

该方法支持用 string 型 userAccount 生成 Token。

| **参数**    | **描述**                                             |
| ---------------- | ------------------------------------------------------------ |
| `appID`          | Agora 为应用程序开发者签发的 App ID。详见 [获取 App ID](https://docs.agora.io/cn/Agora%20Platform/token?platform=All%20Platforms#app-id). |
| `appCertificate` | Agora 为应用程序开发者签发的 App Certificate。启用 App Certificate 后你必须使用 Token 才能加入频道，详见 [开启 App Certificate](https://docs.agora.io/cn/Agora%20Platform/token/#app-certificate). |
| `channelName`    | 标识通话的频道名称，长度在64字节以内的字符串。以下为支持的字符集范围（共89个字符）: <li>a-z,<li>A-Z,<li>0-9,<li>空格,<li>"!", "#", "$", "%", "&", "(", ")", "+", "-", ":", ";", "<", "=", ".", ">", "?", "@", "[", "]", "^", "*_", " {", "}", "\|", "~", ","。 |
|`userAccount` | 用户 User Account。该参数为必需，最大不超过 255 字节，不可为 null。请确保加入频道的 User Account 的唯一性。 以下为支持的字符集范围（共 89 个字符）：<li>26 个小写英文字母 a-z；<li>26 个大写英文字母 A-Z；<li>10 个数字 0-9；<li>空格；<li>"!", "#", "$", "%", "&", "(", ")", "+", "-", ":", ";", "<", "=", ".", ">", "?", "@", "[", "]", "^", "*_", " {", "}", "\|", "~", ","。 |
| `role`           | <li>`Role_Attendee = 0`: 已废弃。通信模式的通话方。享有权限与角色 `Role_Publisher` 相同。<li> `Role_Publisher = 1` ：直播模式下的主播（BROADCASTER）。<li>`Role_Subscriber = 2`: (默认) 直播模式下的观众（AUDIENCE）。<li>`Role_Admin = 101` ：已废弃。享有权限与角色 `Role_Publisher` 相同。 |
| `privilegeExpiredTs`      | 时间戳。自 1970 年 1 月 1 日零时起经过的秒数。比如你希望将权限设为 Token 生成后 10 分钟，那么你要在这里把 privilegeExpiredTs 设为当前 timestamp 再加 600 (秒)。如果权限始终不过期，请填 0。|
