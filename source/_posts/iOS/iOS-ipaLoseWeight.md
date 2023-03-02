---
title: iOS应用瘦身
date: 2022-01-11 10:18:07
index_img: /img/ios/apple.jpg
categories:
- iOS
tags:
- iOS
---

### 摘要
APP 大小限制是指 App 压缩包（也就是 .ipa 文件）所占的空间，用户在下载 App 时，下载的是压缩包，这样做可以节省流量；当压缩包下载完成后，就会自动解压，解压过程也就是通常所说的安装过程；安装大小就是指压缩包解压后所占用的空间

虽然苹果历年都会调整 App 下载大小，由之前的 100M 到后来的 150M 再到现在的 200M。如今，App 下载大小超出 200 MB 时 ，会出现两种情况：
- iOS 13 以下的用户，无法通过蜂窝数据下载 App；
- iOS 13 及以上的用户，需要手动设置才可以使用蜂窝网络下载 App。
Apple __TEXT 段大小限制：

- iOS 7 之前，二进制文件中所有的 __TEXT 段总和不得超过 80 MB；
- iOS 7.X 至 iOS 8.X，二进制文件中，每个特定架构中的 __TEXT 段不得超过 60 MB；
-  iOS 9.0 之后，二进制文件中所有的 __TEXT 段总和不得超过 500 MB。

### 瘦身方向
- Mach-O 可执行文件
- 工程相关修改Build Setting
- 资源文件 
   1. 图片资源：Assets.car/bundle/png/jpg 等
   2. 视频 / 音频资源：mp4/mp3 等
   3. 静态网页资源：html/css/js 等
   4. 国际化文件 (json 或 string)

其实核心组成部分便是资源文件与Mach-O 可执行文件两部分，这两个部分便是我们的主要瘦身方向
#### 图片资源
 使用LSUnusedResources工具，导入项目查找无用图片
 ![](/img/ios/loseweight/LSUnusedResources.png)
 删除无用的1x图片,将项目中的2x、3x图片全部放到Asset文件中，苹果会使用App Thinning方式针对不同机型下发图片，并对图片进行压缩。
 将图片进行有损压缩，少量图片直接在tinypng进行压缩
 #### 无用类查找
 fui  https://github.com/dblock/fui 查找无用class
 由于这个工具还不是100%靠谱，可根据这个列表，在Xcode中手动检查并删除不再用到的类

### 编译设置
Xcode 支持编译器层面的一些优化选项，通过修改 Build Setting 的一些相关配置，可以让我们介于更快的编译速度和更小的二进制大小并且更快的执行速度之间自由选择想要进行的优化粒度，这些选项有的会影响资源文件，有的会影响可执行文件，因为内容比较多，所以起一个独立的章节描述。
这种方式的性价比很高，改动一项配置，就可能会带来收益，但是可能具有一定的风险，需要谨慎

#### Bitcode
Bitcode 是一个编译好的程序的中间表示形式（IR）。上传到 App Store Connect 中的包含 Bitcode 的 App 将会在 App store 中进行链接和编译。苹果会对包含 Bitcode 的二进制 app 进行二次优化，而不需要提交一个新的 app 版本到 app store 中。属于 Apple 内部的优化，但需要注意；
全部都要支持。我们所依赖的静态库、动态库、Cocoapods 管理的第三方库，都需要开启 Bitcode。否则打包会编译失败，具体错误会在 Xcode 中指出；
Crash 定位。开启 Bitcode 后最终生成的可执行文件是 Apple 自动生成的，同时会产生新的符号表文件，所以我们无法使用自己包生成的 DYSM 符号化文件来进行符号化，而是使用使用 Apple 生成的 DYSM 符号化文件；
Flutter 不支持 Bitcode，如果项目是包含 Flutter 框架的，就无法使用这种方式；
BitCode 在 iOS 开发中是可选的，在 watchOS 开发中是必须要选择的， Mac OS 是不支持 BitCode 的
开启方式：Build Settings -> Enable Bitcode -> 设置为 YES 


#### 无用架构去除
可以在 Build Setting - Excluded Architectures 项设置排除的架构
模拟器 64 位处理器测试需要 x86_64 架构，真机 32 位处理器需要 armv7, 或者 armv7s 架构，真机 64 位处理器需要 arm64 架构
理论上只保留 arm64 架构其实就够用了，可以去除 armv6 、 armv7 、 armv7s 三种架构
#### 使用链接时优化 LTO(Link-Time Optimization)
可以在 Build Setting - Link-Time Optimization 项设置优化方式
LTO 会降低编译链接的速度，所以建议在打正式包时开启；开启了 LTO 之后，Link Map 的可读性明显降低，多出了很多数字开头的类（LTO 的全局优化导致的），所以如果需要阅读 Link Map，可以先关闭 LTO

### Mach-O 可执行文件

iOS 可执行文件是 Mach-O 格式，主要由 Header、Load Commands、Data 三部分。
可以简单认为：
Header 描述了文件的大概信息。
Load Commands 由多条 Load Command 组成，它们描述了 Data 在二进制文件和虚拟内存中的布局信息，有了这个布局信息就能够知道 Data 在二进制文件中和虚拟内存中是怎样排布的，它相当于修房子时的图纸一样。
Data 存储了实际的内容，主要是程序的指令和数据，它们的排布完全依照 Load Commands 的描述。
Mach-O 文件中的 Data 部分主要是以 Segment（中文翻译为段）和 Section （中文翻译为节）的方式来组织内容的，好比学校中有年级和班级、公司中有部门和小组一样，把有共同特点的内容组织到一块，可以方便管理，提高效率。
使用 $ xcrun size -lm <binary-path> 指令可以查看 Mach-O 文件 Data 部分的结构和各 Segment/Section 的大小信息（该 Mach-O 文件由 Xcode 的 iOS App 模板工程构建而来）。在不需要更详细的信息时，这条命令很方便


在对 Mach-O 文件进行瘦身优化时，我们可以通过分析 Link Map 文件来给我们一定的数据参考，帮助我们分析 Mach-O 文件的构成。
Link Map 是编译链接时可以生成的一个 txt 文件，它生成目的就是帮助程序员分析包大小。Link Map 记录了每个方法在当前的二进制架构下占据的空间。通过分析 Link Map，我们可以了解每个类甚至每个方法占据了多少安装包空间。
开启 Build setting 中的 Write Link Map File 开关，Xcode 就会生成一份 Link Map 文件。其中生成的 Link Map 文件路径如下：~/Developer/Xcode/DerivedData/项目/Build/Intermediates.noindex/项目.build/Debug-iphonesimulator/项目.build/项目-LinkMap-normal-x86_64.txt
如果直接阅读 Link Map 文件，效率会比较低，也不直观，我们可以使用一些工具帮助我们分析
 ![](/img/ios/loseweight/linkMap.png)










