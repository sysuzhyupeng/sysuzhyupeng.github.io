---
layout: post
title: 前端性能统计
description: 对前端性能统计进行讨论.
category: blog
---

前端性能统计
-
  网站的速度影响了用户访问网站最初的体验。试想，如果一个用户，在等待了若干秒后，还是停留在白屏的状态，那么他的选择将是离开这个网站。性能统计有助于帮我们检测网站的用户体验。  
  那么网站都有哪些指标？
  
* `首屏时间`。指一个网站被浏览器如IE窗口上的区域被充满所需时间。其实就是网页刚进入时，渲染完整个浏览器屏幕的时间。关于是否包含首屏所有的图片下载完成。这个网上有些争议，另一种说法是只要DOM+样式 都渲染完了，就算完成了。Chrome的Network可以清晰看到这个过程。
  
* `白屏时间`，页面处于空白的时间，通常影响白屏时间的多数是：DNS解析耗时+服务端耗时+网络传输耗时。(当然可能因为头部js阻塞页面影响)
  
* `用户可操作时间`。一般来讲`domready`时间，便是我们的用户可操作时间了。
  
* `总下载时`间, 页面总体的下载时间，所有的页面资源都下载完成。（window.onload）
  
如何统计这些指标  
-
  当然我们可以自己打开控制台，在页面的各个阶段，将时间打印出来，或者使用HTML5新增的接口：`performance`来进行评估。正常我们需要一个监控程序，去时刻提醒，现在网站的速度处于什么状况。所以，在代码中，增加统计，并把统计结果发送到服务器。在服务器采集这些日志，并产生一个监控的网站。
  
如何统计首屏时间
-
  对于网页高度小于屏幕的网站来说，统计首屏时间非常的简单，因为我们已经可以从performance中得到渲染开始时间`performance.timing.navigationStart`，只要在页面底部加上脚本打印当前时间即可（比如`http://localhost:8091/?action=speedlog`）