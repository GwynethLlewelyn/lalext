# LAL v0.35.4发布，OBS支持RTMP H265推流，我跟了

Go语言流媒体开源项目 [LAL](https://github.com/q191201771/lal) 今天发布了v0.35.4版本。

> LAL 项目地址：https://github.com/q191201771/lal

老规矩，简单介绍一下：

### ▦ 一. OBS支持RTMP H265推流

新出的标准，一般被称为enhanced RTMP，OBS新版(29.1+版本，[点我去下载安装包](https://github.com/obsproject/obs-studio/releases))已经实现可以使用，LAL也做了相应的适配，换言之，你可以使用OBS推送H265的RTMP流给LAL了。

Tips: 使用H265的好处是相较于H264，占用更低的码率却拥有更高的图像质量。

值得注意的几点细节：

1. LAL转换为其他封装协议（比如RTSP、HLS）后，输出的协议依然是标准协议，这点和之前没啥区别。
2. LAL输出是RTMP/FLV时，依然保持enhanced RTMP格式，目前适配了enhanced RTMP的播放器只有mpegts.js，后续ffmpeg和VLC等播放器应该也会很快跟上支持。
3. enhanced RTMP和之前金山云搞的HEVC over RTMP（就是codecid=12那个，国内大部分CDN产商都支持了）是两套东西，LAL两种格式都支持了，目前的策略是进来的RTMP/FLV是啥格式，出去的RTMP/FLV就是啥格式，后续可能会考虑转换（比如通过配置或url参数等）。

关于enhanced RTMP协议实现细节、OBS对应的使用方法可以看看我们的这篇文章： [《enhanced RTMP》](https://pengrl.com/lal/#/enhanced_rtmp)

### ▦ 二. G711A/G711U

上个版本支持G711音频编码格式时留了个待完成的尾巴，RTSP转RTMP只有视频没有声音，这个版本支持了。

G711的整体情况我们正在整理中： https://www.yuque.com/pengrl/public/psxbp37r3yqopnxx

### ▦ 三. RTSP TCP/UDP 切换

一个优化： 有的RTSP源不支持TCP传输音视频数据，会在SETUP信令阶段给对端回复status code 461，用于表明自身不支持TCP，新版本的LAL收到461后会尝试切换UDP SETUP。反之UDP切TCP也是一样的道理。

### ▦ 更多

还有一些修改不逐个介绍了，大致如下：

> - [opt] mpegts根据编码格式打包patpmt（提高纯视频流的兼容性）
> - [opt] HTTP-API: 支持跨域
> - [fix] rtmp: 解析amf strict array
> - [fix] rtmp转mpegts处理时间戳错误，导致有b帧时ffplay播放hls报错
> - [fix] GetSamplingFrequency missing 24000
> - [fix] simplifing logic to enable `fragment_duration_ms` on configuration to be under thousands (but not below hundreds)
> - [fix] 解决CustomizePubSessionContext使用dumpFile空指针导致崩溃的问题
> - [test] unit test for base.DumpFile
>
> 以上内容摘取自 [《lal CHANGELOG版本日志》](https://pengrl.com/lal/#/CHANGELOG) ，你可以通过源文档获取更详细的内容。

### ▦ 开发者

感谢参与这个版本的开源贡献者：yoko, ZSC714725, joaop, sanenchen, yang heng01~, penglh, LiH0820

### ▦ 进一步了解lal

- [github](https://github.com/q191201771/lal)
- [官方文档](https://pengrl.com/lal)
- [联系作者](https://pengrl.com/lal/#/Author)

微信扫码加我好友（进微信群）：

![wechat](https://pengrl.com/images/yoko_vx.jpeg?date=2304)

本文完，祝你今天开心。

yoko, 202304

