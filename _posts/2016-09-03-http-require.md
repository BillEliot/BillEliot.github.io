---
layout: post
title: "Http请求详细过程"
desc: "Http请求详细过程"
keywords: [Http]
tags: [Http]
categories: [Html]
---

**Http是一个应用层协议，只是一种通讯规范，也就是双方要进行通信需要事先约定一个规范**  
*当我们在浏览器输入www.baidu.com的时候：*  
1. **连接** 当我们进行这样一个请求时，首先要建立一个Socket连接，因为Socket是通过IP和端口建立的，所以在这之前还要进行DNS解析过程，也就是将 www.baidu.com 解析成IP，如果URL里面不包含端口号，则会使用该协议的默认端口号。(*www.baidu.com -> www.baidu.com:8080*)  
2. **请求** 连接成功建立后，浏览器开始向服务器发送请求，该请求一般是GET或POST命令。  
3. **应答** WEB服务器收到请求后进行处理，找到相应文件后会将文件内容传给浏览器。  
为了告知浏览器，WEB服务器首先会传一些HTTP头信息，之后再传具体内容。  
(常见的HTTP头信息：  
* HTTP 1.0 200 OK 列出服务器正在运行的HTTP版本和应答代码  
* MIME_Version:1.0 它指示MIME类型的版本  
* content_type:类型 指示HTTP体信息的MIME类型(如 content_type:text/html 表示传送数据是HTML文档)  
* content_length:长度 指示HTTP体信息的长度  
)  
4.**关闭连接** 应答结束后，Web服务器与浏览器必须断开连接，以保证其他浏览器与WEB服务器建立连接。  

## 数据包在网络中的漫游历程

在网络分层结构中，各层之间是**严格单向依赖**的。  
*服务*是用来描述各层之间关系的抽象概念，即网络层中各层向紧邻上层提供的一组操作，下层是服务提供者，上层是请求服务的用户。  

**应用层**  
