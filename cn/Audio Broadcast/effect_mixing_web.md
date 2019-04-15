
---
title: 播放音效/音乐混音
description: How to enable audio mixing on the Web
platform: Web
updatedAt: Wed Apr 03 2019 06:33:04 GMT+0800 (CST)
---
# 播放音效/音乐混音
## 功能描述

在通话或直播过程中，除了用户自己说话的声音，有时候需要播放自定义的声音或者音乐文件并且让频道内的其他人也听到，比如需要给游戏添加音效，或者需要播放背景音乐等，Agora 提供以下两组方法可以满足播放音效和音乐文件的需求。

## 播放音效文件

音效通常指持续很短的音频。播放音效文件方法主要用来播放短小的音效，比如鼓掌、游戏子弹撞击声音等，可以多个音效叠加播放，且音效文件可以预加载以提高性能。
音效由音频文件路径指定，soundId 为自行设定的音效 ID，需保证唯一性。SDK 并不强制如何定义 sound ID，保证每个音效有唯一的识别即可。一般的做法有自增 ID 等。

### 实现方法

开始前请确保你已完成环境准备、安装包获取等步骤，详见[集成客户端 ](../../cn/Audio%20Broadcast/web_prepare.md)。

```javascript
// 预加载音效，需注意音效文件的大小
stream.preloadEffect(1, "https://web-demos-static.agora.io/agora/smlt.flac", function(err){
    if (err){
        console.error("Failed to preload effect, reason: ", err);
    }else{
        console.log("Effect is preloaded successfully");
    }
});

// 播放音效
stream.playEffect({
    soundId: 1,
    filePath: "https://web-demos-static.agora.io/agora/smlt.flac"
}, function(error) {
    if (error) {
        // 错误处理
        return;
    }
    // 播放成功后的流程
});

// 获取所有音效播放的音量
var volumes = stream.getEffectsVolume();
volumes.forEach(function({soundId, volume}){
    console.log("SoundId", soundId, "Volume", volume);
});

// 暂停播放所有音效
stream.pauseAllEffects(function(err){
    if (err){
        console.error("Failed to pause effects, reason: ", err);
    }else{
        console.log("Effects are paused successfully");
    }
});

// 继续播放暂停的音效
stream.resumeAllEffects(function(err){
    if (err){
        console.error("Failed to resume effects, reason: ", err);
    }else{
        console.log("Effects are resumed successfully");
    }
});

// 停止所有音效
stream.stopAllEffects(function(err){
    if (err){
        console.error("Failed to stop effects, reason: ", err);
    }else{
        console.log("Effects are stopped successfully");
    }
});

// 释放预加载的音效文件
stream.unloadEffect(1, function(err){
    if (err){
        console.error("Failed to unload effect, reason: ", err);
    }else{
        console.log("Effect is unloaded successfully");
    }
});
```

### API 参考

- [`Stream.playEffect`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/web/v2.6/interfaces/agorartc.stream.html?transId=2.6#playeffect)
- [`Stream.stopEffect`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/web/v2.6/interfaces/agorartc.stream.html?transId=2.6#stopeffect)
- [`Stream.pauseEffect`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/web/v2.6/interfaces/agorartc.stream.html?transId=2.6#pauseeffect)
- [`Stream.resumeEffect`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/web/v2.6/interfaces/agorartc.stream.html?transId=2.6#resumeeffect)
- [`Stream.setVolumeOfEffect`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/web/v2.6/interfaces/agorartc.stream.html?transId=2.6#setvolumeofeffect)
- [`Stream.preloadEffect`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/web/v2.6/interfaces/agorartc.stream.html?transId=2.6#preloadeffect)
- [`Stream.unloadEffect`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/web/v2.6/interfaces/agorartc.stream.html?transId=2.6#unloadeffect)
- [`Stream.getEffectsVolume`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/web/v2.6/interfaces/agorartc.stream.html?transId=2.6#geteffectsvolume)
- [`Stream.setEffectsVolume`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/web/v2.6/interfaces/agorartc.stream.html?transId=2.6#seteffectsvolume)
- [`Stream.stopAllEffects`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/web/v2.6/interfaces/agorartc.stream.html?transId=2.6#stopalleffects)
- [`Stream.pauseAllEffects`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/web/v2.6/interfaces/agorartc.stream.html?transId=2.6#pausealleffects)
- [`Stream.resumeAllEffects`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/web/v2.6/interfaces/agorartc.stream.html?transId=2.6#resumealleffects)

### 开发注意事项

- 仅支持在线音效文件
- 支持的音频格式为 MP3，AAC 以及浏览器支持的其他音频格式
- 以上方法的回调均使用 Node.js 回调风格

## 音乐混音

混音是指播放音乐文件，同时让频道内的其他人听到此音乐。混音方法主要用来播放比较长的背景音，比如直播的时候播放的音乐，同时只可以有一个文件播放。如果在混音播放第一个文件的过程中播放第二个文件，会自动停止第一个文件的播放。
Agora 混音功能支持如下设置：

- 混音或替换： 混音指的是音乐文件的音频流跟麦克风采集的音频流进行混音（叠加）并编码发送给对方；替换指的是麦克风采集的音频被音乐文件的音频流替换掉，对方只能听见音乐播放。
- 循环：可以设置是否循环播放混音文件，以及循环次数。

### 实现方法

开始前请确保你已完成环境准备、安装包获取等步骤，详见[集成客户端 ](../../cn/Audio%20Broadcast/web_prepare.md)。

```javascript
// 设置混音选项
// cacheResource 选填，设置是否缓存音乐文件，默认为 true。
// filePath 必填，设置混音文件路径，仅支持在线文件。
// cycle 选填，设置音频文件循环播放的次数，仅支持 Chrome 65+。如不设置则默认播放一次。 
// replace 选填，设置是否用音频文件内容替换本地麦克风采集的音频流。默认为 false。
// playTime 必填，设置音频文件开始播放的位置，单位为 ms。设为 0 即从头开始播放。 
var options = {
      cacheResource: false,
      filePath: "http://www.hochmuth.com/mp3/Haydn_Cello_Concerto_D-1.mp3", 
      cycle: 1, 
      replace: false, 
      playTime:0 
}

// 开始混音
localStream.startAudioMixing(options, function(err){
     if (err){
             console.log("Failed to start audio mixing. " + err);
      }
});

// 调整混音的音量。取值为 [0, 100]，100 代表维持原来混音的音量（默认）。
localStream.adjustAudioMixingVolume(volume);

// 获取当前播放的混音音乐的位置，返回值单位为 ms。
localStream.getAudioMixingCurrentPosition();

// 设置混音文件开始播放的位置，单位为 ms。
localStream.setAudioMixingPosition(pos);

// 暂停播放混音文件
localStream.pauseAudioMixing();

// 恢复播放混音文件
localStream.resumeAudioMixing();

// 获取当前混音的播放进度，返回值单位为 ms。
localStream.getAudioMixingCurrentPosition();

// 获取混音文件播放时长，返回值单位为 ms。
localStream.getAudioMixingDuration();

// 停止混音
localStream.stopAudioMixing();
```

### API 参考

- [`startAudioMixing`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#startaudiomixing)
- [`stopAudioMixing`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#stopaudiomixing)
- [`adjustAudioMixingVolume`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#adjustaudiomixingvolume)
- [`pauseAudioMixing`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#pauseaudiomixing)
- [`resumeAudioMixing`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#resumeaudiomixing)
- [`getAudioMixingDuration`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#getaudiomixingduration)
- [`getAudioMixingCurrentPosition`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#getaudiomixingcurrentposition)
- [`setAudioMixingPosition`](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#setaudiomixingposition)

### 开发注意事项

- 仅支持在线音频文件混音
- 混音方法支持以下浏览器：
  - Safari 12+
  - Chrome 65+
  - 最新版 Firefox
- 请在频道内调用该方法，如果在频道外调用该方法可能会出现问题。
- 部分参数选项只有特定浏览器支持，详见 [startAudioMixing](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#startaudiomixing)。
