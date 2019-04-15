
---
title: 使用双声道/高音质
description: How to set the audio profile on Windows
platform: Windows
updatedAt: Tue Dec 25 2018 10:33:06 GMT+0800 (CST)
---
# 使用双声道/高音质
## 功能描述
 在一些比较专业的场景里，用户对声音的效果尤为敏感，比如语音电台，此时就需要对双声道和高音质的支持。
 所谓的高音质指的是我们提供采样率为 48 Khz、码率 192 Kbps 的能力，帮助用户实现高逼真的音乐场景，这种能力在语音电台、唱歌比赛类直播场景中应用较多。
## 实现方法
开始前请确保你已完成环境准备、安装包获取等步骤，详见[集成客户端](../../cn/Video/windows_video.md)。

Agora SDK 提供 [setAudioProfile](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#ab0cb52e238b729a15525a5cc12543d9e) 方法给开发者根据场景需求灵活配置适合的音质属性。这个方法有 2 个参数：

- `profile` 代表不同的音频参数配置，比如采样率、码率和编码模式等。
- `scenario` 侧重于不同的使用场景，如娱乐、教学和游戏直播等。流畅度、噪声控制、音质等频道特性会根据不同的场景做出优化。

```c++
RtcEngineParameters rep(*lpAgoraEngine);
// 设置双声道高音质
int nRet = rep.setAudioProfile(AUDIO_PROFILE_MUSIC_HIGH_QUALITY_STEREO, AUDIO_SCENARIO_DEFAULT);
```

除高音质外，本文同时给出几种常用的参数组合供开发者参考。

```C++
// FM 高音质
rep.setAudioProfile(AUDIO_PROFILE_TYPE::AUDIO_PROFILE_MUSIC_HIGH_QUALITY_STEREO, AUDIO_PROFILE_TYPE::AUDIO_SCENARIO_SHOWROOM);

// 游戏开黑场景
rep.setAudioProfile(AUDIO_PROFILE_TYPE::AUDIO_PROFILE_SPEECH_STANDARD, AUDIO_PROFILE_TYPE::AUDIO_SCENARIO_CHATROOM_GAMING);

// 娱乐场景
rep.setAudioProfile(AUDIO_PROFILE_TYPE::AUDIO_PROFILE_MUSIC_STANDARD, AUDIO_PROFILE_TYPE::AUDIO_SCENARIO_CHATROOM_ENTERTAINMENT);

// KTV
rep.setAudioProfile(AUDIO_PROFILE_TYPE::AUDIO_PROFILE_MUSIC_HIGH_QUALITY, AUDIO_PROFILE_TYPE::AUDIO_SCENARIO_CHATROOM_ENTERTAINMENT);
```


### API 参考

- [`setAudioProfile`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#ab0cb52e238b729a15525a5cc12543d9e)

## 开发注意事项

- 该方法必须在加入频道之前调用
- `scenario` 参数仅在频道模式设为直播时有效
