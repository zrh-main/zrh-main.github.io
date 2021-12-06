---
title: hexo插入图片
categories: hexo
date: 2021-12-06 10:31:16
tags:
---


## HEXO插入图片

### 安装
  ``` bash
  npm install hexo-asset-image --save
  ```
### 修改_config.yml配置
> 打开hexo的配置文件_config.yml找到 post_asset_folder，把这个选项从false改成true
### 替换插件源码
> 打开/node_modules/hexo-asset-image/index.js替换为如下代码:
``` JavaScript
'use strict';var cheerio=require('cheerio');function getPosition(str,m,i){return str.split(m,i).join(m).length}var version=String(hexo.version).split('.');hexo.extend.filter.register('after_post_render',function(data){var config=hexo.config;if(config.post_asset_folder){var link=data.permalink;if(version.length>0&&Number(version[0])==3)var beginPos=getPosition(link,'/',1)+1;else var beginPos=getPosition(link,'/',3)+1;var endPos=link.lastIndexOf('/')+1;link=link.substring(beginPos,endPos);var toprocess=['excerpt','more','content'];for(var i=0;i<toprocess.length;i++){var key=toprocess[i];var $=cheerio.load(data[key],{ignoreWhitespace:false,xmlMode:false,lowerCaseTags:false,decodeEntities:false});$('img').each(function(){if($(this).attr('src')){var src=$(this).attr('src').replace('\\','/');if(!/http[s]*.*|\/\/.*/.test(src)&&!/^\s*\//.test(src)){var linkArray=link.split('/').filter(function(elem){return elem!=''});var srcArray=src.split('/').filter(function(elem){return elem!=''&&elem!='.'});if(srcArray.length>1)srcArray.shift();src=srcArray.join('/');$(this).attr('src',config.root+link+src);console.info&&console.info("update link as:-->"+config.root+link+src)}}else{console.info&&console.info("no src attr, skipped...");console.info&&console.info($(this))}});data[key]=$.html()}}});
```
### 使用
> 现在可以插入图片了，
> 在hexo new post photo之后就在source/_posts生成photo.md文件和photo文件夹，把要插入的图片复制到photo文件夹内，在photo.md文件里面按markdown的标准写,（我的文件名是head.jpeg）
> 比如: &nbsp;&nbsp;&nbsp;\!\[代替图片的文字\]\(head.jpeg\)