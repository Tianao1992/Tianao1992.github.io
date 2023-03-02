---
title: "test"
layout: about
---

# 联系方式

- 手机：17778078207
- Email：tianao1992@163.com
------

# 个人信息
- 田傲/男/1992
- 工作年限：7年
- GitHub：https://github.com/tianao1992
- 博客：https://tianao1992.github.io/
- 期望职位：iOS高级开发工程师
- 目前状态：在职

------
# 技能清单

- 熟练掌握Objective-c、swift、js等开发语言
- 熟练使用壳子化(多target)实现不同项目需求。
- 熟练掌握MVC、MVVM等主流。熟悉代理、单利、责任链、观察者等设计模式
- 熟练掌握cocoapods组件化、静态库制作
- 能通过多线程技术GCD,NSOperation,NSThread 解决项目中问题
- 熟练掌握runtime&runloop。
- 熟练掌握JavaScriptCore,WKWebView 与h5 交互。
- 熟练使用SVN、Git等代码管理工具，崇尚Git。
- 熟练掌握 FMDB、Plist、NSUserdefaults等数据持久化方法。
- 熟练使用.crash文件与第三方bugly定位崩溃问题。
- 熟练掌握vue全家桶、webpack
---

# 工作经历

## 天狮集团 (2021.3-至今)

负责<实亿趣><starshow><今天买买>移动端开发； 
负责电商直播项目主播端任务分发与排期；
负责项目架构分层,功能模块间解耦；
负责starshow主播端项目代码分支管理pod库版本管理；
负责功能版本前期技术调研

## 北京容联云通讯（2017.6-2020.6）

负责<容信><容视><有会>移动端开发；  
负责iOS端的架构与性能优化；  
负责开发与维护iOS中间层SDK；  
根据项目需求开发IM插件，白板插件，会议插件等；  
负责编写相关技术接口文档；  
负责<有会>flutter集成;  

## 北京征晨文化传播有限公司（2015.11-2017.6）

负责<户外猩球>iOS端开发与版本迭代；  
负责iOS端的架构与性能优化；  
负责iOS端系统适配/错误崩溃捕捉工具；  
负责H5相关技术混合开发应用；  
负责官网与管理后台的开发与维护  

## 上海永琪美容美发有限公司（2015.03—2015.09）

负责<大头娃娃>iOS端架构与研发；  
负责代码迁移，增添第三方管理工具  

## 郑州文鑫科技有限公司（2014.11—2015.03）

负责<吃啥呀> iOS端开发与版本迭代;
负责相关功能端技术调研与代码review； 

------

# 项目列表

## starShow(swift)
*今天买买电商直播项目主播端应用*
### 相关技术
* 基于腾讯TRTC封装上层业务实现开播、美颜、视频连麦互动等功能
* 应用组件化通信基于CTMediator，Module_Extension模块解耦
* 直播组件库(swift)与TRTCLiveRoom相关oc文件(xxx-umbrella.h)混编
* 壳子化工程项目针对不同需求输出不同包，添加scheme配置环境变量
* 国际化实现，本地语言资源json文件添加Bundle
***
## 今天买买(swift)
*天狮集团国内电商项目应用*
### 相关技术

* 首页模块针对不同功能活动cms小组件生成维护强业务逻辑
* 应用瘦身(153M->134M)包含图片资源清理、工程配置、冗余库整理
* 前端交互通用wkwebview类封装，及中间对象生成避免内存泄漏
* RealmSwift、bigdata 项目核心数据埋点
* HandyJSON、Kingfisher、SnapKit 等三方集成
***
## 容视（iOS）
*实时视频会议的APP，包含会议白板、IM、遥控硬件等功能；*
### 相关技术

* 多target（单工程，输出RS、YH）；
* cocopods 私有组件库制作、静态库生成提升编译速度；
* 多bundle,图片,音频文件等资源管理；
* LBXScan扫码集成, method-swizzing解决占用cpu内存过高；
* 多window处理,屏幕旋转切换window相关问题；
* 会议摘要富文本编辑及展示；
* 国际化，暗黑模式，日志上传等功能；
***
## 有会（iOS）
*基于公司媒体库研发的一款音视频会议应用*
### 相关技术

* 音视频能力、im、白板等SDK集成与维护；
* 遥控硬件部分页面使用flutter混合开发；
* 会中横竖屏处理、会管、会控、成员状态变更；
* 本地.crash文件分析与三方bugly集成；
***
## 容信（iOS）
*即时通讯IM产品、封装功能模块SDK、满足企业定制化需求*
### 相关技术

* 工程管理模块AppMode交互SDK、runtime通讯子工程插件
* 群文件分页请求及dispatch_group_t 使用
* im消息列表UITableview 性能优化
* wkwebview与js 交互，解决内存问题
* 基于基线版本开发定制化产品；
***
## 户外猩球（iOS）
*户外见闻文章、训练营、夏令营*
### 相关技术

* UIWebView NSURLProtocol 实现图片缓存及代理鉴权
* AVAssetExportSession 对视频的压缩 PBJVision 实现视频的录制及
* 环信im、支付宝支付、微信支付、等SDK集成
* JavaScriptCore 与 js 进行交互
* 自定义 CollectionViewDataSource 对页面的灵活布局
* 七牛云本地图片批量上传
***
## 大头娃娃（iOS）
*美发预约、技师介绍、门店简介*
### 相关技术

* AFNetWorking 网络请求模块封装、网络状态监测
* 极光 SDK,百度 SDK 集成,coocpods 管理第三方
* NSUserDefault,plist,fmdb 进行数据的持久化
* 分享链接跳转应用指定页面
* 使用友盟SDK分享到微信、QQ、微博等

***
## 有会官网（H5）
*针对会议产品展示、政企合作展示等*
### 相关技术

* Vue、webpack、Axios、Element UI；
---
## 户外猩球SMS后台（H5）
*户外猩球后台管理、订单、教官信息、装备管理*
### 相关技术
* jQuery、require.js 、backbone.js，axjax
------

