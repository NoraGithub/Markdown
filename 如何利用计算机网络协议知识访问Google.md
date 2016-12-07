---
title: 如何利用计算机网络协议知识访问Google
date: 2016-09-29 15:47:08
tags:
layout: draft
---

留坑，关于路由翻墙。

---

<!-- more -->

# 网络访问的简单模型


[#reference-前端经典面试题: 从输入URL到页面加载发生了什么？](https://segmentfault.com/a/1190000006879700)

当我们在浏览网页的时候，计算机做了什么？
1. 在计算机眼里，没有域名概念，只有IP概念。当我们输入域名的时候，其实计算机做了一次 DNS查询，把域名和IP的映射（一个域名对应多个IP）获取并访问。
![域名查询](http://upload-images.jianshu.io/upload_images/544981-909ab11f316bcc9c.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
2. 和服务器（指定IP机器）进行“三次握手”后展示网页内容。
![“三次握手”建立连接](http://upload-images.jianshu.io/upload_images/544981-a13bc6b207f8d84d.gif?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
（略）TCP“四次挥手”结束访问
（略）SSL/TLS“四次握手”
（略）![“四次握手”建立连接](...)

## 提炼问题

所以如果我们需要浏览Google，要做好两点。
1.  如何获取正确的IP地址（防止DNS污染）
2.  保护好网页内容。（可以检查ip包内数据然后评比访问）

好的。[OpenWrt智能、自动、透明翻墙路由器教程 ](https://github.com/softwaredownload/openwrt-fanqiang)应该是参考[Shadowsocks + GfwList 实现 OpenWRT 路由器自动翻墙](https://cokebar.info/archives/962)的DNS方案二进行实现。


1. 如何防治DNS污染？
	a.  维护一个域名白名单list，白名单外域名请求自己vps，白名单请求114DNS
	b.  维护一个域名黑名单list，黑名单域名请求自己vps，黑名单外114DNS
	c.  借助ChinaDNS作为上游服务器，判断dns
2. 如何保护网页内容？
	针对ip决定tcp请求的访问通道。




# 计算机网络模型
## TCP/IP 网络模型（五层）
## OSI 网络模型（七层）
### 应用层
DNS,IP,HTTP
### 传输层
TCP UDP
### 网络层
###Advanced



# 问题

 - 
 - 
 - 
 - 


# 解决方案

openwrt本质是什么呢？是翻墙的路由器fanqiang解决方案，一套基于路由器硬件的Linux OS。它整合了一系列基于计算机网络的小工具。比如shadowsocks，dnsmasq，iptable(防火墙进行转发功能）

dnamasq：dns
iptable：防火墙（ip包过滤规则）
chinadns
chnrouter:区分国内外ip段
Shadowsocks是一个轻量级socks5代理，以python写成；

码农对于shadowsocks应该不陌生，而一般普通网民可能知之甚少。shadowsocks实质上也是一种socks5代理服务，类似于ssh代理。与vpn的全局代理不同，shadowsocks仅针对浏览器代理，不能代理应用软件，比如youtube、twitter客户端软件。如果把vpn比喻为一把屠龙刀，那么shadowsocks就是一把瑞士军刀，轻巧方便，功能却非常强大。

这里发布的shadowsocks分为两个版本，说明如下：
###shadowsocks-libev
官方原版包含 ss-{local,redir,tunnel} 三个可执行文件。默认启动 ss-local 建立本地SOCKS代理
###shadowsocks-libev-spec 
针对OpenWrt 路由器的优化版本包含 ss-{redir,rules,tunnel} 三个可执行文件。

###ss-tunnel###
做 DNS 查询转发，ss-tunnel 默认转发127.0.0.1:5353 至 8.8.4.4:53 通过 ShadowSocks 服务器查询 DNS 用于线路优化。ss-rules 可设置 ignore.list 中的 IP 流量不走代理。
###ss-redir###
建立透明代理, 做tcp转发。

![配置文件目录树](http://upload-images.jianshu.io/upload_images/544981-1629851c9094a57f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![配置文件目录树](http://upload-images.jianshu.io/upload_images/544981-69a62d3c96c71642.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# Advanced

1、apn是什么？
2、as、bgp概念
3、什么是骨干路由
4、openwrt（嵌入式框架，固件）（主流路由器固件有 dd-wrt,tomato,openwrt三类）。固件是嵌入在硬件设备中的软件，类似操作系统？
5、opkg（包管理工具，适用于嵌入式。类似apt/dpkg）
## BGP







国外：Client–dnsmasq–ChinaDNS–shadowsocks–代理服务器–GoogleDNS
国内：Client–dnsmasq–ChinaDNS–114DNS

域名和空间没有必然联系，域名的作用就是作为一个字符串映射到一个IP地址上，因为（1）IP地址太难记了（2）IP地址数目有限（同一个IP上可以放N个域名）所以才需要域名这么个东西。这就意味着，你有换空间的**自由**。哪天对空间服务商不高兴了，可以直接把他踹了，把域名解析到别家去，用另一家空间。哎哎，等会儿，啥叫**域名解析**？

域名服务器？
name.server
a记录mx纪录cname纪录

shadowsocks-libev:shadowsocks-libev 是一个 shadowsocks 协议的轻量级实现，是 shadowsocks-android, shadowsocks-ios 以及 shadowsocks-openwrt 的上游项目。其具有以下特点：
openssl
polarssl:嵌入式设备的精简ssl






## DNS 抢答机制#
PAC代理本质其实就是传统的HTTP代理?


![端口关联的理解](http://upload-images.jianshu.io/upload_images/544981-4ebf6537ab4976d3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)





Reference:
[翻墙路由器的原理与实现](http://drops.wooyun.org/papers/10177)
[linux平台下防火墙iptables原理(转)](http://www.cnblogs.com/ggjucheng/archive/2012/08/19/2646466.html)
[匿名、透明、HTTP、SSL、SOCKS代理的区别](http://blog.mimvp.com/2015/03/anonymous-transparent-the-difference-between-http-ssl-socks-proxy/)

·SSH是标准的网络协议，可用于大多数UNIX操作系统，能够实现字符界面的远程登录管理，它默认使用22号端口，采用密文的形式在网络中传输数据，相对于通过明文传输的Telnet，具有更高的安全性。

## 路由选择算法
### Dijkstra Algorithm
### dv



