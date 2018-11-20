
---
title: 修改裸数据
description: 
platform: Android
updatedAt: Fri Sep 28 2018 02:24:24 GMT+0800 (CST)
---
# 修改裸数据
# 修改裸数据

Agora 原始数据接口是 SDK 库提供的高级功能，便于你（开发者）获取媒体引擎的原始语音或视频数据。开发者可以修改语音或视频数据，创建特效来更好地满足自己应用程序的特殊需求。

你可以在将数据发送给编码器前插入一个前处理阶段，对捕捉到的视频帧或语音信号进行修改。也可以在将数据发送给解码器后插入一个后处理阶段，对接收到的视频帧和语音信号进行修改。

Agora 原始数据接口是一个 C++ 接口。你需要在 Android 上使用 SDK 库的 JNI 和插件管理器。

## 修改语音数据

1.  定义 `AgoraAudioFrameObserver` 继承 `IAudioFrameObserver` \(接口类 `IAudioFrameObserver` 在 `IAgoraMediaEngine.h` 定义\) 。需要实现以下虚拟接口:

	```
	class AgoraAudioFrameObserver : public agora::media::IAudioFrameObserver
	{
		public:
			virtual bool onRecordAudioFrame(AudioFrame& audioFrame) override
			{
				return true;
			}
			virtual bool onPlaybackAudioFrame(AudioFrame& audioFrame) override
			{
				return true;
			 }
			virtual bool onPlaybackAudioFrameBeforeMixing(unsigned int uid, AudioFrame& audioFrame) override
			 {
				return true;
			 }
			virtual bool onMixedAudioFrame(AudioFrame& audioFrame) override
			 {
			 return true;
			 }

	};
	```

	上述例子为语音前处理或后处理接口返回 true。如有需要，用户可以修改数据:

	```
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

2.  向媒体引擎注册语音帧观测器。在创建了 `IRtcEngine` 对象后、加入频道前，你可以注册语音观测器对象。

	```
	AgoraAudioFrameObserver s_audioFrameObserver;

	agora::util::AutoPtr<agora::media::IMediaEngine> mediaEngine;
	mediaEngine.queryInterface(engine, agora::AGORA_IID_MEDIA_ENGINE);
	if (mediaEngine)
	{
		mediaEngine->registerAudioFrameObserver(&s_audioFrameObserver);
	}
	```

> 这里的 `engine` 可以通过 [创建插件](#create_plugin) 里的 `loadAgoraRtcEnginePlugin` 传入。

## 修改视频数据

1.  定义 `AgoraVideoFrameObserver` 继承 `IVideoFrameObserver` \(接口类 `IVideoFrameObserver` 在 `IAgoraMediaEngine.h` 定义\) 。需要实现以下虚拟接口:

    ```
    class AgoraVideoFrameObserver : public agora::media::IVideoFrameObserver
    {
      public:
        virtual bool onCaptureVideoFrame(VideoFrame& videoFrame) override
        {
          return true;
        }
        virtual bool onRenderVideoFrame(unsigned int uid, VideoFrame& videoFrame) override
        {
          return true;
        }
    };
    ```

	上述例子仅需保证语音前后处理的返回值为 true。如有需要，可参考 API 修改语音帧的样本数量、频道数量、采样率等:

	```
		class IVideoFrameObserver
		{
			public:
				enum VIDEO_FRAME_TYPE {
				FRAME_TYPE_YUV420 = 0,  //YUV 420 format
				};
			struct VideoFrame {
				VIDEO_FRAME_TYPE type;
				int width;  //width of video frame
				int height;  //height of video frame
				int yStride;  //stride of Y data buffer
				int uStride;  //stride of U data buffer
				int vStride;  // stride of V data buffer
				void* yBuffer;  //Y data buffer
				void* uBuffer;  //U data buffer
				void* vBuffer;  //V data buffer
				int rotation; // rotation of this frame (0, 90, 180, 270)
				int64_t renderTimeMs;
				};
			public:
				virtual bool onCaptureVideoFrame(VideoFrame& videoFrame) = 0;
				virtual bool onRenderVideoFrame(unsigned int uid, VideoFrame& videoFrame) = 0;
		};
	```

2.  向媒体引擎注册视频帧观测器。在创建了 `IRtcEngine` 对象后、开启视频模式后、加入频道前，你可以注册观测器对象。

	```
	AgoraVideoFrameObserver s_videoFrameObserver;

	agora::util::AutoPtr<agora::media::IMediaEngine> mediaEngine;
		mediaEngine.queryInterface(engine,agora::AGORA_IID_MEDIA_ENGINE);
		if (mediaEngine)
		{
			 mediaEngine->registerVideoFrameObserver(&s_videoFrameObserver);
		}
	```

> 这里的 `engine` 可以通过 [创建插件](#create_plugin) 里的 `loadAgoraRtcEnginePlugin` 传入。


<a name = "create_plugin"></a>
## 创建插件

### 具体用法

Agora Native SDK 支持以动态链接库的方式加载第三方插件。具体用法如下:

1.  创建一个共享库工程，SO 的命名必须以 `libapm-` 为前缀，`so` 后缀，比如 `libapm-encryption.so` 。

2.  实现和导出相关接口函数。

	-   以下为必须实现的插件接口函数，在 SDK 加载插件时会被调用，注册成功时返回 0，失败时返回非 0。

	```
	extern "C" __attribute__((visibility("default"))) int loadAgoraRtcEnginePlugin(agora::IRtcEngine* engine);
	```

	代码示例如下:

	```
	extern "C" __attribute__((visibility("default"))) int loadAgoraRtcEnginePlugin(agora::IRtcEngine* engine)
	{
			agora::util::AutoPtr<agora::media::IMediaEngine> mediaEngine;
			mediaEngine.queryInterface(engine,agora::AGORA_IID_MEDIA_ENGINE);
			if (mediaEngine)
			{
					mediaEngine->registerVideoFrameObserver(&s_videoFrameObserver);
			}
		return 0;
	}
	```

	-   以下为可选的插件接口函数，在 SDK 卸载插件时会被调用。

	```
	extern "C" __attribute__((visibility("default"))) void unloadAgoraRtcEnginePlugin(agora::IRtcEngine* engine);
	```

	示例代码如下:

	```
	extern "C" __attribute__((visibility("default"))) void unloadAgoraRtcEnginePlugin(agora::IRtcEngine* engine)
	{
			agora::util::AutoPtr<agora::media::IMediaEngine> mediaEngine;
			mediaEngine.queryInterface(engine,agora::AGORA_IID_MEDIA_ENGINE);
			if (mediaEngine)
			{
					mediaEngine->registerVideoFrameObserver(NULL);
			}
	}
	```

### 加载插件过程

Agora Native SDK 加载插件的过程如下:

1.  SDK 引擎在初始化时搜索 native 库所在目录下的所有匹配 `libapm-xxx.so` 格式的动态链接库，如 `libapm-encryption.so` 就是一个符合格式的插件名字；

2.  SDK 用 `dlopen` 加载插件；

3.  插件必须按上述要求实现并导出 `loadAgoraRtcEnginePlugin` 接口，以及可选实现 `unloadAgoraRtcEnginePlugin` 接口。针对每个找到的插件，SDK 获取并调用 `loadAgoraRtcEnginePlugin` 函数，`loadAgoraRtcEnginePlugin` 返回 0 表示成功，如果返回非 0，SDK 会卸载该插件 \(FreeLibrary\)；该接口的输入参数为 `agora::IRtcEngine` 接口对象，插件可以在 `loadAgoraRtcEnginePlugin` 函数里使用该对象注册 `IPacketObserver`、`IAudioFrameObserver` 或 `IVideoFrameObserver` 等。

4.  SDK 引擎销毁时，调用插件的 `unloadAgoraRtcEnginePlugin` 入口函数，该函数为可选实现。



