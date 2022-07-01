# Voice Engine



**(1)**  VoiceEngine

音频引擎是包含一系列音频多媒体处理的框架，包括从视频采集卡到网络传输端等整个解决方案。

PS：VoiceEngine是WebRTC极具价值的技术之一，是Google收购GIPS公司后开源的。在VoIP上，技术业界领先，后面的文章会详细了解



a.  iSAC

Internet Speech Audio Codec

针对VoIP和音频流的宽带和超宽带音频编解码器，是WebRTC音频引擎的默认的编解码器
采样频率：16khz，24khz，32khz；（默认为16khz）
自适应速率为10kbit/s ~ 52kbit/；
自适应包大小：30~60ms；
算法延时：frame + 3ms

 

b.  iLBC
Internet Low Bitrate Codec

VoIP音频流的窄带语音编解码器
采样频率：8khz；
20ms帧比特率为15.2kbps
30ms帧比特率为13.33kbps
标准由IETF RFC3951和RFC3952定义


c.  NetEQ for Voice

针对音频软件实现的语音信号处理元件

NetEQ算法：自适应抖动控制算法以及语音包丢失隐藏算法。使其能够快速且高解析度地适应不断变化的网络环境，确保音质优美且缓冲延迟最小。

是GIPS公司独步天下的技术，能够有效的处理由于网络抖动和语音包丢失时候对语音质量产生的影响。

PS：NetEQ 也是WebRTC中一个极具价值的技术，对于提高VoIP质量有明显效果，加以AEC\NR\AGC等模块集成使用，效果更好。

> NOTE:
>
> 参见 dandelioncloud [voip 名词解释 Qos，VAD，AEC，AGC，CNG ，AEC,AGC](https://dandelioncloud.cn/article/details/1534836971138740226) 
>
> 

d.  Acoustic Echo Canceler (AEC)
回声消除器是一个基于软件的信号处理元件，能实时的去除mic采集到的回声。

 

e.  Noise Reduction (NR)
噪声抑制也是一个基于软件的信号处理元件，用于消除与相关VoIP的某些类型的背景噪声（嘶嘶声，风扇噪音等等… …）