
---
title: 选择加密方案
description: 
platform: Web
updatedAt: Fri Jan 04 2019 10:11:43 GMT+0000 (UTC)
---
# 选择加密方案

## 功能描述
如果你需要启用加密功能，Agora 提供加密方案与设置加密密码方法。

下图描述了启用内置加密方案的声网音视频通信方案：

<img alt="../_images/agora-encryption.png" src="https://web-cdn.agora.io/docs-files/cn/agora-encryption.png" style="width: 840px;"/>


## 实现方法
开始前请确保你已完成环境准备、安装包获取等步骤，详见[集成客户端 ](../../cn/Video/web_prepare.md)。

```javascript
// 设置加密方案
// 在 "aes-128-xts"，"aes-256-xts" 和 "aes-128-ecb" 中选择一种方案
client.setEncryptionMode(encryptionMode);
// 设置 AES 加密密码
client.setEncryptionSecret(password);
```

### API 参考

- [`setEncryptionMode`](https://docs.agora.io/cn/Video/API%20Reference/web/interfaces/agorartc.client.html#setencryptionmode)
- [`setEncryptionSecret`](https://docs.agora.io/cn/Video/API%20Reference/web/interfaces/agorartc.client.html#setencryptionsecret)

## 开发注意事项

请确保在[加入 Agora RTC 频道（`join`）](https://docs.agora.io/cn/Video/API%20Reference/web/interfaces/agorartc.client.html#join)之前调用该方法。


