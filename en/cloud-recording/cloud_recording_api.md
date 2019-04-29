
---
title: Cloud Recording API
description: 
platform: CPP
updatedAt: Mon Apr 29 2019 02:59:12 GMT+0800 (CST)
---
# Cloud Recording API
| **Interface Class**                                          | **Description**                       |
| ------------------------------------------------------------ | ------------------------------------- |
| <a href="#ICloudRecorder">ICloudRecorder</a>                 | Provides methods invoked by your app. |
| <a href="#ICloudRecorderObserver">ICloudRecorderObserver</a> | Enables callbacks to your app.        |

<a name = "ICloudRecorder"></a>

## ICloudRecorder class

The ICloudRecorder class provides the main methods that can be invoked by your app and consists of the following methods:

- [`CreateCloudRecorderInstance`](#CreateCloudRecorderInstance)
- [`GetRecordingId`](#GetRecordingId)
- [`StartCloudRecording`](#StartCloudRecording)
- [`StopCloudRecording`](#StopCloudRecording)
- [`Release`](#Release)

<a name = "CreateCloudRecorderInstance"></a>

### CreateCloudRecorderInstance

```c++
ICloudRecorder* CreateCloudRecorderInstance(ICloudRecorderObserver *observer);
```

Creates an Agora Cloud Recording instance. Each recording instance has a unique recording ID.

| Parameter  | Description                                                  |
| ---------- | ------------------------------------------------------------ |
| `observer` | The Agora Cloud Recording SDK notifies the app of the triggered events by callbacks in the <a href="#ICloudRecorderObserver">ICloudRecorderObserver</a> class. |

<a name = "GetRecordingId"></a>

### GetRecordingId

```c++
virtual RecordingId GetRecordingId() = 0;
```

Gets a Cloud Recording ID.

| Parameter      | Description                                                  |
| -------------- | ------------------------------------------------------------ |
| `recording_id` | The recording ID. The unique identification of the current recording. |

<a name = "StartCloudRecording"></a>

### StartCloudRecording

```c++
virtual int StartCloudRecording(
  const char* appId,
  const char* channel_name,
  const char* token,
  const uid_t uid,
  const RecordingConfig& recording_config,
  const CloudStorageConfig &storage_config) = 0;
```

Allows the Agora Cloud Recording SDK to join a channel and start recording.

| Parameter          | Description                                                  |
| ------------------ | ------------------------------------------------------------ |
| `appId`            | The app ID of the channel to be recorded. See <a href="../../en/cloud-recording/token.md">Getting an App ID</a>. |
| `channel_name`     | The name of the channel to be recorded.                          |
| `token`            | The Token used in the channel to be recorded. See <a href="../../en/cloud-recording/token.md">Use Security Keys</a>. |
| `uid`              | The UID of the recording client. A 32-bit unsigned integer ranging from 1 to (2<sup>32</sup>-1) that is unique in the channel. |
| `recording_config` | The recording configurations. See <a href="#RecordingConfig">RecordingConfig</a>. |
| `storage_config`   | The Third-party cloud storage configurations. See <a href="#CloudStorageConfig">CloudStorageConfig</a>. |

#### Returns

- 0: Success.
- ≠ 0: Failure.

<a name = "RecordingConfig"></a>

#### RecordingConfig

```c++
RecordingConfig() :
  recording_stream_types(kStreamTypeAudioVideo),
  decryption_mode(kModeNone),
  secret(0),
  channel_type(kChannelTypeCommunication),
  audio_profile(kAudioProfileMusicStandard),
  video_stream_type(kVideoStreamTypeHigh),
  max_idle_time(30) {
}
```

| Parameter                | Description                                                  |
| ------------------------ | ------------------------------------------------------------ |
| `recording_stream_types` | The type of the media stream: <li>`kStreamTypeAudioVideo`: (Default) Records audio and video.<li>`kStreamTypeAudioOnly`: Records audio only.<li>`kStreamTypeVideoOnly`: Records video only. |
| `decryption_mode`        | When the channel is encrypted, the Agora Cloud Recording SDK uses `decryption_mode` to enable the built-in decryption function. The decryption mode must be the same as the encryption mode set by the Agora Native/Web SDK. The following decryption methods are supported:<li>`kModeNone`: (Default) None.<li>`kModeAes128Xts`: AES-128, XTS mode.<li>`kModeAes128Ecb`: AES-128, ECB mode. <li>`kModeAes256Xts`: AES-256, XTS mode. |
| `secret`                 | The decryption password when decryption mode is enabled. The default value is NULL. |
| `channel_type`           | Channel profile: <li>`kChannelTypeCommunication`: (Default) Communication profile. This profile is used in one-on-one or group calls, where all users in the channel can talk freely.<li>`kChannelTypeLive`: Live broadcast. This profile includes two roles, host and audience. The host sends and receives audio/video, while the audience only receives audio/video. <p>**The Agora Cloud Recording SDK must use the same channel profile as the Agora Native/Web SDK. Otherwise, issues may occur.** |
| `audio_profile`          | Sets the sample rate, bitrate, encoding mode, and the number of channels.<li>`kAudioProfileMusicStandard`: (Default) Sample rate of 48 kHz, music encoding, mono, and bitrate of up to 48 Kbps.<li>`kAudioProfileMusicHighQuality`: Sample rate of 48 kHz, music encoding, mono, and bitrate of up to 128 Kbps.<li>`kAudioProfileMusicHighQualityStereo`: Sample rate of 48 kHz, music encoding, stereo, and bitrate of up to 192 Kbps. |
| `video_stream_type`      | Sets the type of the video stream.<li>`kVideoStreamTypeHigh`: (Default) Records the high-stream video.<li>`kVideoStreamTypeLow`: Records the low-stream video. |
| `max_idle_time`          | The Agora Cloud Recording SDK automatically stops recording when there is no user in the recording channel after a time period (30 seconds by default) set by this parameter. |
| `transcoding_config`     | The transcoding configurations. See <a href="#TranscodingConfig">TranscodingConfig</a>. |

<a name = "TranscodingConfig"></a>

#### TranscodingConfig

```c++
TranscodingConfig() :
  width(360),
  height(640),
  fps(15),
  bitrate(500),
  max_resolution_uid(0),
  layout(kMixedVideoLayoutTypeFloat) {
}
```

| Parameter            | Description                                                  |
| -------------------- | ------------------------------------------------------------ |
| `width`              | The width of the mixed video (pixels). The default value is 360. The maximum resolution is 1080p and an error occurs if the maximum resolution is exceeded.          |
| `height`             | The Height of the mixed video (pixels). The default value is 640. The maximum resolution is 1080p and an error occurs if the maximum resolution is exceeded.         |
| `fps`                | The video frame rate (fps). The default value is 15.           |
| `bitrate`            | The video bitrate (Kbps). The default value is 500.           |
| `max_resolution_uid` | When `layout` is set as `kMixedVideolayoutTypeVertical` (vertical layout), you can specify the uid of the large video window by this parameter. |
| `layout`             | Sets the video mixing layout. <li>`kMixedVideoLayoutTypeFloat`: Default layout. The first user joining the channel displays a large video window on the top of the screen, while video streams from other users are arranged horizontally at the bottom in small video windows. A maximum of two rows with eight windows in each row is supported.<p>![](https://web-cdn.agora.io/docs-files/1550657482861)<li>`kMixedVideoLayoutTypeBestFit`: Best fit. This layout automatically scales according to the number of video streams. The number of columns and rows varies depending on the number of video streams. Supports up to 17 video streams.<p>![](https://web-cdn.agora.io/docs-files/1547544528138)<li>`kMixedVideolayoutTypeVertical`: Vertical layout. A specified uid displays a large video window on the left of the screen, while video streams from other users are arranged vertically on the right in small video windows. A maximum of two columns with eight windows in each column is supported. <p>![](https://web-cdn.agora.io/docs-files/1547544539933) |

<a name = "resolution_table"></a>

For detailed video profile, see **Resolution, Frame Rate and Bitrate Table**.

| Resolution | Frame rate (fps) | Base Bitrate (Kbps, for Communication) | Live Bitrate (Kbps, for Live Broadcast) |
| ---------- | ----------------- | -------------------------------------- | --------------------------------------- |
| 160 × 120  | 15                | 65                                     | 130                                     |
| 120 × 120  | 15                | 50                                     | 100                                     |
| 320 × 180  | 15                | 140                                    | 280                                     |
| 180 × 180  | 15                | 100                                    | 200                                     |
| 240 × 180  | 15                | 120                                    | 240                                     |
| 320 × 240  | 15                | 200                                    | 400                                     |
| 240 × 240  | 15                | 140                                    | 280                                     |
| 424 × 240  | 15                | 220                                    | 440                                     |
| 640 × 360  | 15                | 400                                    | 800                                     |
| 360 × 360  | 15                | 260                                    | 520                                     |
| 640 × 360  | 30                | 600                                    | 1200                                    |
| 360 × 360  | 30                | 400                                    | 800                                     |
| 480 × 360  | 15                | 320                                    | 640                                     |
| 480 × 360  | 30                | 490                                    | 980                                     |
| 640 × 480  | 15                | 500                                    | 1000                                    |
| 480 × 480  | 15                | 400                                    | 800                                     |
| 640 × 480  | 30                | 750                                    | 1500                                    |
| 480 × 480  | 30                | 600                                    | 1200                                    |
| 848 × 480  | 15                | 610                                    | 1220                                    |
| 848 × 480  | 30                | 930                                    | 1860                                    |
| 640 × 480  | 10                | 400                                    | 800                                     |
| 1280 × 720 | 15                | 1130                                   | 2260                                    |
| 1280 × 720 | 30                | 1710                                   | 3420                                    |
| 960 × 720  | 15                | 910                                    | 1820                                    |
| 960 × 720  | 30                | 1380                                   | 2760                                    |



<a name = "CloudStorageConfig"></a>

#### CloudStorageConfig

```c++
struct CloudStorageConfig {
  CloudStorageVendor vendor;
  unsigned int region;
  char* bucket;
  char* access_key;
  char* secret_key;
};
```

| Parameter    | Description                                                  |
| ------------ | ------------------------------------------------------------ |
| `vendor`     | The third-party cloud storage. <li>0: Qiniu Cloud.<li>1: Amazon S3.<li>2: Aliyun. |
| `region`     | The regional information specified by the third-party cloud storage. <br>When the third-party cloud storage is Qiniu Cloud (`vendor` = 0):<li>0: Huadong <li>1: Huabei <li>2: Huanan <li>3: Beimei  <br>When the third-party cloud storage is Amazon S3 (`vendor` = 1): <li>0: US_EAST_1 <li>1: US_EAST_2 <li>2: US_WEST_1 <li>3: US_WEST_2  <li>4: EU_WEST_1 <li> 5: EU_WEST_2  <li>6: EU_WEST_3 <li>7: EU_CENTRAL_1 <li>8: AP_SOUTHEAST_1 <li>9: AP_SOUTHEAST_2 <li>10: AP_NORTHEAST_1 <li>11: AP_NORTHEAST_2 <li>12: SA_EAST_1 <li>13: CA_CENTRAL_1 <li>14: AP_SOUTH_1 <li>15: CN_NORTH_1 <li>16: CN_NORTHWEST_1 <li>17: US_GOV_WEST_1 <br>When the third-party cloud storage is Aliyun  (`vendor` = 2):<li>0：CN_Hangzhou <li>1：CN_Shanghai <li>2：CN_Qingdao <li>3：CN_Beijin  <li>4：CN_Zhangjiakou <li> 5：CN_Huhehaote  <li>6：CN_Shenzhen <li>7：CN_Hongkong <li>8：US_West_1 <li>9：US_East_1 <li>10：AP_Southeast_1 <li>11：AP_Southeast_2 <li>12：AP_Southeast_3 <li>13：AP_Southeast_5 <li>14：AP_Northeast_1 <li>15：AP_South_1 <li>16：EU_Central_1 <li>17：EU_West_1 <li>18：EU_East_1|
| `bucket`     | The bucket of the third-party cloud storage.                     |
| `access_key` | The access key of the third-party cloud storage.                 |
| `secret_key` | The secret key of the third-party cloud storage.                 |

<a name = "StopCloudRecording"></a>

### StopCloudRecording

```c++
virtual int StopCloudRecording() = 0;
```

Allows the Cloud Recording SDK to leave the channel and stop recording. If the `StopCloudRecording` method is not called, the SDK automatically leaves the channel and stops recording when no user is in the channel for more than 30s by default.
	
After the recording stops, you need to call `Release` to destroy the recording instance.

#### Returns

- 0: Success.
- ≠ 0: Failure.

<a name = "Release"></a>


### Release

```c++
virtual void Release(bool cancelCloudRecording = false) = 0;
```

Destroys the `ICloudRecorder` instance and releases all recording resources. To use Agora Cloud Recording again, you need to call the `CreateCloudRecorderInstance` method to recreate a new instance.

| Parameter                   | Description                                                  |
| --------------------------- | ------------------------------------------------------------ |
| `cancelCloudRecording` | Sets whether or not to record in the background: <li>`true`: The recording and uploading processes stop.<li>`false`: (Default) The recording and uploading processes continue in the background on the Agora server. |

<a name = "ICloudRecorderObserver"></a>

## ICloudRecorderObserver class

The `ICloudRecorderObserver` class provides callbacks to your app and consists of the following callbacks:

- [`OnRecordingConnecting`](#OnRecordingConnecting)
- [`OnRecordingStarted`](#OnRecordingStarted)
- [`OnRecordingStopped`](#OnRecordingStopped)
- [`OnRecordingUploaded`](#OnRecordingUploaded)
- [`OnRecordingBackedUp`](#OnRecordingBackedUp)
- [`OnRecordingUploadingProgress`](#OnRecordingUploadingProgress)
- [`OnRecorderFailure`](#OnRecorderFailure)
- [`OnUploaderFailure`](#OnUploaderFailure)
- [`OnRecordingFatalError`](#OnRecordingFatalError)

<a name = "OnRecordingConnecting"></a>

### OnRecordingConnecting

```c++
virtual void OnRecordingConnecting(RecordingId recording_id) = 0;
```

Occurs when the app is connecting to the Agora Cloud Recording server.

| Parameter      | Description                                                  |
| -------------- | ------------------------------------------------------------ |
| `recording_id` | The recording ID. The unique identification of the current recording. |

<a name = "OnRecordingStarted"></a>

### OnRecordingStarted

```c++
virtual void OnRecordingStarted(RecordingId recording_id) = 0;
```

Occurs when the Agora Cloud Recording SDK starts recording.

| Parameter      | Description                                                  |
| -------------- | ------------------------------------------------------------ |
| `recording_id` | The recording ID. The unique identification of the current recording. |

<a name = "OnRecordingStopped"></a>

### OnRecordingStopped

```c++
virtual void OnRecordingStopped(RecordingId recording_id) = 0;
```

Occurs when the Agora Cloud Recording SDK stops recording.

| Parameter      | Description                                                  |
| -------------- | ------------------------------------------------------------ |
| `recording_id` | The recording ID. The unique identification of the current recording. |

<a name = "OnRecordingUploaded"></a>

### OnRecordingUploaded

```c++
virtual void OnRecordingUploaded(RecordingId recording_id,
	const char* file_name) = 0;
```

Occurs when all the recorded file are uploaded to the third-party cloud storage. 

| Parameter      | Description                                                  |
| -------------- | ------------------------------------------------------------ |
| `recording_id` | The recording ID. The unique identification of the current recording.  |
| `file_name`    | The filename of the M3U8 file. Each recording instance has an M3U8 file as the playlist pointing to all the recorded files. |

<a name = "OnRecordingBackedUp"></a>

### OnRecordingBackedUp

```c++
virtual void OnRecordingBackedUp(RecordingId recording_id, 
	const char* file_name) = 0;
```

Occurs when the recorded file is uploaded to Agora Cloud Backup. 
	
If any of the files fails to upload to the third-party cloud storage, the Agora server automatically uploads these files to Agora Cloud Backup and the SDK triggers this callback after the recording stops.

| Parameter      | Description                                                  |
| -------------- | ------------------------------------------------------------ |
| `recording_id` | The recording ID. The unique identification of the current recording. |
| `file_name`    | The filename of the M3U8 file. Each recording instance has an M3U8 file as the playlist pointing to all the recorded files.    |

<a name = "OnRecordingUploadingProgress"></a>

### OnRecordingUploadingProgress

```c++
virtual void OnRecordingUploadingProgress(RecordingId recording_id,
      unsigned int progress, const char* recording_playlist_filename) = 0;
```

Occurs when the recorded file is uploading to the third-party cloud storage. The SDK triggers this callback once every minute to inform the app of the uploading progress.

| Parameter                     | Description                                                  |
| ----------------------------- | ------------------------------------------------------------ |
| `recording_id`                | The recording ID. The unique identification of the current recording.|
| `progress`                    | The uploading progress. Shows the percentage of the uploaded files in the recorded files. |
| `recording_playlist_filename` | The filename of the M3U8 file. Each recording instance has an M3U8 file as the playlist pointing to all the recorded files. |

<a name = "OnRecorderFailure"></a>

### OnRecorderFailure

```c++
virtual void OnRecorderFailure(RecordingId recording_id,
	RecordingErrorCode code, const char* msg) = 0;
```

Occurs when an error occurs in the recorder component. This callback is only used to notify the state of the recorder component, and you do not need to do anything with this.

| Parameter      | Description                                                  |
| -------------- | ------------------------------------------------------------ |
| `recording_id` | The recording ID. The unique identification of the current recording.  |
| `code`         | Error code.                                                  |
| `msg`          | Error message.                                               |
	
<a name = "OnUploaderFailure"></a>

### OnUploaderFailure

```c++
virtual void OnUploaderFailure(RecordingId recording_id,
	RecordingErrorCode code, const char* msg) = 0;
```

Occurs when an error occurs in the uploader component. This callback is only used to notify the state of the uploader component, and you do not need to do anything with this.

| Parameter      | Description                                                  |
| -------------- | ------------------------------------------------------------ |
| `recording_id` | The recording ID. The unique identification of the current recording.  |
| `code`         | Error code.                                                  |
| `msg`          | Error message.                                               |

<a name = "OnRecordingFatalError"></a>

### OnRecordingFatalError

```c++
virtual void OnRecordingFatalError(RecordingId recording_id,
	RecordingErrorCode code) = 0;
```

Occurs when the Agora Cloud Recording SDK encounters an error that cannot be recovered automatically. You need to call the `Release` method to release all resources and check the status of the Agora Cloud Recording server according to the returned error code.

| Parameter      | Description                                                  |
| -------------- | ------------------------------------------------------------ |
| `recording_id` | The recording ID. The unique identification of the current recording. |
| `code`         | Error code.                                            |


<a name = "ErrorCode"></a>

## Error codes

The Agora Cloud Recording SDK may return the following error codes during runtime:

| Error Codes                         | Description                                                  | Solution                                                     |
| ----------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| `RecordingErrorOk`                 | No error.                                                    | None.                                                        |
| `RecordingErrorConnectError`       | Fails to connect to the Agora Cloud Recording server.        | Check the permission of appid and the network connection status. |
| `RecordingErrorDisconnected`       | The connection to the Agora Cloud Recording server is interrupted. | Check the network connection status.                         |
| `RecordingErrorInvalidParameter`   | Invalid parameters.                                          | Check the parameters.                                        |
| `RecordingErrorInvalidOperation`   | Invalid operation.                                           | Check the operations.                                        |
| `RecordingErrorNoUsers`            | No user is in the channel.                                   | Call the Release (false) method to release resources.        |
| `RecordingErrorNoRecordedData`     | No data is generated.                                        | Call the Release (false) method to release resources.        |
| `RecordingErrorRecorderInit`       | An error occurs in initializing the recorder component.      | Call the Release (true) method to restart recording.          |
| `RecordingErrorRecorderFailed`     | An error occurs in the recorder component.                   | Call the Release (true) method to restart recording.          |
| `RecordingErrorUploaderInit`       | An error occurs in initializing the uploader component.      | Call the Release (true) method to restart recording.          |
| `RecordingErrorUploaderFailed`     | An error occurs in the uploader component.                   | Call the Release (true) method to restart recording.          |
| `RecordingErrorBackupFailed`       | An error occurs in the cloud backup.                         | Call the Release (false) method to release resources.        |
| `RecordingErrorRecorderExpireExit` | Abnormal exit.                                               | Call the Release (true) method to restart recording.          |
| `RecordingErrorGeneralError`       | Other errors.                                                | Call the Release (true) method to restart recording.          |
