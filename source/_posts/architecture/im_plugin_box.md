---
title: 关于多 IM 通信通道「SDK」的架构设计
categories: Architecture
tags: [android, iOS]
---

### 设计背景

由于公司的主要产品是做智能硬件相关的产品。最早开始设备端是基于 Android OS 开发的。所以当时在做及时通信方案选型的时候，选择了云信 IM，来作为设备端与 App 的通信工具。后来慢慢，我们的新品「SKU」不断增多，系统也由最初的 Android OS，到 Linux，到 Atos ，不断往低成本方案演进，而且还出来了欧洲版。诸多因素导致 App 端云信 IM 已经不能担此重任，必须重新选型。基于此做出了一个 IMPluginBox 的 Framework。

### 架构设计

此 SDK 采用分层架构设计，每层的职责清晰且具有可扩展性。目前此 SDK 底层集成了: 网易云信 NIM, 腾讯云通信 TIM, 阿里云 MQTT, 三家 SDK. SDK 的架构图如下：

![](https://raw.githubusercontent.com/Davidxiaoshuo/blog_source/master/resources/images/message-plugin-box.png)

在 iOS 中，根据 CocoaPods 的特性，可以根据具体需求，底层去集成一家或多家 SDK。 

如: `pod 'WLIMPlugBox', '0.3.0', :subspecs => ['AliyunMQTT', 'NIM']`


在 Android 中，利用 gradle 的 Flavor 的特性，对各家 SDK 进行组合，可以发布多个 Library 到 Maven，App 可根据需求集成不同 Lib。

如: `"ai.ling.lib:im-messager-nimAndAliyunMqtt:0.1.0-rc.22";`


### 业务层应答机制

由于 App 需要实时掌握设备端的状态。所以需要基于 IM 需要设计出一套 ACK 机制来实现此需求。

![](https://raw.githubusercontent.com/Davidxiaoshuo/blog_source/master/resources/images/im_ack.png)
