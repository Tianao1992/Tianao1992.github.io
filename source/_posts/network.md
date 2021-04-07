---
title: 网络相关
date: 2020-2-15 19:05:40
index_img: /img/bg/tree.jpg
categories:
- ItTree
tags:
- basic
---

### 概括: http协议 、https网络安全、tcp/upd、DNS解析、Session/Cookie

### 网络七层协议

![](/img/ittree/network/networkqc.png)

### http协议介绍

HTTP协议（HyperText Transfer Protocol，超文本传输协议）是因特网上应用最为广泛的一种网络传输协议，所有的WWW文件都必须遵守这个标准。
HTTP是一个基于TCP/IP通信协议来传递数据（HTML 文件, 图片文件, 查询结果等

#### http请求报文
客户端发送一个HTTP请求到服务器的请求消息包括以下格式：请求行（request line）、请求头部（header）、空行和请求数据四个部分组成，下图给出了请求报文的一般格式

![](/img/ittree/network/httprequset.png)


#### http 响应报文
HTTP响应也由四个部分组成，分别是：状态行、消息报头、空行和响应正文

![](/img/ittree/network/httprsp.jpg)

#### http 请求方法
get、post、head、options、put、delete 等


#### GET 和POST方式的区别

- get请求参数以？分割拼接到URL后面，POST请求参数在body里面

- get参数长度限制2048个字符，post一般没有改限制

- get请求不安全， post请求比较安全

从语义角度来回答

get  是获取资源 安全的 幂等的 可缓存的

post 是处理资源 非安全 非幂等的 不可缓存


安全性: 不应该引起server端的任何状态变化

幂等性: 同一个请求方法执行多次和执行一次的效果完全相同

### tcp连接 三次握手 四次挥手

如图

![](/img/ittree/network/tcplink.png)

#### 为什么要三次握手

为了防止服务器端开启一些无用的连接增加服务器开销以及防止已失效的连接请求报文段突然又传送到了服务端，因而产生错误。

由于网络传输是有延时的(要通过网络光纤和各种中间代理服务器)，在传输的过程中，比如客户端发起了SYN=1创建连接的请求(第一次握手)。

如果服务器端就直接创建了这个连接并返回包含SYN、ACK和Seq等内容的数据包给客户端，这个数据包因为网络传输的原因丢失了，丢失之后客户端就一直没有接收到服务器返回的数据包。

客户端可能设置了一个超时时间，时间到了就关闭了连接创建的请求。再重新发出创建连接的请求，而服务器端是不知道的，如果没有第三次握手告诉服务器端客户端收的到服务器端传输的数据的话，

服务器端是不知道客户端有没有接收到服务器端返回的信息的

#### 为什么要四次挥手

TCP释放连接时之所以需要“四次挥手”,是因为FIN释放连接报文与ACK确认接收报文是分别由第二次和第三次"握手"传输的。为何建立连接时一起传输，释放连接时却要分开传输？

建立连接时，被动方服务器端结束CLOSED阶段进入“握手”阶段并不需要任何准备，可以直接返回SYN和ACK报文，开始建立连接。
释放连接时，被动方服务器，突然收到主动方客户端释放连接的请求时并不能立即释放连接，因为还有必要的数据需要处理，所以服务器先返回ACK确认收到报文，经过CLOSE-WAIT阶段准备好释放连接之后，才能返回FIN释放连接报文


### https

#### https 介绍

基于HTTP协议，通过SSL或TLS提供加密处理数据、验证对方身份以及数据完整性保护

#### https 连接过程

![](/img/ittree/network/ssl.png)

客户端会发送给服务端一个加密算法的列表，tsl版本号 随机数c ,然后服务端再回给客户端
一个ssl证书和商定的加密算法，首先通过非对称加密进行对称加密密钥的传输，之后https之间的
网络请求就通过被非对称加密所保护的对称密钥进后续网络访问

#### https都使用了什么加密手段

- 建立连接过程使用非对称加密，非对称加密很耗时

- 后续通信过程使用对称加密

### tcp upd

TCP/IP 中有两个具有代表性的传输层协议，分别是 TCP 和 UDP

- TCP面向连接; UDP是无连接 的，即发送数据之前不需要建立连接

- TCP提供可靠的服务。也就是说，通过TCP连接传送的数据，无差错，不丢失，不重复，且按序到达;UDP尽最大努力交付，即不保证可靠交付

- tcp 连接是 1对1,UDP支持一对一，一对多，多对一和多对多的交互通信


#### tcp 特点

面向连接 可靠传输 面向字节流 流量控制 拥塞控制

可靠传输 停止等待协议

 无差错情况 超时重传 确认丢失 确认迟到

面向字节流

![](/img/ittree/network/bit.png)


流量控制 滑动窗口协议

![](/img/ittree/network/chuankou.png)

拥塞控制

慢开始、拥塞避免
快恢复、快重传

![](/img/ittree/network/yongse.png)

### DNS解析

域名到IP地址的映射，DNS解析请求采用UDP数据包且明文

![](/img/ittree/network/dns.png)

#### DNS劫持

#### DNS劫持与HTTP的关系是怎样的？
 没有关系，DNS解析发生在http建立连接之前，DNS解析请求使用UDP数据包，端口是 53

 #### 怎样解决DNS 劫持

- httpNDS 
- 长连接

httpDNS: 使用http协议向DNS服务器的80端口进行请求

长连接：通过长连接通道向 apiserver请求


### session/cookie

http协议是无状态的，使用 session 和 cookie 作为补偿

#### cookie

cookie 主要用来记录用户状态，区分用户，状态保存在客户端

怎样修改cookie?

- 新cooki覆盖旧cookie

- 覆盖规则: name、path、domain等需要与原cookie一致

怎样删除cookie?

设置 cookie的expires= 过去的一个时间点，或者maxage= 0

#### session 

sessoin 和 cookie的关系是怎样的？

- session需要依赖于cooKie机制


![](/img/ittree/network/session.png)
