---
title: 简介
tags:
  - 全文检索
categories:
  - Java
---



## ES 的两种连接方式

Transport 连接 ，默认端口 9300
这种客户端连接方式是直接连接 ES 的节点，使用 TCP 的方式进行连接

REST API ，默认端口 9200
这种客户端的连接方式是 RESTful 风格的，使用 http 的方式进行连接。其接受参数和响应均和 Transport 方式一致。

推荐使用 REST 的方式调用功能。