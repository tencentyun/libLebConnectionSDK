### v2.0.1 @ 2023.03.17

- 增强信令鲁棒性和性能
- 支持answer video头信息解析
- 支持动态音视频
- 增加配置接口
  - open timout设置
  - minisdp开关
  - jitterbuffer开关
- 其他传输优化

### v2.0.0 @ 2022.12.22

- 增加SDK内部播控模式
  - 外部设置音频解码callback
  - 内部执行音频解码，输出PCM S16
  - 内部播速控制
- Minisdp更新
- 传输优化，提升抗弱网能力

### v1.0.3.4 @ 2022.09.26

- 支持TCP transport
- 升级minisdp
- 优化回源模式
- 优化音视频数据输出

### v1.0.3.3 @ 2022.06.22
- 增加video jitterbuffer
- 支持video fec
- 增加win64版本
- 修复已知问题

### v1.0.3.2 @ 2022.03.30
- 支持多码率动态切换
- 支持视频AV1拉流
- 支持SDK上报
- 增加音视频同步输出优化
- 改进webrtc demuxer
- 增加Anroid ijkplayer体验apk

### v1.0.3.1 @ 2021.12.01
- 缩减包大小
- 增加mac版本
- 增加android x86和x64版本
- 增加ios x86版本
- sdk优化
- 问题修复

### v1.0.2.4 @ 2021.10.19
- sdk优化
- 问题修复

### v1.0.2.3 @ 2021.09.10
- sdk优化
- 增加linux版本
- webrtc demuxer实现优化

### v1.0.2.2 @ 2021.07.30
- sdk接口优化
- 增加error code定义和出错回调
- webrtc demuxer实现优化

### v1.0.2.1 @ 2021.07.01
- 视频h264、h265拉流
- 音频aac、opus拉流
