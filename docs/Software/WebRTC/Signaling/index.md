# Signaling信令

一、在 wikipedia [WebRTC](https://en.wikipedia.org/wiki/WebRTC) 中有这样的描述:

> The WebRTC API includes **no provisions for signaling**, that is discovering peers to connect to and determine how to establish connections among them. 

二、Elab zhihu [浅谈WebRTC技术原理与应用](https://zhuanlan.zhihu.com/p/453986829)

Signaling交换的内容: 

> 信令服务器的作用是基于双工通信来中转信息。中转信息包括:
>
> 1、公网IP映射后的网络定位信息（公网IP、端口）
>
> 2、媒体数据流
>
> 3、......

Signaling交换的流程图:

> ![img](https://pic3.zhimg.com/80/v2-1cbc9c5221bd4e6c4a4d8995cc6dcf9e_1440w.jpg)
>
> 
>
> 1、A和B双方先调用 `getUserMedia` 打开本地摄像头，作为本地待输出媒体流；
>
> 2、向**信令服务器**发送加入房间请求；
>
> 这是业务逻辑



> 
>
> 1.1 媒体捕获
>
> 1.2 网络协商
>
> 1.3 媒体协商



三、webrtcforthecurious [Signaling](https://webrtcforthecurious.com/docs/02-signaling/) 

webrtcforthecurious [Signaling](https://webrtcforthecurious.com/docs/02-signaling/) ，下面highlight一些其中描述的内容:

1、WebRTC uses an existing protocol called the Session Description Protocol.

2、"The WebRTC agents don’t care how they are transported" 这段话的意思是 WebRTC并没有定义"Signaling message"的传输方式



## 其它

zhihu [什么叫信令?信令的功能是什么?](https://www.zhihu.com/question/386788957)