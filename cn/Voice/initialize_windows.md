
---
title: 创建 AgoraRtcEngine 实例并初始化
description: windows平台初始化
platform: Windows
updatedAt: Fri Jul 19 2019 08:28:22 GMT+0800 (CST)
---
# 创建 AgoraRtcEngine 实例并初始化
## 前提条件

在创建 AgoraRtcEngine 实例并初始化前，请确保你已完成环境准备、安装包获取等步骤，详见[集成客户端](../../cn/Voice/windows_video.md)。

初始化过程中，你需要传入一个的 App ID。因此需要现在 Agora Dashboard 注册项目并获取 App ID。

1. 进入 [Agora Dashboard](https://dashboard.agora.io/) ，并按照屏幕提示注册账号并登录 Dashboard。详见[创建新账号](../../cn/Voice/sign_in_and_sign_up.md)。
2. 点击 **项目列表** 处的**新手指引**。

	![](https://web-cdn.agora.io/docs-files/1563521764570)

3. 在弹出的窗口中输入你的第一个项目名称，然后点击**创建项目**。你可以参考屏幕提示，了解实现一个视频通话的基本步骤。

	![](https://web-cdn.agora.io/docs-files/1563521821078)

4. 项目创建成功后，你会在**项目列表**下看到刚刚创建的项目。点击项目名后的**编辑**按钮，进入项目页。你也可以直接点击左边栏的**项目管理**图标，进入项目页面。

	![](https://web-cdn.agora.io/docs-files/1563522909895)

5. 在**项目管理**页，你可以查看你的 **App ID**。

	![](https://web-cdn.agora.io/docs-files/1563522556558)


## 实现方法
进入频道之前，调用 <code>create</code> 方法创建一个 AgoraRtcEngine 实例，并调用 <code>initialize</code> 方法完成初始化。

填入获取到的 App ID 。只有 App ID 相同的应用程序才能进入同一个频道进行互通。

```
m_lpAgoraObject = new CAgoraObject();
m_lpAgoraEngine = createAgoraRtcEngine();
RtcEngineContext ctx;

ctx.eventHandler = &m_EngineEventHandler;
ctx.appId = lpVendorKey;

m_lpAgoraEngine->initialize(ctx);
```

> 请确保在调用其他 API 前先调用 `create` 和 `initialize` 方法创建并初始化 AgoraRtcEngine。

## 相关文档
完成创建实例后，你可以使用 Agora SDK，依次实现如下功能进行语音通话：

- [加入频道](../../cn/Voice/join_communication_windows.md)
- [发布和订阅音频流](../../cn/Voice/publish_windows_audio.md)

如果对网络或音质有特殊需求，你还可以在加入频道前：

- [进行通话前网络质量测试](../../cn/Voice/lastmile_windows.md)
- [使用双声道/高音质](../../cn/Voice/audio_profile_windows.md)

