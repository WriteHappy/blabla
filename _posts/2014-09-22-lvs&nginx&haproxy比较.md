---
layout: post
title: lvs,nginx,haproxy比较
category: operation
---
[原文出处](http://blog.chinaunix.net/uid-20485483-id-3084299.html)

##lvs的特点##
* 抗负载能力强、是工作在网络4层之上且仅做分发只用，没有流量产生，这个特点也是它在负载均衡这个圈子里面性能最强劲的；
* 可配置性低，这是双刃剑：缺点是没有太多可配置的东西，达不到控制的细化；优点是运维同学爽了，既然配置的东西少了，那就减少了人为出错的几率；
* 软件本身不支持正则处理，不能做动静分离，这个就比较遗憾了;其实现在许多网站在这方面都有较强的需求，这个是Nginx/HAProxy+Keepalived的优势所在。

##haproxy的特点##
* haproxy支持虚拟机
* 支持session的保持，cookie引导等工作
* 并发性上优于nginx，缺不如lvs
* haproxy可以对mysql进行负载均衡
* haproxy的大致包含以下几种：

		roundrobin，简单的轮询
		static-rr，根据权重进行负载均衡
		leastconn,最少连接者优先处理
		source，根据请求源ip，和nginx的ip_hash机制类似，
		uri，根据请求的URI
		uri_param,根据请求的URI参数
		hdr（name），根据http请求头来锁定
		rdp-cookie（name），根据cookie来锁定并hash每次的TCP请求。（十分有用）

##nginx的特点##
* 工作在网络7层协议之上，针对http应用做的分流策略，比如针对域名、目录结构，它的正则比haproxy更为强大和灵活。
* nginx对网络的依赖很小，能ping通就能进行负载分流，而haproxy除了要ping通还要进行keep alive的测试
* nginx的安装和配置比较简单（相对于haproxy）
* Nginx可以通过端口检测到服务器内部的故障，比如根据服务器处理网页返回的状态码、超时等等，并且会把返回错误的请求重新提交到另一个节点，不过其中缺点就是不支持url来检测（haproxy是支持url检测的）;
