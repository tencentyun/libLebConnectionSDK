# 快直播传输层SDK - libLebConnectionSDK

  
## 1. 主要功能
  1. 音视频拉流，兼具优异的低延迟性能和抗弱网能力
  2. 视频支持H264和H265，支持B帧，视频输出格式为AnnexB
  3. 音频支持AAC和OPUS，音频输出格式为不含头信息的音频帧裸数据
  4. 支持Android、IOS和Windows平台


## 2. SDK接口调用流程
  ![image](https://github.com/tencentyun/libLebConnectionSDK/blob/main/docs/api_calling_sequence.png)

## 3. SDK接口说明
- 创建快直播连接
```
WEBRTC_EXPORT_API LebConnectionHandle* OpenLebConnection(void* context, LebLogLevel loglevel);
```
- 开始连接，内部完成信令后，直接建联拉流
```
WEBRTC_EXPORT_API void StartLebConnection(LebConnectionHandle* handle, LebConfig config);
```
- 开始信令，回调onOfferCreated输出offer sdp，通过http交互获取answer sdp
```
WEBRTC_EXPORT_API void CreateOffer(LebConnectionHandle* handle, LebConfig config);
```
- 设置获取的answer sdp，开始建联拉流
```
WEBRTC_EXPORT_API void SetRemoteSDP(LebConnectionHandle* handle, LebSdpInfo info);
```
- 停止连接
```
WEBRTC_EXPORT_API void StopLebConnection(LebConnectionHandle* handle);
```
- 关闭连接
```
WEBRTC_EXPORT_API void CloseLebConnection(LebConnectionHandle* handle);
```
- 播放过程中查询统计数据，onStatsInfo异步回调输出
```
WEBRTC_EXPORT_API void GetStats(LebConnectionHandle* handle);
```
- 注册日志回调函数
```
WEBRTC_EXPORT_API void RegisterLogInfoCallback(LebConnectionHandle* handle, OnLogInfo callback);
```
- 注册sdp回调函数，获取offer sdp
```
WEBRTC_EXPORT_API void RegisterSdpInfoCallback(LebConnectionHandle* handle, OnSdpInfo callback);
```
- 注册视频信息回调函数，获取视频信息
```
WEBRTC_EXPORT_API void RegisterVideoInfoCallback(LebConnectionHandle* handle, OnVideoInfo callback);
```
- 注册音频信息回调函数，获取音频信息
```
WEBRTC_EXPORT_API void RegisterAudioInfoCallback(LebConnectionHandle* handle, OnAudioInfo callback);
```
- 注册视频数据回调函数，获取视频帧裸数据
```
WEBRTC_EXPORT_API void RegisterVideoDataCallback(LebConnectionHandle* handle, OnEncodedVideo callback);
```
- 注册音频数据回调函数，获取音频帧裸数据
```
WEBRTC_EXPORT_API void RegisterAudioDataCallback(LebConnectionHandle* handle, OnEncodedAudio callback);
```
- 注册MetaData回调函数，获取MetaData数据
```
WEBRTC_EXPORT_API void RegisterMetaDataCallback(LebConnectionHandle* handle, OnMetaData callback);
```
- 注册统计回调函数，获取统计数据
```
WEBRTC_EXPORT_API void RegisterStatsInfoCallback(LebConnectionHandle* handle, OnStatsInfo callback);
```
详细数据结构定义请见头文件leb_connection_api.h

### 4. 播放器集成示例
  ![image](https://github.com/tencentyun/libLebConnectionSDK/blob/main/docs/player_framework.png)

