## OBS WebRTC 协议推流接入

WebRTC 协议推流主要用于视频云的快直播（超低延迟直播）推流，负责将采集的音视频画面或者视频文件通过 WebRTC 协议推送到直播服务器。下述内容主要介绍如何使用 OBS 工具，实现 webRTC 协议推流功能。



## For Windows

##### 注意事项

目前对 OBS 的版本要求在26版本或版本以上。



### 配置 OBS 插件

1. ##### 配置插件数据

   下载 [OBS 插件](https://mediacloud-76607.gzc.vod.tencent-cloud.com/TOBSWebRTC/Release/tencent_webrtc_plugin_20210628.zip)，把 data 文件里面的两个 `services.json` 和 `package.json` 文件，挪动到对应的 **obs-studio** > **rtmp-service** > **data** 目录进行覆盖。（`obs-studio` 默认安装在 C 盘，对应的目录为：`C:\Program Files\obs-studio\data\obs-plugins\rtmp-services`，请根据您的实际情况进行配置。）

   ![img](https://main.qcloudimg.com/raw/03859054448cb140d31f2a57a60d82aa.png)

2. ##### 配置插件动态库

   将 `obs-plugins\64bit` 中的 dll 和 pdb 文件，挪动到对应的 **obs-studio** > **obs-plugins** > **64bit** 目录下。（`obs-studio` 默认安装在 C 盘，对应的目录为：`C:\Program Files\obs-studio\obs-plugins\64bit`，请根据您的实际情况进行配置。）
   
   ![img](https://main.qcloudimg.com/raw/0384bd8ebe63704fdb306a0620124ebf.png)



### 配置推流链接



1. ##### 生成 WebRTC 推流地址

   

   1. 登录腾讯云直播控制台，在 **直播工具箱**>**[地址生成器](https://console.cloud.tencent.com/live/addrgenerator/addrgenerator)** 生成推流地址，具体操作请参见 [地址生成器](https://cloud.tencent.com/document/product/267/35257)。

   2. 把生成的 `rtmp` 前缀修改成 `webrtc`，具体使用说明请参见 [自主拼装直播 URL](https://cloud.tencent.com/document/product/267/32720)。

      ![img](https://main.qcloudimg.com/raw/34924378812d1a36f04cfe1a2180e7a0.png)

   

2. ##### 配置 OBS 推流服务

   

   2.1 打开 OBS，您可通过底部工具栏的 **控件**>**设置** 按钮进入设置界面。

   2.2 单击**推流**进入流设置页签，选择服务类型为`Tenent webrtc`，服务器为`Default`，串流密钥中输入上述步骤1中生成的WebRTC 推流地址，并在后面拼接上`&stopstream_api=https://webrtcpush.myqcloud.com/webrtc/v1/stopstream`。

   串流密钥示例：

   ```
   webrtc://domain/AppName/StreamName?txSecret=xxx&txTime=xxx &stopstream_api=https://webrtcpush.myqcloud.com/webrtc/v1/stopstream 
   ```

   如下图：

   ![img](https://main.qcloudimg.com/raw/5c33acc958da82c01127ba2d4575ce1e.png)



## For Mac

//TODO