# Andrii Sergiienko [**WebRTC Cookbook**](http://subnets.ru/books/webrtc-cookbook-andrii-sergiienko.pdf)



## **Peer Connections**

With simple and short recipes, you will learn how to create your own **signaling server**. The key data that needs to be exchanged by peers before they establish a direct connection is called the **session description**—it specifies the peers' **configuration**. Signaling server is a component in an application's infrastructure that is accessible by all peers and serves to exchange multimedia's session description. The way peers should exchange data is not described by WebRTC standards, so you should make the decision on your own regarding the protocol and mechanism you will use for this task.

> NOTE:
>
> 使用signaling server的目的是建立peer-to-peer connection。它非常类似于front。



The signaling stage is represented in the schema that is shown in the following diagram:

![](/Users/dengkai01/github/audio-video/docs/Software/WebRTC/book-WebRTC-Cookbook/signing-server.jpg)



This chapter also covers the basic use case of how to use **WebRTC data channels**: **file transferring** and **peer-to-peer chat**.

You will also learn how to configure and use **Session Traversal Utilities for NAT (STUN)** and **Traversal Using Relays around NAT (TURN)** services, and of course, this chapter covers making peer-to-peer calls using WebRTC.

