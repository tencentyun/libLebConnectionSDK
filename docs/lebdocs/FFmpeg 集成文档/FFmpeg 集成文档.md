# 快直播传输层SDK FFmpeg 集成说明



### 下载FFmpeg

1. `git clone https://github.com/FFmpeg/FFmpeg.git`

2. `cd FFmpeg`

3. `git checkout release/4.2`

4. `git pull`



### 下载FFmpeg适配patch

1. `wget https://recorder-10018504.cos.ap-shanghai.myqcloud.com/feiwei/libLebConnection/0001-integrate-libLebConnection-to-FFmpeg-by-adding-webrtc.patch`

2. `git am 0001-integrate-libLebConnection-to-FFmpeg-by-adding-webrtc.patch`

​      

### 下快直播传输层SDK

1. 访问 https://github.com/tencentyun/libLebConnectionSDK/tree/main/libs 下载最新版本SDK

2. `unzip -d libLebConnection libLebConnection_linux_v1.0.2_twebrtc14b8a7f1b2_tffmpegbabcf62544_2021-10-19-10-49.zip`

3. 记SDK目录路径为：`/xxxx/libLebConnection`



### 配置库文件地址

1. `export LD_LIBRARY_PATH=/xxxx/libLebConnection/libs/x64:$LD_LIBRARY_PATH`

或者直接把路径添加到`/etc/ld.so.conf`里



### 配置和编译FFmpeg

1. 进入FFmpeg文件夹

2. `./configure --enable-libLebConnection --enable-pic --enable-gpl --enable-nonfree --disable-doc --extra-cflags=-I/xxxx/libLebConnection/include/ --extra-ldflags=-L/xxxx/libLebConnection/libs/x64/  --extra-libs=-lLebConnection_so`

3. `make && make install`

4. 测试webrtc拉流

`./ffmpeg -i "webrtc://5664.liveplay.myqcloud.com/live/5664_harchar"  -f null /dev/null`