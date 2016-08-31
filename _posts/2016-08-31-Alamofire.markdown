---
layout: post
title: Alamofire 初探
date: 2016-08-31 15:32:24.000000000 +08:00
---

作为 iOS 开发者，大都不可避免的需要用到网络库。现就 Alamofire，初探其实现。先上类图：

![类图](/assets/images/2016/Alamofire.png)

看类图已经能了解整个框架的大致实现了。下面仅对类图做简单的补充说明：

*   Manager 是一个单件，提供各种请求接口。包含 NSSession、SessionDelegate 实例。其在 Download.swift、Upload.swift、Stream 中均有扩展，以根据不同的 task 提供定制化接口。
*   SessionDelegate 遵守 NSURLSessionDelegate、NSURLSessionTaskDelegate、NSURLSessionDataDelegate、NSURLSessionDownloadDelegate 协议，作为分发类根据 host 和 Request 的 task 类型调起相应 TaskDelegate 方法，并提供处理各种 delegate 消息的 hook。
*   ServerTrustPolicyManager 默认为 nil， 可以根据需要设置校验策略（验证默认证书连、自制证书、公钥等）。
*   TaskDelegate 的 queue 为请求结束时的操作队列。
*   Request 类提供控制请求和结果回调的 API，可以调用 response 系列接口，设置请求结束后的回调函数。此外，它还提供一些基本的结果序列化接口。
