
---
title: 云端录制 RESTful API 快速开始
description: Quick start for rest api
platform: All Platforms
updatedAt: Tue Jul 02 2019 06:25:03 GMT+0800 (CST)
---
# 云端录制 RESTful API 快速开始
Agora 云端录制服务提供 RESTful API，无需集成 SDK，直接通过网络请求开启和控制云录制，在自己的网页或应用中灵活使用。

> - 你需要通过自己的 app server 发送网络请求。
> - 云端录制 RESTful API 仅支持 HTTPS 协议。

![img](https://web-cdn.agora.io/docs-files/1559295245761)

通过 RESTful API，你可以发送网络请求控制云端录制：

- 开始/结束云端录制
- 查询当前的录制状态

同时云端录制 RESTful API 还提供回调服务，开通后你可以收到云端录制的事件通知，下表列出回调服务和使用 RESTful API 查询录制状态的区别，你可以根据自己的需要选择是否开通回调服务。

| 查询状态                                                     | 回调服务                                         |
| ------------------------------------------------------------ | ------------------------------------------------ |
| 调用 [`query`](../../cn/cloud-recording/cloud_recording_api_rest.md) 方法即可查询 | 需配置用于接收回调的 HTTP 服务器，并单独开通服务 |
| 需主动查询，无法保证及时性                                   | 事件发生时及时通知                               |
| 仅提供 M3U8 文件名和录制的状态                               | 提供云端录制所有的事件通知和具体信息             |

如需使用回调服务，请参考 [RESTful API 回调](../../cn/cloud-recording/cloud_recording_callback_rest.md)。

## 前提条件

请确保满足以下要求：

- 联系 [sales@agora.io](mailto:sales@agora.io) 开通云端录制服务。
- 开通第三方云存储服务，目前支持七牛云、阿里云（推荐）和 Amazon S3。

## 通过 Basic HTTP 认证

Agora RESTful API 要求 Basic HTTP 认证。每次发送 HTTP 请求时，都必须在请求头部填入 `Authorization`  字段。关于如何生成该字段的值，请参考 [RESTful API 认证](https://docs.agora.io/cn/faq/restful_authentication)。

## 获取 resource ID

调用 [`acquire`](../../cn/cloud-recording/cloud_recording_api_rest.md) 方法请求一个用于云端录制的 resource ID。

调用该方法成功后，你可以从 HTTP 响应包体中的 `resourceId` 字段得到一个 resource ID。这个 resource ID 的时效为 5 分钟，你需要在 5 分钟内用这个 resource ID 开始录制。

> 一个 resource ID 仅可用于一次录制。

该方法的请求和响应示例详见 [`acquire` 示例](https://docs.agora.io/cn/cloud-recording/cloud_recording_api_rest?platform=All%20Platforms#acquire-请求示例)。

## 开始录制

获取 resource ID 后，在五分钟內调用 [`start`](../../cn/cloud-recording/cloud_recording_api_rest.md) 方法开始录制。 

该方法的请求和响应示例详见 [`start` 示例](https://docs.agora.io/cn/cloud-recording/cloud_recording_api_rest?platform=All%20Platforms#start-请求示例)。

## 查询录制状态

录制过程中，你可以多次调用 [`query`](../../cn/cloud-recording/cloud_recording_api_rest.md) 方法查询录制的状态。

调用该方法成功后，你可以从 HTTP 响应包体中获得录制生成的[索引文件](#m3u8)和当前录制的状态。

该方法的请求和响应示例详见 [`query` 示例](https://docs.agora.io/cn/cloud-recording/cloud_recording_api_rest?platform=All%20Platforms#query-请求示例)。

## 停止录制

调用  [`stop`](../../cn/cloud-recording/cloud_recording_api_rest.md) 方法停止录制。

该方法的请求和响应示例详见 [`stop` 示例](https://docs.agora.io/cn/cloud-recording/cloud_recording_api_rest?platform=All%20Platforms#stop-请求示例)。

## 上传和管理录制文件

开始录制后，Agora 服务器会将录制内容切片为多个 TS 文件并不断上传至预先设定的第三方云存储，直至录制结束。

### 录制 ID

录制 ID 是录制的唯一标识，每一次云端录制对应一个录制 ID。

通过 [`start`](../../cn/cloud-recording/cloud_recording_api_rest.md) 请求成功开始录制后，你可以在响应中获取录制 ID（`sid`），所有的回调中也都包含录制 ID。

### <a name="m3u8"></a>录制文件索引

每次录制均会生成一个 M3U8 文件，用于索引该次录制所有的 TS 文件。你可以通过 M3U8 文件播放和管理录制文件。

M3U8 文件名由录制 ID 和频道名组成，如 `sid_cname.M3U8`。

### 上传录制文件

录制文件的上传由 Agora 服务器自动完成，你需要关注下面的回调。

- 录制过程中
  - [`uploading_progress`](../../cn/cloud-recording/cloud_recording_callback_rest.md)：录制开始后约每分钟触发一次，该回调中的 `progress` 参数表示已上传文件占已录制文件的百分比。
- 停止录制后，根据上传情况，SDK 会触发以下回调之一：
  - [`uploaded`](../../cn/cloud-recording/cloud_recording_callback_rest.md)：如果录制文件全部成功上传至预先设定的云存储，最后一个录制切片文件上传完成时触发该回调，通知上传完成。
  - [`backuped`](../../cn/cloud-recording/cloud_recording_callback_rest.md)：如果有录制文件未能成功上传至第三方云存储，Agora 服务器会将这部分文件自动上传至 Agora 云备份，当录制结束后会触发该回调。Agora 云备份会继续尝试将这部分文件上传至设定的第三方云存储。如果等待五分钟后仍然不能正常[播放录制文件](../../cn/cloud-recording/cloud_recording_onlineplay.md)，请联系 Agora 技术支持。
  
## <a name="demo-rest"></a>示例代码

以下为使用 RESTful API 进行云端录制的示例代码（Python），供你参考。

```python
import requests
import time
TIMEOUT=60

#TODO: Fill in AppId, Basic Auth string, Cname (channel name), and cloud storage information
APPID=""
Auth=""
Cname=""
ACCESS_KEY=""
SECRET_KEY=""
VENDOR = 0
REGION = 0
BUCKET = ""

url="https://api.agora.io/v1/apps/%s/cloud_recording/" % APPID

acquire_body={
    "uid": "957947",
    "cname": Cname,
    "clientRequest": {}
}

start_body={
    "uid": "957947",
    "cname": Cname,
    "clientRequest": {
        "storageConfig": {
            "secretKey": SECRET_KEY,
            "region": REGION,
            "accessKey": ACCESS_KEY,
            "bucket": BUCKET,
            "vendor": VENDOR
        },
        "recordingConfig": {
            "audioProfile": 0,
            "channelType": 0,
            "maxIdleTime": 30,
            "transcodingConfig": {
                    "width": 640,
                    "height": 360,
                    "fps": 15,
                    "bitrate": 500
            }
        }
    }
}

stop_body={
    "uid": "957947",
    "cname": Cname,
    "clientRequest": {}
}


def cloud_post(url, data=None,timeout=TIMEOUT):
    headers = {'Content-type': "application/json", "Authorization": Auth}

    try:
        response = requests.post(url, json=data, headers=headers, timeout=timeout, verify=False)
        print("url: %s, request body:%s response: %s" %(url, response.request.body,response.json()))
        return response
    except requests.exceptions.ConnectTimeout:
        raise Exception("CONNECTION_TIMEOUT")
    except requests.exceptions.ConnectionError:
        raise Exception("CONNECTION_ERROR")

def cloud_get(url, timeout=TIMEOUT):
    headers = {'Content-type':"application/json", "Authorization":Auth}
    try:
        response = requests.get(url, headers=headers, timeout=timeout, verify=False)
        print("url: %s,request:%s response: %s" %(url,response.request.body, response.json()))
        return response
    except requests.exceptions.ConnectTimeout:
        raise Exception("CONNECTION_TIMEOUT")
    except requests.exceptions.ConnectionError:
        raise Exception("CONNECTION_ERROR")

def start_record():
    acquire_url = url+"acquire"
    r_acquire = cloud_post(acquire_url, acquire_body)
    if r_acquire.status_code == 200:
        resourceId = r_acquire.json()["resourceId"]
    else:
        print("Acquire error! Code: %s Info: %s" %(r_acquire.status_code, r_acquire.json()))
        return False

    start_url = url+ "resourceid/%s/mode/mix/start" % resourceId
    r_start = cloud_post(start_url, start_body)
    if r_start.status_code == 200:
        sid = r_start.json()["sid"]
    else:
        print("Start error! Code:%s Info:%s" %(r_start.status_code, r_start.json()))
        return False

    time.sleep(10)
    query_url = url + "resourceid/%s/sid/%s/mode/mix/query" %(resourceId, sid)
    r_query = cloud_get(query_url)
    if r_query.status_code == 200:
        print("The recording status: %s" % r_query.json())
    else:
        print("Query failed. Code %s, info: %s" % (r_query.status_code, r_query.json()))

    time.sleep(30)
    stop_url = url+"resourceid/%s/sid/%s/mode/mix/stop" % (resourceId, sid)
    r_stop = cloud_post(stop_url, stop_body)
    if r_stop.status_code == 200:
        print("Stop cloud recording success. FileList : %s, uploading status: %s"
              %(r_stop.json()["serverResponse"]["fileList"], r_stop.json()["serverResponse"]["uploadingStatus"]))
    else:
        print("Stop failed! Code: %s Info: %s" % r_stop.status_code, r_stop.json())

start_record()
```
