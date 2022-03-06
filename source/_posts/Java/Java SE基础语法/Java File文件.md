---
title: 文件
date: 2022-2-10 16:56:43
tags:
  - Java SE基础语法
categories: Java
---

## 概述
  File类文件和目录路径名的抽象表示形式。里面有一些文件和目录(文件夹)的相关	操作。
## 构造方法
  - `public File(String pathname)` pathname表示文件的路径
  - `public File(String parent, String child)` parent表示文件的位置，child表示文件名
  - `public File(File parent, String child)`parent表示文件路径的file对象，child表示文件名
## 常用方法
### 获取功能方法：
- public String getAbsolutePath() 
获取绝对路径
- public String getPath()  
获取路径(构造里是什么，就会获取什么)
- public String getName()  		
获取路径最后一串内容，一般是文件名
- public long length()
获取文件的大小（目录没有大小）

### 判断功能方法：
- public boolean exists()
判断文件或目录是否存在
- public boolean isDirectory()
判断该对象是否为目录
- public boolean isFile()
判断该对象是否为文件
### 创建删除功能的方法：
- public boolean createNewFile()
创建新的文件
- public boolean delete()  
删除文件或非空目录
- public boolean mkdir()
创建新目录，如果父目录不存在，则创建失败
- public boolean mkdirs()
创建多级目录，如果父目录不存在，则创建父目录
### 目录的遍历（下面两个方法必须要确保file对象是一个目录）
- public String[] list()
获取目录中所有的内容名称，存储到一个字符串数组中。
- public File[] listFiles()
获取目录中所有的内容，存储到一个文件数组中。