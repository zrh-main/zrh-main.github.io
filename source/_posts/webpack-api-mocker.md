---
title: webpack-api-mocker
date: 2021-11-15 16:13:34
tags: 
  - 请求拦截,数据模拟
  - vue
categories: 前端
---

## 简介
> 拦截 Ajax 请求,自定义返回数据
> [npm地址](https://www.npmjs.com/package/webpack-api-mocker)
## Vue项目中使用webpack-api-mocker进行mock
### 安装webpack-api-mocker
``` bash
npm i webpack-api-mocker --save-dev
```
### 根目录编写 /mocker/index.js，用于定义mock接口
``` JavaScript
// 使用 require 引入json文件，可以直接访问数据
const appData = require('../data.json')
const proxy = {
    'GET /api/login': {
        success: appData.login.success,
        message: appData.login.message
    },
    'GET /api/list': [{
            id: 1,
            username: 'kenny',
            sex: 6
        },
        {
            id: 2,
            username: 'kenny',
            sex: 6
        }
    ],
    'POST /api/post': (req, res) => {
        res.send({
            status: 'error',
            code: 403
        });
    },
    'DELETE /api/remove': (req, res) => {
        res.send({
            status: 'ok',
            message: '删除成功！'
        });
    }
}
module.exports = proxy
```
### 修改 vue.config.js 配置文件（若不存在，在项目下新建即可）
``` JavaScript
const path = require('path')
const apiMocker = require('webpack-api-mocker')
module.exports = {
  devServer: {
    before(app) {
　　　　// 注意，此处引用的是自定义的接口文件
      apiMocker(app, path.resolve('./mocker/index.js'), {
        changeHost: true,
      })
    }
  }
}
```
### 定义json 数据（用于测试)根目录新增data.json编写模拟数据
``` JavaScript
{
    "login": {
      "success": "true",
      "message": "登陆成功"
    },
    "fileList": {
      "success":"true",
      "list":[
      {"fileId":"1","fileName":"a1.c","content":"content-test1"
      },
      {"fileId":"2","fileName":"a2.c","content":"content-test2"
      },
      {"fileId":"3","fileName":"a3.c","content":"content-test2"
      },
      {"fileId":"4","fileName":"a4.c","content":"content-test2"
      },
      {"fileId":"5","fileName":"a5.c","content":"content-test2"
      },
      {"fileId":"6","fileName":"a8.c","content":"content-test2"
      },
      {"fileId":"7","fileName":"a9.c","content":"content-test2"
      }]
    }
}
```