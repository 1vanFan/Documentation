
---
title: 修改音视频原始数据
description: 
platform: iOS,macOS
updatedAt: Thu Jun 20 2019 03:08:31 GMT+0800 (CST)
---
# 修改音视频原始数据
Agora 原始数据接口是 SDK 库提供的高级功能，便于你（开发者）获取媒体引擎的原始语音或视频数据。开发者可以修改语音或视频数据，创建特效来更好地满足自己应用程序的特殊需求。

你可以在将数据发送给编码器前插入一个前处理阶段，对捕捉到的视频帧或语音信号进行修改。也可以在将数据发送给解码器后插入一个后处理阶段，对接收到的视频帧和语音信号进行修改。

Agora 原始数据接口是一个 C++ 接口。

## 修改语音数据

1. 定义 `AgoraAudioFrameObserver` 继承 `IAudioFrameObserver` \(接口类 `IAudioFrameObserver` 在 `IAgoraMediaEngine.h` 定义\) 。需要实现以下虚拟接口:

	```c++
	class AgoraAudioFrameObserver : public agora::media::IAudioFrameObserver
	{
		public:
		    // 10 秒自动回调：获取录制的声音
			virtual bool onRecordAudioFrame(AudioFrame& audioFrame) override
			{
				return true;
			}
			// 10 秒自动回调：获取播放的声音
			virtual bool onPlaybackAudioFrame(AudioFrame& audioFrame) override
			{
				return true;
			 }
			// 10 秒自动回调：获取混音前的指定用户的声音
			virtual bool onPlaybackAudioFrameBeforeMixing(unsigned int uid, AudioFrame& audioFrame) override
			 {
				return true;
			 }
			// 10 秒自动回调：获取录制和播放语音混音后的数据
			virtual bool onMixedAudioFrame(AudioFrame& audioFrame) override
			 {
			 return true;
			 }

	};
	```

	上述例子为语音前处理或后处理接口返回 true。如有需要，用户可以修改数据:

	```c++
	class IAudioFrameObserver
	{
		public:
				enum AUDIO_FRAME_TYPE {
				FRAME_TYPE_PCM16 = 0, //PCM 16bit little endian
				};
		struct AudioFrame {
				AUDIO_FRAME_TYPE type;
				int samples;  //number of samples in this frame
				int bytesPerSample; //number of bytes per sample: 2 for PCM 16
				int channels; // number of channels (data are interleaved if stereo)
				int samplesPerSec; //sampling rate
				void* buffer; //data buffer
				int64_t renderTimeMs;
			 };
		public:
				virtual bool onRecordAudioFrame(AudioFrame& audioFrame) = 0;
				virtual bool onPlaybackAudioFrame(AudioFrame& audioFrame) = 0;
				virtual bool onPlaybackAudioFrameBeforeMixing(unsigned int uid, AudioFrame& audioFrame) = 0;
				virtual bool onMixedAudioFrame(AudioFrame& audioFrame) = 0;
	};
	```

2. 向媒体引擎注册视频帧观测器。在创建了 `IRtcEngine` 对象后、加入频道前，你可以注册语音观测器对象。

	```c++
	AgoraAudioFrameObserver s_audioFrameObserver;

	agora::util::AutoPtr<agora::media::IMediaEngine> mediaEngine;
	mediaEngine.queryInterface(*engine, agora::AGORA_IID_MEDIA_ENGINE);
	if (mediaEngine)
	{
		mediaEngine->registerAudioFrameObserver(&s_audioFrameObserver);
	}
	```

> 可以通过以下方法获得 `engine`。其中 kit 指的是 `AgoraRtcEngineKit`。
>
> ```
> agora::IRtcEngine* rtc_engine = (agora::IRtcEngine*)kit.getNativeHandle;
> agora::util::AutoPtr<agora::media::IMediaEngine> mediaEngine;
> mediaEngine.queryInterface(rtc_engine, agora::AGORA_IID_MEDIA_ENGINE);
> ```


