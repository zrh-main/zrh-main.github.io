---
title: node版本管理工具-nvmw
tags:
  - node
categories: 后端
date: 2021-11-15 21:32:03
---


## 介绍
nvmw 是 Windows 下的 node 管理工具
<font color='red'>已经安装node了，则需要先卸载</font>

## nvmw常用操作
### 全局安装
``` bash
npm install -g nvmw
```
### 查看nvmw版本
``` bash
nvmw -V
```
### nvmw下载不同的node版本
命令：
``` bash
nvmw install v版本号
```
例：`nvmw install v12.13.0`
### nvmw当前终端切换node版本
命令：
``` bash
nvmw use  v版本号
```
例：`nvmw use v12.13.0`
### nvmw永久切换node版本
命令：
``` bash
nvmw switch v版本号
```
例：`nvmw switch  v12.13.0`
### nvmw查看node版本
``` bash
nvmw ls
```

