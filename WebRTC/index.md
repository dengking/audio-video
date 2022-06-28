# WebRTC

在 《FFmpge音视频开发基础与实战》中，有这样的介绍:

> 目前主流的实时音视频解决方案主要基于WebRTC标准。与传统的RTMP+CDN系统相比，基于WebRTC的方案延迟更低，卡顿情况更少，且支持直接接入浏览器进行推流与播放。



> 许多基于公网的视频会议系统都以WebRTC为基础，以尽可能低的延迟提供高质量的音频和视频实时通信服务。



## wikipedia [WebRTC](https://en.wikipedia.org/wiki/WebRTC)

### Design

The WebRTC API includes **no provisions for signaling**, that is discovering peers to connect to and determine how to establish connections among them. 

> NOTE:
>
> "signaling"的含义是"信令"。

