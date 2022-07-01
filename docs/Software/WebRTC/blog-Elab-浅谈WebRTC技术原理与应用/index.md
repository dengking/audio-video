# Elab zhihu [浅谈WebRTC技术原理与应用](https://zhuanlan.zhihu.com/p/453986829)

> https://baijiahao.baidu.com/s?id=1721644496896867783&wfr=spider&for=pc



WebRTC**（Web Real-Time Communication）**，即网页即时通信。是一个支持[网页浏览器](https://link.zhihu.com/?target=https%3A//zh.wikipedia.org/wiki/%E7%B6%B2%E9%A0%81%E7%80%8F%E8%A6%BD%E5%99%A8)进行实时语音对话或视频对话的技术方案。从前端技术开发的视角来看，是一组可调用的API标准。



![img](https://pic3.zhimg.com/80/v2-eedfba0e9e112c46e5407671453fdcea_1440w.jpg)



在WebRTC发布之前，开发实时音视频交互应用的成本是非常昂贵，需要考虑的技术问题很多，如音视频的编解码问题，数据传输问题，延时、丢包、抖动、回音的处理和消除等，如果要兼容浏览器端的实时音视频通信，还需要额外安装插件。

## 二、简要特征

### **1. 实时通讯**

是一项实时通讯技术，允许网络应用或者站点，在不借助中间媒介的情况下，建立浏览器之间点对点（Peer-to-Peer）的连接，实现视频流和（或）音频流或者其他任意数据的传输。

> NOTE: 
>
> "不借助中间媒介" 这一点非常重要，其实就是 "Peer-to-Peer"

### **2. 无依赖/插件**

### **3.** **协议栈** **众多**

它并不是单一的协议，包含了媒体、加密、传输层等在内的多个协议标准以及一套基于 **JavaScript** 的 API，**它包括了音视频的采集、编解码、网络传输、显示等功能**。通过简单易用的 JavaScript API ，在不安装任何插件的情况下，让浏览器拥有了 P2P音视频和数据分享的能力。



![img](https://pic3.zhimg.com/80/v2-7410c262b95f959ec46e7187ad6fed4a_1440w.jpg)



> NOTE:
>
> 需要注意的是，上面是两族协议:
>
> 第一列: 它不是WebRTC定义的，它所传输的是signaling message。
>
> 第二列: 它是WebRTC定义的，它所传输的是媒体流 。

（WebRTC依赖众多协议栈图）



| 相关协议 | 介绍与作用 |
| ------------- | ---------------------- |
| ICE、STUN、TURN | 用于内网穿透, 解决了获取与绑定公网映射地址 |
| DTLS | 用于对传输内容进行加密 |
| SRTP 、 SRTCP | 对媒体数据的封装与传输控制协议 |



同时WebRTC 并不是一个孤立的协议，它拥有灵活的**信令**，可以便捷的对接现有的SIP 和电话网络的系统。



## 三、兼容覆盖

目前大部分主流浏览器都正常兼容，如下图



![img](https://pic1.zhimg.com/80/v2-dc90b4cc924246bded92e4a62792bc3c_1440w.jpg)



## 四、技术框架

如下图的技术框架描述了WebRTC的核心内容和面向不同开发者的API设计。



![img](https://pic2.zhimg.com/80/v2-610d02f4e2ba72d257bae1a31ac196fd_1440w.jpg)



（技术框架图）

从图中可看到主要面向三类开发者的API设计，包括：

a.对于Web开发者的API

框架包含了基于 **JavaScript**、 经过W3C认证了的一套API标准，使得web开发者可以基于这套API开发基于WebRTC的即时通讯应用

b.对于浏览器厂商的API

框架同样包含了基于C++的底层WebRTC接口，对于浏览器厂商底层的接入十分友好。

c.浏览器厂商可自定义的部分

框架中还包含浏览器厂商可自定义的音视频截取等扩展部分。



## 五、核心内容

### WebRTC主要组成：

#### Voice Engine（音频引擎）

1、Voice Engine包含iSAC/iLBC Codec（音频编解码器，前者是针对宽带和超宽带，后者是针对窄带）

2、NetEQ for voice（处理网络抖动和语音包丢失）

3、Echo Canceler（回声消除器）/ Noise Reduction（噪声抑制）

#### Video Engine（视频引擎）

1、VP8 Codec（视频图像编解码器）

2、Video jitter buffer（视频抖动缓冲器，处理视频抖动和视频信息包丢失）

3、Image enhancements（图像质量增强）

#### Transport

1、SRTP: 安全的实时传输协议，用于**音视频流**传输

2、Multiplexing: 多路复用

3、P2P: STUN+TURN+ICE，用于NAT网络和防火墙穿越的

4、DTLS: 安全传输可能还会用到DTLS（数据报安全传输），用于加密传输和密钥协商

5、UDP: 整个WebRTC通信是基于UDP的

## 六、实现原理

### 1、公网IP映射：明确网络定位信息

WebRTC是建立浏览器端到端的连接（P2P），由于不需要服务器中转，所以获取连接对象的网络地址的方式，是借助于**ICE**、**STUN**、**TURN**等辅助内网穿透技术（NAT）得到对应主机的公网网络地址和端口等网络定位信息。明确网络定位是建立端与端直接通讯的基础。

![img](https://pic2.zhimg.com/80/v2-e5d445732b06a4970c656e3ee31316cd_1440w.jpg)

（NAT原理图）

![img](https://pic2.zhimg.com/80/v2-e811130ce400da9079ed0c62ff5e79bd_1440w.jpg)

（STUN服务器用于辅助内网穿透得到对应主机的公网网络地址和端口信息图）



### 2. 信令服务器：网络协商与信息交换

信令服务器的作用是基于双工通信来中转信息。中转信息包括:

1、公网IP映射后的网络定位信息（公网IP、端口）

2、媒体数据流

3、......





![动图](https://pic2.zhimg.com/v2-5f71e669e191cfd65cd2e031accbbf65_b.webp)





（概念图）



![img](https://pic2.zhimg.com/80/v2-66ae722f657818043d84356b889ec995_1440w.jpg)



（信令服务器信息交互过程图）



### 3. 会话描述协议SDP：统一的媒体协商方式

> NOTE:
>
> 关于SDP，在 [WebRTC for the Curious](https://webrtcforthecurious.com/) # [Signaling](https://webrtcforthecurious.com/docs/02-signaling/) # [What is the *Session Description Protocol* (SDP)?](https://webrtcforthecurious.com/docs/02-signaling/#what-is-the-session-description-protocol-sdp) 有着更加详细的介绍。

不同端/浏览器对于媒体流数据的编码格式各异，如VP8、VP9等，参与会话的各个成员的能力不对等、用户环境与配置不一致等。

WebRTC 通讯还需要确定和交换本地和远程音频和视频媒体信息，例如分辨率和编解码器功能。 交换媒体配置信息的信令通过使用会话描述协议 (SDP) 交换Offer和Anwser来进行。

SDP的交换一定是先于音视频流交换的。其内容包括会话基本信息、媒体信息描述等。

```ini
//SDP的结构体
Session description（会话级别描述）
         v=  (protocol version)
         o=  (originator and session identifier)
         s=  (session name)
         c=* (connection information -- not required if included in all media)
         One or more Time descriptions ("t=" and "r=" lines; see below)
         a=* (zero or more session attribute lines)
         Zero or more Media descriptions

Time description
         t=  (time the session is active)

Media description（媒体级别描述）, if present
         m=  (media name and transport address)
         c=* (connection information -- optional if included at session level)
         a=* (zero or more media attribute lines)
```

一个SDP例子如下

```js
v=0 //代表版本，目前一般是`v=0`.
o=- 3883943731 1 IN IP4 127.0.0.1
s=
t=0 0 //会话处于活动状态的时间
a=group:BUNDLE audio video //：描述服务质量，传输层复用相关信息
m=audio 1 RTP/SAVPF 103 104 0 8 106 105 13 126 // ...

a=ssrc:2223794119 label:H4fjnMzxy3dPIgQ7HxuCTLb4wLLLeRHnFxh81
```

### 4.一对一连接建立过程

建立一对一的Web RTC连接过程为



![img](https://pic2.zhimg.com/80/v2-f56f5bb335ddbb3430d623b1dc27fbc9_1440w.jpg)

#### 简要过程

![img](https://pic1.zhimg.com/80/v2-42a066f5f2fdf85f80594824977e74c8_1440w.jpg)



**如上图所示，解释一下：**

*1）*交换SDP，获取各自媒体配置信息；

*2）*STUN服务器交换网络地址和端口等网络信息；

*3）*Turn中转音视频媒体流数据。





#### 工作流



![img](https://pic3.zhimg.com/80/v2-1cbc9c5221bd4e6c4a4d8995cc6dcf9e_1440w.jpg)

（工作流程图）

1、A和B双方先调用 `getUserMedia` 打开本地摄像头，作为本地待输出媒体流；

2、向**信令服务器**发送加入房间请求；

3、Peer B 接收到 Peer A 发送的 offer SDP 对象，并通过`PeerConnection`的`SetLocalDescription`方法保存 Answer SDP 对象并将它通过信令服务器发送给 Peer A。

4、在 SDP 信息的 offer/answer 流程中，Peer A 和 Peer B 已经根据 SDP 信息创建好相应的 **音频 Channel** 和 **视频 Channel**，并开启Candidate 数据的收集，Candidate数据（本地IP地址、公网IP地址、Relay服务端分配的地址）。

5、当 Peer A 收集到 Candidate 信息后通过信令服务器发送给 Peer B。同样的过程 Peer B 对 Peer A 也会再发送一次。



### 5. 多对多的建立



![img](https://pic1.zhimg.com/80/v2-4756b9a6bdcea112710f5536a3a0c604_1440w.jpg)



（多对多建立点到点连接概念图，以三个用户点对点的连接为例）

### 6. JavaScript主要接口



#### [getUserMedia()](https://link.zhihu.com/?target=https%3A//webrtc.github.io/samples/src/content/getusermedia/gum/)

[getUserMedia()](https://link.zhihu.com/?target=https%3A//webrtc.github.io/samples/src/content/getusermedia/gum/)：访问数据流，例如来自用户的相机和麦克风

```js
//请求媒体类型
const constraints = {
  video: true
  audio:true
};

const video = document.querySelector('video');

//挂载流到相应dom展示本地媒体流
function handleSuccess(stream) {
  video.srcObject = stream;
}

function handleError(error) {
  console.error('getUserMedia error: ', error);
}
//利用摄像头捕获多媒体流
navigator.mediaDevices.getUserMedia(constraints).
  then(handleSuccess).catch(handleError);
```

#### [RTCPeerConnection](https://link.zhihu.com/?target=https%3A//webrtc.github.io/samples/src/content/peerconnection/pc1/)

[RTCPeerConnection](https://link.zhihu.com/?target=https%3A//webrtc.github.io/samples/src/content/peerconnection/pc1/)：通过加密和带宽管理工具启用音频或视频通话

```js
// 允许 RTC 服务器配置。
const server = {
    "iceServers":
            [{ "urls": "stun:stun.stunprotocol.org"}]
};

// 创建本地连接
const localPeerConnection = new RTCPeerConnection(servers);

// 收集Candidate 数据
localPeerConnection.onicecandidate=function(event){
    ...
}
// 监听到媒体流接入时的操作
localPeerConnection.ontack=function(event){
    ...
}
```



#### [RTCDataChannel](https://link.zhihu.com/?target=https%3A//webrtc.github.io/samples/src/content/datachannel/basic/)

[RTCDataChannel](https://link.zhihu.com/?target=https%3A//webrtc.github.io/samples/src/content/datachannel/basic/)：支持通用数据的点对点通信，常用于数据点到点的传输

```js
const pc = new RTCPeerConnection();
const dc = pc.createDataChannel("my channel");
//接受数据
dc.onmessage = function (event) {
  console.log("received: " + event.data);
};
//打开传输
dc.onopen = function () {
  console.log("datachannel open");
};
//关闭传输
dc.onclose = function () {
  console.log("datachannel close");
};
```



## 七、应用案例

多人视频案例为实践应用

### 1. 设计框架

![img](https://pic3.zhimg.com/80/v2-b1ee05d55f217863bdf4dea907545d56_1440w.jpg)



（多人视频基本框架图）

### 2.关键代码

#### 1.1 媒体捕获

获取浏览器视频权限，捕获本地视频媒体流，在Video元素中附加媒体流，显示本地视频结果。

```javascript
//摄像头兼容性处理
navigator.getUserMedia = ( navigator.getUserMedia ||
               navigator.webkitGetUserMedia ||
               navigator.mozGetUserMedia ||
               navigator.msGetUserMedia);
// 获取本地音频和视频流
navigator.mediaDevices.getUserMedia({
                "audio": false,
                "video": true
}).then( (stream)=> {
//显示自己的输出流，挂到页面Video元素上
    document.getElementById("myVido").srcObject=stream
})
```

#### 1.2 网络协商

创建对等连接，收集ICE候选，等待媒体流接入时挂载到dom



#### 1.3 媒体协商

> NOTE: 上述两个"协商"总结地不错

#### 1.4 信令中转



## 八、重要意义

1、降低在web端的音视频交互开发门槛

2、避免依赖、插件造成的次生问题

3、统一化和标准化对传统音视频交互环境差异性的规避

以往音视频交互需要面对不同的NAT、防火墙对媒体P2P的建立带来了很大的挑战

现在WebRTC 中有P2P 打洞的开源项目 libjingle ,支持 STUN，TURN 等协议

更高效优化的算法、技术对于音视频交互性能的提升

WebRTC 通过NACK、FEC技术，避免了经过服务端路由中转，减少了延迟和带宽消耗。还有 TCC + SVC + PACER + JitterBuffer 等技术对于音视频流畅性进行了优化