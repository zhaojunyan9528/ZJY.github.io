---
title: HTTP的请求头标签 If-Modified-Since与Last-Modified
tags:
  - 前端
categories:
  - - 笔记
    - Javascript
date: 2021-03-28 10:00:20
---

## HTTP的请求头标签 If-Modified-Since与Last-Modified

### 1.基本定义

If-Modified-since和last-modified都是http请求头中标签，用于记录页面的最后修改时间

### 2.发送方向

Last-Modified是服务器向客户端发送的http请求头标签
if-modified-since是由客户端向服务器发送的http请求他标签

### 3.应用场景

（1）last-modified:当浏览器第一次请求某个url时，服务端返回状态200，内容是请求的资源，同时有一个last-modifined属性标记此文件在服务端最后被修改的时间，格式：
Last-Modified: Fri, 12 May 2006 18:53:33 GMT
（2）if-modified-since: 客户端第二次请求此url时，根据http协议的规定，浏览器会向服务器发送一个if-modified-since报头，询问此文件是否被修改过，格式：
If-Modified-Since: Fri, 12 May 2006 18:53:33 GMT
后面时间是本地浏览器存储文件修改的时间

如果服务端的资源没有发生变化，则时间一致，自动返回状态码304，客户端接收到后就将本地文件显示到浏览器，这样就节省来传输流量

如果服务器资源发生改变或者重启服务器，时间不一致，就返回http状态码200和新的文件内容，客户端接收到后就丢弃旧的文件，把新文件缓存起来，并显示到浏览器中。

以上操作可以保证不向客户端发送重复资源，服务端资源发生变化时，客户端能得到最新的资源