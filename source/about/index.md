---
title: 个人简介
description: 个人简介
layout: about
comments: false
sidebar: custom
---

### 自我评价

​	本人具有丰富的 iOS / Android 双端开发经验，具有一定的团队管理经验。乐观积极，强烈的责任心与执行能力，超强的学习能力，良好的业务推动能力，优良的跨团队、跨部门沟通协作能力。具有优秀的编码能力，问题分析 & 定位能力，优良的架构设计能力，良好的编码风格。以下几点是自我评价的具体实例：

- 主导产品大版本升级工作，Luka App 2.0 主导 App 架构设计，协助后端同学设计 API，并最终落地发布。
- 引入新的技术方案，全面推动 GraphQL API 落地
- 设计优化重构核心 SDK ，并从内部孵化提供给第三方使用。
- 带领团队高效快速的进行版本迭代。Luka 2.0 App 已经由 2.0.0 版本快速迭代到 2.22.0 版本


# 工作经历

#### 2016.10 ~ 至今&emsp;&emsp;北京物灵科技有限公司

移动端研发架构「负责人」

工作描述：

- 主导 Luka App 2.0 的研发
- 负责产品技术选型 & 核心功能组件研发 & 新技术方案引入。
- 负责 App + 设备端团队的质量管理：CodeReview + CI / CD。
- 负责 App + 设备端团队的版本迭代技术实现把控。
- 负责协调各方资源并规划和分配，配合项目经理高效高质推进项目。
- 负责 PC 端产测客户端的研发。



#### 2015.5 ~ 2016.9&emsp;&emsp;百度

移动端高级研发工程师

工作描述：

- 负责 IDL - HCI 组 DuBike 项目 Android App 端研发工作
- 负责 DuBike App 与硬件组进行对接工作
- 负责 IDL - HCI 组 `脸优` 项目 iOS App 端的研发工作
- 负责 人脸贴纸 SDK 的设计 & 研发工作 & 对完开放对接
- 负责 人脸闸机，公司内部 iOS App 的研发工作



#### 2014.5 ~ 2015.5&emsp;&emsp;软通动力 - 百度 IDL

Android 高级研发工程师

工作描述：

- 负责 IDL - HCI 组 DuBike 项目 Android App 端研发工作
- 负责 DuBike App 与硬件通信协议的设计 & 研发
- 负责协助 PM 定义 DuBike 功能需求



#### 2012.1 ~ 2014.5&emsp;&emsp;北京博力恒昌科技有限公司

Android 研发工程师

工作描述：

- 负责智能家居中控端「Android OS」研发落地
- 负责智能家居 Android - App 的研发落地
- 参与 Zigbee 消息报文的制定 & 解析 & 封装工作
- 负责公司售后技术支持工作



# 项目经历



## Luka 阅读养成

#### [什么是 Luka 阅读养成](https://baike.baidu.com/item/LUKA/53457293)

​	Luka借助前沿的人工智能（AI）技术与生动灵性的角色设计，为2岁以上的宝宝家庭创造有未来感的绘本阅读体验。培养孩子绘本阅读兴趣、养成绘本阅读习惯、推荐绘本， 简单快捷一站式帮助父母满足孩子们对看绘本听故事的渴望，为阅读敏感期的孩子培养受用一生的宝贵阅读习惯。通过，绘本在线 & 离线阅读、听听音频在线 / 离线播放、语音交互，App 实时操控设备等功能，加持 LingUI 关系式交互，使得 Luka 成为一个灵气十足的，陪伴宝宝一起成长的阅读小伙伴。



#### Luka 绘本阅读机器人的使命是什么

​	让孩子从屏幕回归书本。



#### Luka 项目团队规模

​	该项目隶属于公司 KID 业务线。我们在 App 端「iOS & Android」分别投入 <u>2</u> 人力进行支持；设备端「Robot端」投入  <u>3</u>人力进行支持。

完整的后台分布式服务 + CMS 后台管理系统 + 产测服务 + 产测客户端 + CV 算法。



#### Luka 绘本阅读机器人的几大功能

- 离线/在线绘本阅读能力，多模式CV识别能力「手指指读，跟读，精读，评分」。
- 宝宝书架、拍录绘本、绘本录音个性化绘本能力。
- 四大分类听听音频、宝宝歌单、专辑绑定、专属云盘、播放足迹，超多超精音频资源。
- 从唤醒，识别，理解，语音合成「Wakeup - ASR - NLP - TTS」的全链条语音能力。
- 视频课程、定时熏听，定制化的课程辅导能力。



### Luka 阅读养成 App-iOS 2.0

​	支持 iOS 9.0 及以上系统。架构： MVVM，整体组合套件：MVVM + GraphQL API「Apollo」+ IM 通讯 + Releam + ReactiveKit + Router

##### 方案选型：

- 开发语言：`Swift`，基于 `SwiftLint` 管控代码规范。

- 网络服务框架： 采用 Facebook 的出品的 `GraphQL` API ，基于`Apollo` 封装网络框架。

- IM 通讯服务：基于腾讯云通信 TIM、网易云信 NIM，阿里云 MQTT 封装 `MessagePluginBox`，可根据业务 & SKU 选择不同服务通道。

- 模块通信：基于自定义 Router 路由机制。

- 数据存储：本地数据基于 `Releam` 数据库，远程文件资源存储基于 `Aliyun-OSS`。

- App 异常监测：前期采用 `Fabric`，后期采用腾讯 `Bugly`。

- App 统计服务：采用神策埋点统计服务。



#### Luka 阅读养成 App-Android 2.0

​		支持 Android 5.0 及以上。

​				前期架构： MVP， 整体组合套件： MVP + GraphQL API「Apollo」+ IM 通讯 + Releam + Router
​				后期架构： MVVM， 整体组合套件：MVVM + GraphQL API「Apollo」+  IM 通讯 + Releam + Router + Jetpack

##### 方案选型：

- 开发语言： `kotlin`, 基于 `ktlint` 管控代码规范

- 网络服务框架： 采用 Facebook 的出品的 `GraphQL` API ，基于`Apollo` 封装网络框架。

- IM 通讯服务：基于腾讯云通信 TIM、网易云信 NIM，阿里云 MQTT 封装 `MessagePluginBox`，可根据业务 & SKU 选择不同服务通道。

- 模块通信：基于自定义 Router 路由机制。

- 数据存储：本地数据基于 `Releam` 数据库，远程文件资源存储基于 `Aliyun-OSS`。

- App 异常监测：前期采用 `Fabric`，后期采用腾讯 `Bugly`。

- App 统计服务：采用神策埋点统计服务。



#### 主要产出

1. App 整体架构设计，搭建
2. GraphQL API 新技术引进，内部推广，Apollo 网络框架二次封装，使之更切合项目需求需要，并解决 Authorization 不同状态的访问问题。
3. MessagePluginBox 封装，制定消息自定义协议，以及消息交互机制，输出 `Messenger` Fragment 可提供第三方使用的 Framework 级别。「[👇有框架介绍](#架构设计示例 - MessagePluginBox)」
4. 制定 Router 路由协议，输出自定义 Router 框架
5. others：多语言「i18n」自动化脚本；GraphQL 多端统一请求自动化脚本；iOS-CI/CD 整体环境搭建；



### Luka Robot 

#### SKU

目前 Luka 系列 针对孩子不同年龄段，不同家长用户需求，不同的销售渠道，已经研发了多款 SKU ：

- Android OS：Luka HeroS、Luka Hero、Luka

- Linux OS「T20」：Luka Baby

- ATOS:	Luka Mini、 JiFeiFei、Luka Box

其中 Android OS & Linux OS「T20」系列 SKU 由公司内部自研。Android OS 由本人主导研发，Linux 参与研发。

ATOS 方案 SKU 采用外部合作方式进行研发, 本人负责主导外部研发协调，对接工作。



#### Robot App 「Android」

##### 方案选型：Android 4.4 + 模块化 + 事件驱动架构； 输出 Luka App、Hardware Test App、OTA App

- 开发语言: 业务部分 `kotlin`，基于 `ktlint` 管控代码规范；底层能力部分 `c++`

- 网络服务框架：`Retrofit 2.0`

- 语音模块：前期采用科大讯飞 + 先声 SDK；后期切换到腾讯叮当

- 图像识别：LingCV 「此部分为公司算法自研部分」

- UI 模块： Unity 3D 自研动画表情

- 音频播放器： 基于` IjkPlayer` 封装的 LingPlayer 音频播放器

- App 异常监测：前期采用 `Fabric`，后期采用腾讯 `Bugly`。

- App 统计服务：采用神策埋点统计服务。



#### 主要产出：

1. Robot App 整体架构设计，搭建
2. 绘本识别，多模式识别设计，配合 Unity 3D 优化表情平滑过渡表现。
3. 疑难问题解决，如语音交互，音频数据采集，加工导致出现杂音，出现丢帧现象等问题。
4. 基于` IjkPlayer` 封装的 `LingPlayer` 音频播放器，并针对需求，对 ffmpeg 进行裁剪，减小包体积
5. 利用 RTC 时钟芯片，修改 Framework 层代码，实现定时开机功能
6. 基于阿里云 SLS 日志服务，封装出 Android SDK



### Robot Hardware Test 「Win-PC 版」

​	PC 端产测主要服务于工厂生产。在工厂生产过程中，对设备进行：设备码烧录，初始化设置，资源烧录，硬件元器件检测，老化，装箱，拆箱等操作，辅助工厂进行设备生产流程。



#### 主要产出

1. 产测基础功能实现
2. 串口命令设计
3. 基础组件设计 & 实现



##Auri  智能灯

#### [什么是 Auri](https://www.indiegogo.com/projects/auri-the-all-in-one-smart-self-care-light#/)

​	Planet Auri，借助 Amazon Alexa 内核，同时兼具氛围，照明功能的一款家庭助理智能灯。



#### Auri 智能灯的使命是什么

​	面向家庭场景的HOMEAI产品线，让灵性真正走入未来人们的生活中，为成年人及家庭生活带来品质升级。



#### Auri 项目团队规模

​	该项目隶属于公司 Home 业务线，我们在 App 「iOS」端，投入两人力；在 Robot 端研发上投入两人力；在 Server 后台投入两人力。



#### Auri 智能灯的几大功能

- 智能语音交互 Alexa
- 根据音乐舞动灯光
- 多种氛围灯效
- 闹钟、提醒等基础功能



#### 个人职责 「移动端负责人」

- 负责 设备端 &  App 项目框架设计
- 负责产品技术选型 & 核心功能组件研发。
- 配合产品经理讨论需求定义
- 指导并带领初级工程师进行版本迭代任务



#### 主要产出

- Alexa Android SDK 开发并集成。
- 配合算法同学产出一套根据音乐节奏舞动灯光的 SDK。
- 搭建 iOS App 的整体架构，网络请求 SDK。
- 输出一篇专利：[《一种调整多媒体环境的方法、装置及存储设备》](https://www.zhangqiaokeyan.com/patent-detail/06120100424440.html)



## 脸优

#### [什么是脸优](https://baike.baidu.com/item/脸优/18556796?fr=aladdin)

​	脸优，一款以实时摄像为使用方式的趣味性换脸娱乐app，采用基于人脸特征点定位的人脸检测技术、表情迁移技术、图像融合技术，一个全民都能玩的“黑科技”产品。用户通过脸优app生成的作品，

可以分享到多个sns社区。



#### 脸优的使命

脸优FaceIt，就是要玩出“鬼”！



#### 方案选型「iOS」：

- 开发语言采用 Swift + OC 混编， MVC 架构
- Baidu IDL 人脸识别 SDK
- SNS 分享 SDK 基于 腾讯互联QQ + 微信 + 微博 的封装。
- GPUImage SDK 用于处理 滤镜，实时视频录制



#### 个人职责及产出 「iOS-App 研发」

- 负责集成百度账号 SDK
- 负责用户中心模块，变脸模块中 Gif 功能、海报功能、滤镜功能，秀场模块的需求开发与维护。
- 负责 GPUImage 三方库的修改与优化，使之更符合项目和需求。
- SNS 分享 SDK 基于 腾讯互联QQ + 微信 + 微博 的封装。
- 人脸贴纸 iOS - SDK 的制作。后应用于：熊猫直播 App，百度贴吧 App。



## DuBike「百度智能自行车」

#### [什么是 DuBike](https://baike.baidu.com/item/百度智能自行车?fromtitle=DuBike&fromid=15883294)

​	DuBike由百度[深度学习实验室](https://baike.baidu.com/item/深度学习实验室/18784654)与清华美院共同打造，百度IDL 操刀的[智能自行车OS](https://baike.baidu.com/item/智能自行车OS/15928717)研究计划，将以[百度大脑](https://baike.baidu.com/item/百度大脑/13333434)为核心引擎，通过模块化IO，实现自行车的导航、社交、健康监控、众包骑行地图、智能推荐路线及健身计划等功能。智能自行车DuBike将由安置在车身的智能感应系统以及具有健康管理和社交功能的APP组成，将[集成传感器](https://baike.baidu.com/item/集成传感器/5483992)、人工智能、自动控制、[大数据分析](https://baike.baidu.com/item/大数据分析/6250459)等多项技术。



#### DuBike 的使命

​	打造智能骑行平台



#### 方案选型「Android - App」

- 开发语言采用 Java + C++， MVC 架构。
- 百度地图 SDK + 百度地图在线 API 接口。
- Volley 网络请求框架 + Gson 模型解析。
- 自定义 Chart View 。
- 蓝牙 BLE 4.0 传输数据。



#### 个人职责及产出

- 参与产品需求定义阶段讨论

- 负责与硬件进行对接，负责 BLE 蓝牙数据包协议制定。
- 负责提供底层蓝牙数据的解析，存储。
- 负责 App 首页数据模块，自行车配置模块，骑行足迹模块等的需求研发。





# 架构设计示例 - MessagePluginBox

描述： 由于需求 & SKU 不同， App 需要适配多家不同的 IM SDK，故而才有此 SDK 的诞生。此 SDK 采用分层架构设计，每层的职责清晰且具有可扩展性。目前此 SDK 底层集成了，网易云信 NIM、腾讯云通信 TIM、阿里云 MQTT，三家 SDK。SDK 的架构图如下：



在 iOS 中，根据 CocoaPods 的特性，可以根据具体需求，底层去集成一家或多家 SDK。

如：`pod 'WLIMPlugBox', '0.3.0', :subspecs => ['AliyunMQTT', 'NIM']`



在 Android 中，利用 gradle 的 Flavor 的特性，对各家 SDK 进行组合，可以发布多个 Library 到 Maven，App 可根据需求集成不同 Lib。

如：`"ai.ling.lib:im-messager-nimAndAliyunMqtt:0.1.0-rc.22";`



![SDK 的架构图](./../resources/message-plugin-box.png)



# 语言能力

英语：读写能力 良好	|	听说能力 良好



# 专业技能：

Android 领域开发： 精通，Kotlin，Java， JNI， C++&emsp;&emsp;&emsp;&emsp;「语言熟悉度，按顺序排列」

iOS 领域开发：精通，Swift， OC， C++&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;「语言熟悉度，按顺序排列」

Linux 程序：熟悉， C++