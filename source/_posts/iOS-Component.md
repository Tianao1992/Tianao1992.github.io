---
title: iOS组件化
date: 2021-04-30 14:01:59
index_img: /img/ios/apple.jpg
categories:
- iOS
tags:
- iOS
---
### 组件化需求
随着公司业务的不断发展，项目的功能越来越复杂，各个业务代码耦合越来越多，代码量急剧增加，传统的 MVC 或者 MVVM 架构已经无法高效的管理工程代码，因此需要用一种技术来更好地管理工程，而组件化是一种能够解决代码耦合的技术。项目经过组件化的拆分，不仅可以解决代码耦合的问题，还可以增强代码的复用性，工程的易管理性等。

组件化是项目架构层面的技术，不是所有项目都适合组件化，组件化一般针对的是大中型的项目，并且是多人开发。如果，项目比较小，开发人员比较少，确实不太适合组件化，因为这时的组件化可能带来的不是便捷，而是增加了开发的工作量。另外，组件化过程也要考虑团队的情况，总之，根据目前项目的情况作出最合适的技术选型

### 组件库层级划分

![](/img/ios/component/ceji.png)

### 远端索引库制作

#### 远端索引库作用描述 
1. 第三方代码管理 
2. 平台创建框架描述spec，包含：框架名称、版本、真正代码存放地址=远程地址 
3. 上传spec到pod的远程索引库

#### 远端索引库制作

1. 创建git仓库作为.spec 远端私有库

2. 终端 pod repo list 查看本地私有库

3. pod repo add xxx 添加本地私有库

4. 本地索引库还有一个检索索引文件，当添加第三方库的时候先去检索索引文件查找，再去本地索引库查找，然后再去真正的代码存放地址去下载第三方库

####  远端私有库,本地库,pod库关系

![](/img/ios/component/podlib.png)

1. 创建自己的私有远程索引库
2. 拉取私有远程索引库到本地
3. 将自己的私有框架代码上传私有库中
4. 将私有框架代码中的spec文件上传私有远程索引库
5. 使用时，在Podfile添加source，下载过程同cocoapods


### pod库制作相关
1.  pod lib create xxx 编辑podspec文件

![](/img/ios/component/podcreate.png)

2. 编辑.podspec文件添加对应版本信息及homepage, source 
3. git add .
4. git commit -m "xx"
5. git push 
6. git tag xxx
7. git push --tags
8. pod lib lint  本地验证
9. pod spec lint 远程验证
10. 推送到私有索引库 pod repo push TAComponentSpec xxx.podspec--allow-warnings

### pod私有库引入工程

工程podfile文件头部指定 source路径 pod 引入私有库 pod install 引入工程
![](/img/ios/component/source.png)


### 组件库交互(CTMediator)

![](/img/ios/component/ctMediat.png)

**target**


```
- (UIViewController *)Action_nativeFetchDetailViewController:(NSDictionary *)params;
```

**CTMediator分类**
```
// CTMediator+CTMediatorModuleAActions.h

- (UIViewController *)CTMediator_viewControllerForDetail;

// CTMediator+CTMediatorModuleAActions.m

- (UIViewController *)CTMediator_viewControllerForDetail
{
	return [self performTarget:kCTMediatorTargetA action:kCTMediatorActionNativFetchDetailViewController params:@{@"key":@"value"} shouldCacheTarget:NO];
}
```

**调用**

```
/ ViewController.h
#import "CTMediator+CTMediatorModuleAActions.h"

[self presentViewController:[[CTMediator sharedInstance] CTMediator_viewControllerForDetail] animated:YES completion:nil];

```
从以上代码可以看出，使用者只需要依赖中间件，而中间件又不依赖组件，这是真正意义上的解耦






