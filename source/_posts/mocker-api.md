---
title: mocker-api
tags:
  - '请求拦截,数据模拟'
  - vue
categories: 前端
date: 2021-11-27 21:13:29
---


## 简介
> 拦截 Ajax 请求,自定义返回数据
> [npm地址](https://www.npmjs.com/package/mocker-api)
## Vue项目中使用mocker-api进行mock
### 安装mocker-api
``` bash
npm i mocker-api --save-dev
```
### 根目录编写 /mocker/index.js，用于定义mock接口
``` JavaScript
// 使用 require 引入json文件，可以直接访问数据
const appData = require('../data.json')
const proxy = {
    'POST /api/userLogin':
        (req, res) => {
            res.send({
                code: 200,
                data: appData.userLogin,
            });
        },
    'GET /api/list': [
      { id: 1, username: 'kenny', sex: 6 },
      { id: 2, username: 'kenny', sex: 6 }
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
    },
    'GET /api/:owner/:repo/raw/:ref/(.*)': (req, res) => {
        const { owner, repo, ref } = req.params;
        // http://localhost:8081/api/admin/webpack-mock-api/raw/master/add/ddd.md
        // owner => admin
        // repo => webpack-mock-api
        // ref => master
        // req.params[0] => add/ddd.md
        return res.json({
          id: 1,
          owner, repo, ref,
          path: req.params[0]
        });
      },
      'POST /api/login/account': (req, res) => {
        const { password, username } = req.body;
        if (password === '888888' && username === 'admin') {
          return res.json({
            status: 'ok',
            code: 0,
            token: "sdfsdfsdfdsf",
            data: {
              id: 1,
              username: 'kenny',
              sex: 6
            }
          });
        } else {
          return res.status(403).json({
            status: 'error',
            code: 403
          });
        }
      },
      'DELETE /api/user/:id': (req, res) => {
        console.log('---->', req.body)
        console.log('---->', req.params.id)
        res.send({ status: 'ok', message: '删除成功！' });
      }
}
module.exports = proxy
```
### 修改 vue.config.js 配置文件（若不存在，在项目下新建即可）
``` JavaScript
const path = require('path')
const apiMocker = require('mocker-api')
module.exports = {
  devServer: {
    before(app) {
      // 1. 执行apiMocker,加载mock接口配置
      apiMocker(app, path.resolve('./mocker/index.js'), {
        proxy: {
          // 将路径字符串转成正则表达式
          '/repos/(.*)': 'https://api.github.com/',
        },
        // 重写目标的 url 路径。对象键将用作 RegExp 来匹配路径。
        pathRewrite: {
          '^/api/repos/': '/repos/',
        },
        changeHost: true,
        // 修改http-proxy选项
        httpProxy: {
          options: {
            ignorePath: true,
          },
          listeners: {
            proxyReq: function (proxyReq, req, res, options) {
              console.log('proxyReq');
            },
          },
        },
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