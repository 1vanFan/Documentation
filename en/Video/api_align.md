
---
title: Summary of Core APIs
description: List APIs of the key functions across the platforms
platform: All Platforms
updatedAt: Wed Dec 12 2018 23:23:19 GMT+0000 (UTC)
---
# Summary of Core APIs
Agora SDK supports multiple platforms, but the implementations of the APIs are not exactly the same across platforms. The table below lists the core APIs on Android, iOS/macOS, Web, and Windows to help you quickly understand the differences.

<table>
  <tr>
    <th>Core Service</th>
    <th>Android</th>
    <th>iOS/macOS</th>
    <th>Web</th>
    <th>Windows</th>
  </tr>
  <tr>
    <td>Initialize</td>
    <td><a href="https://docs.agora.io/en/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a35466f690d0a9332f24ea8280021d5ed">create</a></td>
    <td><a href="https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/sharedEngineWithAppId:delegate:">sharedEngineWithAppId</a></td>
    <td><a href="https://docs.agora.io/en/Video/API%20Reference/web/globals.html#createclient">AgoraRTC.createClient</a><br><a href="https://docs.agora.io/en/Video/API%20Reference/web/interfaces/agorartc.client.html#init">Client.init</a></td>
    <td><a href="https://docs.agora.io/en/Video/API%20Reference/cpp/group__create_agora_rtc_engine.html">createAgoraRtcEngine</a><br><a href="https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#ac71db65e66942e4e0a0550e95c16890f">initialize</a></td>
  </tr>
  <tr>
    <td nowrap="nowrap">Set Channel Profile</td>
    <td><a href="https://docs.agora.io/en/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a1bfb76eb4365b8b97648c3d1b69f2bd6">setChannelProfile</a></td>
    <td><a href="https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setChannelProfile:">setChannelProfile</a></td>
    <td><a href="https://docs.agora.io/en/Video/API%20Reference/web/globals.html#createclient">AgoraRTC.createClient</a><sup>[1]</sup></td>
    <td><a href="https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#aab53788c74da25080bad61f0525d12ae">setChannelProfile</a></td>
  </tr>
  <tr>
    <td>Set Client Role</td>
    <td><a href="https://docs.agora.io/en/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#aa2affa28a23d44d18b6889fba03f47ec">setClientRole</a></td>
    <td><a href="https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/setClientRole:">setClientRole</a></td>
    <td>/<sup>[2]</sup></td>
    <td><a href="https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a89ca6a15d5a388f3c82038e74bad4040">setClientRole</a></td>
  </tr>
  <tr>
    <td>Join Channel</td>
    <td><a href="https://docs.agora.io/en/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a8b308c9102c08cb8dafb4672af1a3b4c">joinChannel</a></td>
    <td><a href="https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/joinChannelByToken:channelId:info:uid:joinSuccess:">joinChannelByToken</a></td>
    <td><a href="https://docs.agora.io/en/Video/API%20Reference/web/interfaces/agorartc.client.html#join">Client.join</a></td>
    <td><a href="https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#adc937172e59bd2695ea171553a88188c">joinChannel</a></td>
  </tr>
  <tr>
    <td>Leave Channel</td>
    <td><a href="https://docs.agora.io/en/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a2929e4a46d5342b68d0deb552c29d597">leaveChannel</a></td>
    <td><a href="https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/leaveChannel:">leaveChannel</a></td>
    <td><a href="https://docs.agora.io/en/Video/API%20Reference/web/interfaces/agorartc.client.html#leave">Client.leave</a></td>
    <td><a href="https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a51c12d209373650638bfd82e28777081">leaveChannel</a></td>
  </tr>
  <tr>
    <td>Renew Token</td>
    <td><a href="https://docs.agora.io/en/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#af1428905e5778a9ca209f64592b5bf80">renewToken</a></td>
    <td><a href="https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/renewToken:">renewToken</a></td>
    <td><a href="https://docs.agora.io/en/Video/API%20Reference/web/interfaces/agorartc.client.html#renewtoken">Client.renewToken</a></td>
    <td><a href="https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a8f25b5ff97e2a070a69102e379295739">renewToken</a></td>
  </tr>
  <tr>
    <td>Enable Interoperability</td>
    <td><a href="https://docs.agora.io/en/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#a49636ee063476d7c3da533668771fa03">enableWebSdkInteroperability</a></td>
    <td><a href="https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/enableWebSdkInteroperability:">enableWebSdkInteroperability</a></td>
    <td>N/A<sup>[3]</sup></td>
    <td><a href="https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a5b82667e75a8f299a60b9b7968da48de">enableWebSdkInteroperability</a></td>
  </tr>
  <tr>
    <td>Destroy Instance</td>
    <td><a href="https://docs.agora.io/en/Video/API%20Reference/java/classio_1_1agora_1_1rtc_1_1_rtc_engine.html#afb808cdc9025a77af7dd2bce98311bfe">destroy</a></td>
    <td><a href="https://docs.agora.io/en/Video/API%20Reference/oc/Classes/AgoraRtcEngineKit.html#//api/name/destroy">destroy</a></td>
    <td>N/A</td>
    <td><a href="https://docs.agora.io/en/Video/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#afe4804c1f53bfee301c0960fda006c47">release</a></td>
  </tr>
</table>

> [1] Agora Web SDK sets the channel profile by `ClientConfig` in `AgoraRTC.createClient` , see [ClientConfig](https://docs.agora.io/en/Video/API%20Reference/web/interfaces/agorartc.clientconfig.html) for details.

> [2] Currently Agora Web SDK does not have a corresponding method to set the client role, but you can use the `Client.publish`, `Client.unpublish`, and `Client.subscribe` methods to switch roles between host and audience, see [Swtich the Client Role](../../en/Video/role_web.md) for details.

> [3] To communicate with other platforms, set the `mode` parameter as `live` in [ClientConfig](https://docs.agora.io/en/Video/API%20Reference/web/interfaces/agorartc.clientconfig.html) for the Web SDK.
