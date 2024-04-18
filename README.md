# 快直播传输层SDK - libLebConnectionSDK


## 1. 简介
  快直播传输层SDK提供基于原生WebRTC升级扩展的传输能力，用户仅需对已有播放器进行简单改造，即可接入快直播。在完全兼容标准直播的推流、云端媒体处理能力的基础上，实现高并发低延迟直播，帮助用户实现从现有的标准直播平滑地迁移到快直播上来。也可以帮助用户在现有RTC场景中快速实现低成本的大房间低延迟旁路直播。

  快直播传输层SDK主要功能:
  - 音视频拉流，兼具优异的低延迟性能和抗弱网能力
  - 视频支持H.264、H.265、H.266和AV1，支持B帧，视频输出格式为视频帧裸数据(H.264/H.265/H.266为AnnexB，AV1为OBU)
  - 音频支持AAC和OPUS，音频输出格式为音频帧裸数据(启用内部播控时音频输出为PCM S16)
  - 支持Android、iOS、Windows、Linux和Mac平台


## 2. SDK接口调用流程

  ![image](https://video.sdk.qcloudecdn.com/lebsdk/api_calling_sequence.png)

## 3. SDK接口说明
### 3.1 基础接口说明
- 创建快直播连接
```c
LEB_EXPORT_API LebConnectionHandle* OpenLebConnection(void* context, LebLogLevel loglevel);
```
- 注册回调函数
```c
LEB_EXPORT_API void RegisterLebCallback(LebConnectionHandle* handle, const LebCallback* callback);
```
- 开始连接拉流
```c
LEB_EXPORT_API void StartLebConnection(LebConnectionHandle* handle, LebConfig config);
```
- 停止连接
```c
LEB_EXPORT_API void StopLebConnection(LebConnectionHandle* handle);
```
- 关闭连接
```c
LEB_EXPORT_API void CloseLebConnection(LebConnectionHandle* handle);
```

### 3.2 回调接口说明
```c
typedef struct LebCallback {
  // 日志回调
  OnLogInfo onLogInfo;
  // 视频信息回调
  OnVideoInfo onVideoInfo;
  // 音频信息回调
  OnAudioInfo onAudioInfo;
  // 视频数据回调
  OnEncodedVideo onEncodedVideo;
  // 音频数据回调
  OnEncodedAudio onEncodedAudio;
  // MetaData回调
  OnMetaData onMetaData;
  // 统计信息回调
  OnStatsInfo onStatsInfo;
  // 错误回调
  OnError onError;
} LebCallback;
```
Notes：详细数据结构定义请见头文件**leb_connection_api.h**

### 4. FFmpeg集成
SDK提供FFmpeg webrtc demuxer源码：webrtc_demuxer.c，可以实现FFmpeg快直播拉流和媒体处理，以及ffplay播放

快直播FFmpeg配置编译可以参考：
[linux_build_guide](https://github.com/feiwei9696/FFmpeg/blob/release/5.0_webrtc/libLebConnection/build_guide_linux.md),
[mac_build_guide](https://github.com/feiwei9696/FFmpeg/blob/release/5.0_webrtc/libLebConnection/build_guide_mac.md)

### 5. 播放器集成示例

- 外部播控
  ![image](https://video.sdk.qcloudecdn.com/lebsdk/player_framework.png)
    具体可以参考：[基于ijkplayer的快直播传输层SDK应用实践](https://mp.weixin.qq.com/s/f3ct29ydzAjdJ1fIdOmHmQ)
- 内部播控
  ![image](https://video.sdk.qcloudecdn.com/lebsdk/player_framework2.png)

### 6. 其他 

[WebRTC Web Demo体验](https://mps.live/demo/encoding/webrtc)

