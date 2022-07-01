# Video Engine



**(1)** VideoEngine

WebRTC视频处理引擎

VideoEngine是包含一系列视频处理的整体框架，从摄像头采集视频到视频信息网络传输再到视频显示整个完整过程的解决方案。

 a.  VP8
视频图像编解码器，是WebRTC视频引擎的默认的编解码器

VP8适合实时通信应用场景，因为它主要是针对低延时而设计的编解码器。

PS:VPx编解码器是Google收购ON2公司后开源的，VPx现在是WebM项目的一部分，而WebM项目是Google致力于推动的HTML5标准之一。

 

b.  Video Jitter Buffer

视频抖动缓冲器，可以降低由于视频抖动和视频信息包丢失带来的不良影响。

 

c.  Image enhancements

图像质量增强模块

对网络摄像头采集到的图像进行处理，包括明暗度检测、颜色增强、降噪处理等功能，用来提升视频质量。

