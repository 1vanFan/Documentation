
---
title: 命令行录制
description: How to start recording using cmd
platform: Linux CPP
updatedAt: Mon Feb 10 2020 04:44:43 GMT+0800 (CST)
---
# 命令行录制
本文介绍如何通过命令行进行录制。 你也可以[调用 API](../../cn/Recording/recording_api_cpp.md) 进行录制。命令行录制和调用 API 录制实现的是相同的功能，根据自己的需要选择一种即可。

Agora 录制 SDK 提供一个 recorder demo，直接在命令行中输入命令运行 demo 就可以开始录制。

开始前请确保你已经完成录制 SDK 的环境准备和集成工作，详见[集成客户端](../../cn/Recording/recording_integrate_cpp.md)。

> 当录制 SDK 加入频道时，相当于一个哑客户端加入频道，因此需要跟 Agora Native/Web SDK 加入相同的频道，并使用相同的 App ID 和频道模式。

## 编译代码示例

打开命令行工具，到 SDK 包的 **samples/cpp** 的目录下执行以下命令进行编译。

```
make
```

编译成功后，在该目录下会生成一个 `recorder_local` 可执行程序，如图所示。

![img](https://web-cdn.agora.io/docs-files/1544522109941)

## 开始录制

在命令行中输入 `./recorder_local` 加上必要的参数设置，即可快速开始录制，例如：

```bash
./recorder_local --appId <你的 App ID> --channel <频道名> --uid 0 --channelProfile <0 通信模式，1 直播模式> --appliteDir Agora_Recording_SDK_for_Linux_FULL/bin
```

其中：

- `appId`，`channel` 和 `channelProfile` 的设置必须与 Agora Native/Web SDK 一致。
- `appliteDir` 必须设置为 `AgoraCoreServices` 存放的路径，SDK 包内该文件位于 **bin** 文件夹下。

## 设置录制选项

除上面示例中的参数外，录制 demo 还提供很多参数设置选项，在 **samples/cpp** 目录下执行 `./recorder_local` 命令，即可看到录制 demo 全部的参数和选项，你可以参考下表根据需要自行设置。

> `appID`，`uid`，`channel` 和 `appliteDir` 这几个参数必须设置，如不设置则无法录制。

### 必填参数

- `appId`：App ID，必须和 Agora Native/Web SDK 的 App ID 一致，详见[获取 App ID](../../cn/Recording/token.md)。
- `uid`：标识录制实例的用户 ID，32 位无符号整数。建议设置范围：1 到（232-1），并保证唯一性。有两种设置方式：
  - 设置为 0，系统将自动分配一个用户 ID。
  - 设置一个唯一的用户 ID（不能与频道内的任何 UID 重复）。
- `channel`：希望录制的通话或直播的频道名。
- `appliteDir`：必须设置为 `AgoraCoreServices` 存放的目录，SDK 包内该文件路径为：Agora_Recording_SDK_for_Linux_FULL/bin/。

以下参数不是必填，但是要根据实际情况确认是否需要设置：

- `channelKey`：动态密钥。如果待录制频道设置了 Token，该参数必须设置。
- `channelProfile`：频道模式，必须和待录制频道使用相同的频道模式，否则可能导致问题。
  - 0：（默认）通信模式
  - 1：直播模式
- `decryptionMode`：频道加密时，录制 SDK 可以启用内置的解密功能。解密方式必须与频道设置的加密方式一致。目前支持以下几种解密方式：
  - aes-128-xts：128 位 AES 加密，XTS 模式。
  - aes-128-ecb：128 位 AES 加密，ECB 模式。
  - aes-256-xts：256 位 AES 加密，XTS 模式。
- `secret`：启用解密模式后，设置的解密密码。

### 设置端口范围

我们建议指定录制进程使用端口的范围。你可以为多个录制进程统一配置较大的端口范围（建议 40000 - 41000 或更大）。此时，录制 SDK 会在指定范围内为每个录制进程分配端口，并避免端口的冲突。

> 如果不指定参数 `lowUdpPort` 和 `highUdpPort` ，录制进程所使用的端口为随机端口，会有端口冲突的风险。

- `lowUdpPort`：最低 UDP 端口。所设置的端口必须为正整数，且最高 UDP 端口与最低 UDP 端口差值不能小于 6。
- `highUdpPort`：最高 UDP 端口。所设置的端口必须为正整数，且最高 UDP 端口与最低 UDP 端口差值不能小于 6。

### 控制录制进程

- `triggerMode`：选择录制启动模式。
  - 0：（默认）自动模式，加入频道自动开始录制，离开频道自动停止录制。
  - 1：手动模式，手动输入命令开始和结束录制。详见 [FAQ](https://docs.agora.io/cn/faq/cmd_control_record)。
- `idle`：设置空闲频道超时退出时间，单位为秒，最小值为 3 秒，默认值为 300 秒。如果频道空闲的状态持续超过该时间，录制程序会自动退出。idle 的时间也会纳入计费。频道空闲包括以下情况：
  - 通信模式下频道内没有 Native SDK 的用户，并且没有 Web SDK 的用户发布流。
  - 直播模式下频道内没有主播。

### 选择录制内容

- `isAudioOnly`：是否仅录制音频。
  - 0：（默认）音视频同时录制。
  - 1：仅启用音频录制功能，关闭视频录制。
- `isVideoOnly`：是否仅录制视频。
  - 0：（默认）音视频同时录制。
  - 1：仅启用视频录制功能，关闭音频录制。
- `streamType`：设置录制的视频流类型，只有在待录制频道开启了双流模式时该设置才会生效。
  - 0：（默认）录制视频大流。
  - 1：录制视频小流。

> `isAudioOnly` 和 `isVideoOnly` 不能同时设置为 1。

### 设置录制格式

- `getAudioFrame`：录制音频格式。该参数设为 1，2 或 3 （即录制原始音频数据）时，不可将 `isMixingEnabled` 设为 1。
  - 0：默认音频文件格式。
  - 1：原始音频数据 AAC 帧格式。
  - 2：原始音频数据 PCM 帧格式。
  - 3：原始音频数据 PCM 帧混音格式。
- `getVideoFrame`：录制视频格式。该参数设为 1，2，3 或 4 时，不可将 `isMixingEnabled` 设为 1。
  - 0：默认视频文件格式。
  - 1：原始视频数据 H.264 帧格式。
  - 2：原始视频数据 YUV 帧格式。
  - 3：原始视频数据 JPG 帧格式。
  - 4：JPG 文件格式。
  - 5：JPG 文件格式 + MP4 视频文件格式。
    - 单流模式（`isMixingEnabled` 为 0）下，录制得到多个 MP4 视频文件，并截图获得多个 JPG 文件。
    - 合流模式 (`isMixingEnabled` 为 1）下，对合流录制，得到一个 MP4 视频文件，并对各单流截图，获得多个 JPG 文件。
- `captureInterval`：截图的时间间隔，最小值为 1 秒，默认值为 5 秒，只有在 `getVideoFrame` 设为 3，4 或 5 时生效。

### 合流模式录制

- `isMixingEnabled`：是否启用合流模式。
  - 0：（默认）启用单流模式录制。一个 UID 对应一个音频文件和一个视频文件。录制文件的音频属性为：采样率固定为 48 kHz，声道数和码率与原始音频流保持一致。录制文件的视频属性与原始视频流保持一致。
  - 1：启用合流模式录制。多个 UID 的音频混合成一个纯音频文件，多个 UID 的视频混合成一个纯视频文件。无论频道内有多少用户，都只生成 1 个混合音频文件和 1 个混合视频文件。混合文件的音频属性通过 `audioProfile` 参数进行设置，视频属性通过 `mixResolution`参数进行设置。
- `mixedVideoAudio`：如果 `isMixingEnabled` 设为 1 ，该参数可以设置是否实时混合语音和视频:
  - 0：（默认）不混合音频和视频。
  - 1：音频和视频混合成一个文件，录制文件格式为 MP4，但播放器支持有限。
  - 2：音频和视频混合成一个文件，录制文件格式为 MP4，支持更多播放器。  
 具体的播放器支持请参考 [FAQ](https://docs.agora.io/cn/faq/recording_player)。
- `layoutMode`：设置视频合流布局。详见[设置合流布局](../../cn/Recording/recording_layout_guide.md)。
  
  - 0：（默认）悬浮布局。第一个加入频道的用户在屏幕上会显示为大视窗，铺满整个画布，其他用户的视频画面会显示为小视窗，从下到上水平排列，最多 4 行，每行 4 个画面，最多支持共 17 个录制画面。
  - 1：自适应布局。根据用户的数量自动调整每个画面的大小，每个用户的画面大小一致，最多支持 17 个录制画面。
  - 2：垂直布局。指定一个用户在屏幕左侧显示大视窗画面，其他用户的小视窗画面在右侧垂直排列，最多两列，一列 8 个画面，最多支持共 17 个录制画面。
- `maxResolutionUid`：如果 `layoutMode` 设为 2（垂直布局），用该参数设置显示大流画面的用户 UID。
- `audioProfile`：设置音频编码配置，包括采样率、码率和声道数。仅在 `isMixingEnabled` 设为 1 时生效。
  - 0：音频默认设置，采样率 48 kHz，单声道，编码码率为 48 Kbps。
  - 1：高音质，采样率 48 kHz，单声道，编码码率 128 Kbps。
  - 2：高音质立体声，采样率 48 kHz，双声道，编码码率 192 Kbps。双声道不支持原始音频数据。

- `mixResolution`：如果 `isMixingEnabled` 设为 1，可以通过该参数设置合流视频的分辨率，格式为：width，high，fps，Kbps，从左至右分别对应合图的宽、高、帧率和码率。你可以参考[视频属性表](https://docs.agora.io/cn/faq/recording_video_profile)进行设置。
- `defaultVideoBg`：设置合流模式下画布的默认背景图片的路径，仅支持本地 JPEG 文件。如不设置会显示黑色。
- `defaultUserBg`：设置合流模式下用户的默认背景图片的路径，仅支持本地 JPEG 文件。如不设置会显示黑色。合流模式下，如果某用户没有视频流，就会显示该图片。
  
  > 该设置对 Agora Web SDK 的用户无效。

### 录制文件路径

以下两个参数都可以设置录制文件保存的路径，两个参数只能选择一个设置，不能同时使用。

- `recordFileRootDir`：设置录制文件存放的根目录。设置该参数后，会按照录制日期自动生成子目录保存录制文件。
- `cfgFilePath`：指定配置文件的路径。在该配置文件里，你可以设置保存录制文件的绝对路径，但不会自动生成子目录。配置文件的内容必须为 JSON 格式，例如：`{"Recording_Dir" : "recording path"}`，其中 `"Recording_Dir"` 是固定的，不能改动。

### 其他设置

- `proxyServer`：代理服务器的 IP 地址和端口号，如 `127.0.0.1:1080`。
- `audioIndicationInterval`：说话者监测的时间间隔。默认禁用。
  - 0：（默认）禁用说话者监测的功能。
  - \> 0：说话者监测的时间间隔，单位为 ms 。建议设置时间间隔大于 200 ms。一旦检测到频道内有人说话，命令行中会打印出当前时间段内声音最大的用户的 UID 以及当前时间段内所有说话者的 UID 和音量。
- `logLevel`：设置日志过滤等级。设置完成后，只有低于和等于所设等级的日志才会被生成。默认值为 5。
  - 1：日志等级为 Fatal。
  - 2：日志等级为 Error。
  - 3：日志等级为 Warn。
  - 4：日志等级为 Notice。
  - 5：日志等级为 Info。
  - 6：日志等级为 Debug。

## 相关文档

录制完成后，你可能需要使用转码脚本将录制的文件进行合成，详见[使用转码脚本](../../cn/Recording/recording_manage_files.md)。 

录制过程中，如果出现错误码或警告码，请参考[警告码](https://docs.agora.io/cn/Recording/API%20Reference/recording_cpp/namespaceagora_1_1linuxsdk.html#a11cab69078db26c1f166c68e469dcfcf)和[错误码](https://docs.agora.io/cn/Recording/API%20Reference/recording_cpp/namespaceagora_1_1linuxsdk.html#a5f37e3fa14fad2982f248d247d76996b)。
