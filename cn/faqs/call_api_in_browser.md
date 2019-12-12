
---
title: 为什么无法通过浏览器调用云端录制 RESTful API
description: 浏览器调用, 浏览器, 调用, RESTful API, 调用 API
platform: All Platforms
updatedAt: Tue Nov 19 2019 10:42:48 GMT+0800 (CST)
---
# 为什么无法通过浏览器调用云端录制 RESTful API
要使用云端录制 RESTful API，Web API 需要发送跨域请求。根据 CORS 规范，浏览器针对跨域请求会先发送一个 OPTIONS 请求，查询服务器是否允许跨域请求，然后才有可能发起真正的 POST 请求。但是由于云端录制 RESTful API 不支持 OPTIONS 方法，所以无法支持 Web API 调用的方式。
