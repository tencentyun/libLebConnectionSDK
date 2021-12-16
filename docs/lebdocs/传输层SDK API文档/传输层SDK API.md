# 传输层SDK API

### 1、  主要功能

​		音视频拉流，兼具优异的低延迟性能和抗弱网能力

​		视频支持H264和H265，支持B帧，视频输出格式为AnnexB视频帧裸数据

​		音频支持AAC和OPUS，音频输出格式为不含头信息的音频帧裸数据

​		支持Android、IOS、Windows和Linux平台

 

### 2、  基础接口

#### 2.1    创建快直播连接

`LEB_EXPORT_API LebConnectionHandle* OpenLebConnection(void* context, LebLogLevel loglevel);`

 

#### 2.2    注册回调函数

`LEB_EXPORT_API void RegisterLebCallback(LebConnectionHandle* handle, const LebCallback* callback);`

 

#### 2.3    开始连接拉流

`LEB_EXPORT_API void StartLebConnection(LebConnectionHandle* handle, LebConfig config);`

 

#### 2.4    停止连接

`LEB_EXPORT_API void StopLebConnection(LebConnectionHandle* handle);`

 

#### 2.5    关闭连接

`LEB_EXPORT_API void CloseLebConnection(LebConnectionHandle* handle);`

 

### 3、回调接口

```
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

Notes: 详细数据结构定义请见头文件leb_connection_api.h



### 4、接口调用流程

<img src="picture/api.jpg" alt="p1" width="300px" />