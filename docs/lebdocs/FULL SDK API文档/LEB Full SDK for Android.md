# 快直播LEB Full SDK for Android



## 1. SDK简介
    快直播（Live Event Broadingcasting）腾讯基于WebRTC技术的超低延时直播，通过LEB Android SDK，接入商只需对接几个接口，快速实现Android平台实现接入和播放。



## 2. SDK集成接入

demo: https://github.com/tencentyun/leb-android-sdk/tree/master/app

     注意：本demo只演示了快直播的拉流和停流的流程，没有实现其他业务和app本身的逻辑，比如：
     1. Surface相关，前后台切换、全屏缩放、屏幕旋转等逻辑。
     2. Audio相关，音频设备检测、请求和释放音频焦点等。
     3. 播放相关，完善pause、resume、stop等相关逻辑。



### 2.1 jcenter接入方式

    SDK提供两种⽅方式接⼊入: jcenter 和 AAR ，可以根据需要选择接⼊入⽅方式，分别如下：
    首先，在app的最上层 build.gradle 中加⼊入 jcenter的仓库依赖
    buildscript {
        repositories {
            jcenter()
        }
    }
    然后，在相关module的build.gradle中加⼊入依赖
    dependencies {
        implementation 'com.tencent.xbright:lebwebrtcsdk:2.0.5'
    }



### 2.2 SO库的ABI说明

    SDK内包含SO库，⽬前仅⽀支持 armeabi-v7a、 arm64-v8a 两种ABI架构。



### 2.3 权限

```xml
需要配置⼀一些必要的权限才能正常运⾏行行，请确认app的 AndroidManifest.xml 中添加了了如下权限:
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
```



### 2.4 渲染硬件加速

```xml
如需提⾼高显示渲染效率，可以在 AndroidManifest.xml 中将硬件加速打开
<application android:hardwareAccelerated="true">
```



## 3.SDK接口



### 3.1 接口介绍

**SDK封装类LEBWebRTCView**

| 接口名称                                                     | 接口描述         |
| ------------------------------------------------------------ | ---------------- |
| getContext()                                                 | 获取context      |
| initilize(@NonNull LEBWebRTCParameters rtcParams, @NonNull LEBWebRTCEvents rtcEvents) | 初始化sdk        |
| setRemoteSDP(String sdp)                                     | 设置remote sdp   |
| startPlay()                                                  | 开始播放         |
| pausePlay()                                                  | 暂停播放         |
| stopPlay()                                                   | 停止播放         |
| release()                                                    | 释放资源         |
| mutePlay(boolean isMute)                                     | 静音播放         |
| setVolume(double volume)                                     | 设置PCM音量增益  |
| setVideoRotation(int degree)                                 | 设置画面旋转角度 |
| setScaleType(int scaleType)                                  | 设置缩放模式     |
| takeSnapshot                                                 | 截图接口         |
| interface SnapshotListener {<br/>        void onSnapshot(Bitmap bitmap);<br/>} | 截图回调         |



### 3.2 初始化

```xml
LEBWebRTCView mWebRTCView = findViewById(R.id.id_surface_view);

mWebRTCView.initilize(LEBWebRTCParameters rtcParams, LEBWebRTCEvents rtcEvents)

LEBWebRTCParameters为配置参数，具体见下文
LEBWebRTCEvents为事件回调，具体见下文

LEBWbRTCView构造见下面示例：
LEBWebRTCView mWebRTCView = findViewById(R.id.id_surface_view);
R.id.id_surface_view：
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/id_framelayout_cg"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:keepScreenOn="true">
    <com.tencent.xbright.lebwebrtcsdk.LEBWebRTCSurfaceView
        android:id="@+id/id_surface_view"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center" />
</FrameLayout>
```



LEBWebRTCParameters定义如下：

```java
public class LEBWebRTCParameters {

// 快直播播放地址
private String mStreamUrl;
// 是否使用硬件解码
private boolean mEnableHwDecode = true;
// 音频格式
public static final int OPUS = 0x01;
public static final int AAC_LATM = 0x02;
public static final int AAC_ADTS = 0x04;
private int mAudioFormat = OPUS;
// 是否关闭加密传输
private boolean mDisableEncryption = false;
// 是否启用SEI回调
private boolean mEnableSEICallback = false;
// 播放状态回调周期
private int mStatsReportPeriodInMs = 1000;
// WebRTC日志级别
public static final int LOG_VERBOSE = 0x00;
public static final int LOG_INFO    = 0x01;
public static final int LOG_WARNING = 0x02;
public static final int LOG_ERROR   = 0x03;
public static final int LOG_NONE    = 0x04;
private int mLoggingSeverity = LOG_NONE;
// WebRTC连接超时
private int mConnectoionTimeoutInMs = 5000;//ms

// 音频PCM增益，0~10.0，增益过大会使PCM过饱和
private double mDefaultVolume = 1.0f;
// 音频最大队列，会影响最大延时
private int mAudioJitterBufferMaxPackets = 50;
// 是否开启追帧功能
private boolean mEnableAudioJitterBufferFastAccelerate = true;
// 是否拉音频流
private boolean mReceiveAudio = true;
// 是否拉视频流
private boolean mReceiveVideo = true;
...
}
```
LEBWebRTCParameters构造见下面示例：

```java
//创建参数对象
LEBWebRTCParameters mLEBWebRTCParameters = new LEBWebRTCParameters();
//设置播放码流链接, webrtc://xxxxx
mLEBWebRTCParameters.setStreamUrl(mWebRTCUrl);
//设置是否硬解，默认为硬解
mLEBWebRTCParameters.enableHwDecode(mEnableHwDecode);
//设置连接超时时间，默认为5000ms
mLEBWebRTCParameters.setConnectionTimeOutInMs(5000);
//设置播放状态回调事件周期，默认为1000ms
mLEBWebRTCParameters.setStatsReportPeriodInMs(1000);
//设置是否关闭加密，默认为打开加密
mLEBWebRTCParameters.disableEncryption(mDisableEncryption);
//设置是否启用SEI回调，默认为关闭
mLEBWebRTCParameters.enableSEICallback(mEnableSEICallback);
//设置最小JitterDelay，默认为1000ms，越大越能对抗网络抖动，按场景需要设置
mLEBWebRTCParameters.setMinJitterDelayMs(1000);
//设置是否开启追帧功能，默认开启
mLEBWebRTCParameters.enableAudioJitterBufferFastAccelerate(true);
//设置是否拉音频流，默认开启
mLEBWebRTCParameters.enableReceiveAudio(true);
//设置是否拉视频流，默认开启
mLEBWebRTCParameters.enableReceiveVideo(true);
//设置拉流音频格式，LEBWebRTCParameters.OPUS, LEBWebRTCParameters.AAC_LATM, LEBWebRTCParameters.AAC_ADTS
mLEBWebRTCParameters.setAudioFormat(mAudioFormat);
//设置日志级别，默认为LOG_NONE
mLEBWebRTCParameters.setLoggingSeverity(LEBWebRTCParameters.LOG_NONE);
//设置日志回调
mLEBWebRTCParameters.setLoggable((String tag, int level, String message) -> {
    final String t = "[lebwebrtc]" + tag;
    switch (level) {
        case LEBWebRTCParameters.LOG_VERBOSE:
            Log.v(t, message);
            break;
        case LEBWebRTCParameters.LOG_INFO:
            Log.i(t, message);
            break;
        case LEBWebRTCParameters.LOG_WARNING:
            Log.w(t, message);
            break;
        case LEBWebRTCParameters.LOG_ERROR:
            Log.e(t, message);
            break;
        default:
            Log.i(t, message);
            break;
    }
});

LEBWebRTCEvents事件回调定义如下：
public interface LEBWebRTCEvents {
    enum ConnectionState
    {
        // 开始建立连接
        STATE_BEGIN,
        // OFFER创建
        STATE_OFFER_CREATED,
        // ICE完成
        STATE_ICE_COMPLETED,
        // 连接建立
        STATE_WEBRTC_CONNECTED,
        // 渲染第一帧
        STATE_FIRST_FRAME_RENDERED,
        // 连接超时
        STATE_WEBRTC_TIMEOUT,
    }
    // offer创建成功
    void onEventOfferCreated(String sdp);
    // 连接成功
    void onEventConnected();
    // 连接失败
    void onEventConnectFailed(ConnectionState cs);
    // 连接断开
    void onEventDisconnect();
    // 收到首包数据
    void onEventFirstPacketReceived(int mediType);//0:audio, 1:video
    // 渲染首帧
    void onEventFirstFrameRendered();
    // 分辨率切换
    void onEventResolutionChanged(int width, int height);
    // 统计数据
    void onEventStatsReport(LEBWebRTCStatsReport webRTCStatsReport);
    // sei回调，解码线程，不要阻塞，没有start code
    void onEventSEIReceived(ByteBuffer data);
}
其中onEventStatsReport(LEBWebRTCStatsReport webRTCStatsReport)用来回调播放状态，包含音视频播放性能、播放帧率、码率和时长等数据，LEBWebRTCStatsReport定义如下:

public class LEBWebRTCStatsReport {
    //video stats
    public long   mFirstVideoPacketDelayMs;//从启动到收到第一包视频数据的延时
    public long   mFirstFrameRenderDelayMs; //从启动到首帧渲染延时
    public float  mVideoDecodeFps; //当前解码帧率
    public float  mVideoDecoderAvgFps;//平均帧率
    public float  mVideoRenderFps;      // 当前视频渲染帧率
    public long   mVideoRenderReceived; // 视频渲染收到的帧数
    public long   mVideoRenderDropped;  // 渲染时丢弃的帧数
    public long   mTotalFrozenTimeMs; // 总卡顿时长
    public float  mFrozenRate; // 总卡顿时长/播放时长
    public long   mVideoBitrate; //视频码率
    public long   mFramesDecoded; //解码帧数
    public long   mFramesDropped; //丢帧数
    public long   mFramesReceived; //接收帧数
    public int    mVideoPacketsLost; //丢包个数
    public long   mVideoPacketsReceived; //接收包数
    public long   mFrameWidth; //视频宽度
    public long   mFrameHeight; //视频高度
    public long   mVideoDelayMs;
    public long   mVideoJitterBufferDelayMs;
    public long   mVideoNacksSent;
    public long   mRTT;

    //audio stats
    public long   mFirstAudioPacketDelayMs;//从启动到收到第一包音频数据的延时
    public int    mAudioPacketsLost; //丢包个数
    public long   mAudioPacketsReceived; //接收包数
    public long   mAudioBitrate;//音频码率
    public long   mAudioDelayMs;
    public long   mAudioJitterBufferDelayMs;
    public long   mAudioNacksSent;

    //play stats
    public long   mAverageBitRate;//平均码率
    public long   mPlayTimeMs;//播放时长
}
```


​    


### 3.2 启动SDK
    初始化后开始启动过程，步骤如下
#### 1. 开始启动sdk，sdk在p2p未连接时会创建offer(local sdp)
```java
void startPlay() 
```
#### 2. Offer创建成功后回调，用户可以回调中实现向信令服务器发送offer，获取remote sdp，并设置给SDK （信令请求具体见下文）
```java
void onEventOfferCreated(String sdp)
```
#### 3. 将remote sdp设置给SDK，sdk会发起p2p连接，连接成功后开始播放
```java
void setRemoteSDP(String sdp)
```



### 3.3 暂停播放

```java
//暂停播放，保持连接
void pausePlay()
```



### 3.4 继续播放

```java
//在暂停后恢复播放
void startPlay() 
```



### 3.5 退出播放

```java
退出播放需先向信令服务器发出请求（具体见下文）再本地执行下面命令
//退出播放、并断开连接
void stopPlay()
```



### 3.6 释放资源

```java
//释放SDK相关资源
void release() 
```



### 3.7 静音播放

```java
//设置静音播放
void mutePlay(boolen isMute)
```



### 3.8 设置音频PCM增益，0~10.0，默认为1.0

```java
注意：增益过大会使PCM过饱和
void setVolume(double volume)
```



### 3.9 截取视频内容

```java
// 通过SnapshotListener回调输出bitmap，scale会截图缩放比例
void takeSnapshot(@NonNull SnapshotListener listener, float scale)

SnapshotListener 定义如下：
public interface SnapshotListener {
    void onSnapshot(Bitmap bitmap);
}
```



### 3.10 设置显示旋转角度，可选：90，180，270

```java
void setVideoRotation(int degree)
```



### 3.11 显示缩放模式, 等比例缩放和平铺缩放

```java
// scaleType 
//SCALE_KEEP_ASPECT_FIT=0:保留视频宽高比
//SCALE_IGNORE_ASPECT_FILL=1:填充view，不保留宽高比
void setScaleType(int scaleType)
```



## 4. http信令

http信令包括，拉流和停流，由用户在app侧实现，具体定义请见[《signal_http_protoc》](files/leb_signal_spec.pdf)

    //请求拉流
    https://webrtc.liveplay.myqcloud.com/webrtc/v1/pullstream
    //停止拉流
    https://webrtc.liveplay.myqcloud.com/webrtc/v1/stopstream



### 4.1 拉流

```json
//请求 
{
    streamurl:  string //拉流地址
    sessionid:  string //业务生成的唯一key，标识本次拉流
    clientinfo: string //终端类型信息
    localsdp: {
        type: string
        sdp:  string
    }
    clientip:   string //客户端IP
    seipass:    int    //是否带SEI
}

//返回
{
    errcode:  int
    errmsg:   string
    remotesdp: {
        type: string
        sdp:  string
    }
    svrsig: string           //服务器签名，后面每个请求必须携带这个字段内容,业务无需理解字段内容
}
```



### 4.2 停流

```json
//请求
{
    streamurl:  string //拉流地址
    svrsig:     string //pullstream返回的服务器签名
}

//返回
{
    errcode: int
    errmsg:  string
}

停止拉流必须调用本接口通知后台停止拉流，否则后台会在数据通道超时断开前继续下发音视频数据，残留的UDP通道既浪费后台资源，也会影响业务并发和计费带宽统计
```



## 5. bugly接入
建议app接入bugly来监控运行质量，具体见bugly官方文档https://bugly.qq.com/docs/user-guide/instruction-manual-android/?v=20200622202242



### 5.1 build.gradle添加

    dependencies {
        compile 'com.tencent.bugly:crashreport:latest.release' //其中latest.release指代最新Bugly SDK版本号，也可以指定明确的版本号，例如2.1.9
        compile 'com.tencent.bugly:nativecrashreport:latest.release' //其中latest.release指代最新Bugly NDK版本号，也可以指定明确的版本号，例如3.0
    }



### 5.2 bugly初始化示例

```java
CrashReport.UserStrategy strategy = new CrashReport.UserStrategy(getApplicationContext());
strategy.setAppChannel("myChannel");  //设置渠道
strategy.setAppVersion("2.0.5");      //App的版本, 这里设置了SDK version
strategy.setAppPackageName("com.tencent.xbright.lebwebrtcdemo");  //App的包名
CrashReport.initCrashReport(getApplicationContext(), "e3243444c9", false, strategy);
```



## 6. 标准WebRTC扩展

快直播SDK和后台扩展了标准WebRTC，支持AAC、H265、B帧和关闭加密，其中使用AAC、H265和B帧的拉流需要后台配置拉流域名







