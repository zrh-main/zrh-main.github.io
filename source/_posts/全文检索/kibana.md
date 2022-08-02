---
title: kibana
tags:
  - 全文检索
categories:
  - Java
---

## 安装
需要与es同一版本

## 配置
位置:`./config/kibana.yml`

``` yml
# kibana服务端口号
server.port: 5601 
# 服务host, "0.0.0.0"允许同一局域网访问 
server.host: "0.0.0.0"
# 配置es地址hosts与url二选一,高版本使用hosts,低版本使用url

# elasticsearch.hosts: ["http://127.0.0.1:9201/"]
elasticsearch.url: "http://127.0.0.1:9201" 


# 界面语言使用中文
i18n.locale: "zh-CN"
```

