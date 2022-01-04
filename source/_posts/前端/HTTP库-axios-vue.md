---
title: HTTP库-axios-vue
tags:
  - axios
  - vue
categories: 前端
date: 2021-11-15 22:22:20
---


## 简介
Axios 是一个基于 promise 的 HTTP 库，可以用在浏览器和 node.js 中
## 安装
使用 npm:

``` bash
$ npm install axios
```

使用 cdn:
``` html
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
``` 
## vue中使用axios
1. 安装
  ``` bash
  $ npm install axios
  ```
  安装qs库,修改post请求的编码数据
  ``` bash
  $ npm install qs
  ```
2. 创建src/plugins/axios.js配置文件;内容如下:
``` JavaScript
"use strict";
import axios from "axios";
import qs from 'qs'//转换数据类型;post请求需要转换字符串类型
// cors跨域 如果浏览器开启cookie,则使用withCredentials启用跨域验证
if (navigator.cookieEnabled) axios.defaults.withCredentials = true
axios.defaults.headers.post['Content-Type'] = 'application/x-www-form-urlencoded';
//全局配置
let config = {
  baseURL: process.env.VUE_APP_BASE_URL,
  timeout: 3000, // Timeout
  // withCredentials: true, // Check cross-site Access-Control
};
//创建axios实例
const _axios = axios.create(config);
//请求拦截
_axios.interceptors.request.use(
  config => {
    //post请求需要转换字符串类型,在请求拦截中封装
    if (config.method === "post") {
      config.data = qs.stringify(config.data)
    }
    if (sessionStorage.getItem("manageToken")) {
      config.headers.Authorization = sessionStorage.getItem("manageToken")
    }
    return config;
  },
  error => Promise.reject(error)
);
// 响应拦截
// Add a response interceptor
_axios.interceptors.response.use(
  response => response.status === 200 || response.status === 304 ? Promise.resolve(response.data) : Promise.reject(response),
  error => {// Do something with response error
    const { response } = error;
    if (response) {
      errorHandle(response.status, response.data);
      return Promise.reject(error);
    }
    console.log("连接失败,请检查网络!!!")
  }
);
// 响应状态码的错误处理
const errorHandle = (status, info) => {
  switch (status) {
    case 400:
      console.log("语法错误")
      break;
    case 401:
      console.log("请进行身份认证")
      break
    case 403:
      console.log("服务器理解请求客户端的请求，但是拒绝执行此请求            ")
      break
    case 404:
      console.log("服务器无法根据客户端的请求找到资源（网页）。")
      break
    default:
      console.log(info)
  }
}
// Plugin.install = function (Vue) {
//   Vue.axios = _axios;
//   window.axios = _axios;
//   Object.defineProperties(Vue.prototype, {
//     axios: { get() { return _axios } },
//     $axios: { get() { return _axios } },
//   });
// };
export default _axios;
```
3. 创建src/api/api.js接口文件,引入配置的axios并创建请求函数;内容如下:
  ``` JavaScript
  import axios from "@/plugins/axios"
  // post
  const apiPost = data => axios({ url: 'users', method: 'post', data })
  // delete
  const apiDelete = data => axios({ url: `users/${data}`, method: 'delete' })
  // put
  const apiPut = data => axios({ url: `users/${data.id}/state/${data.status}`, method: 'put'})
  // get
  const apiGet = () => axios({ url: 'rights/list' })
  const apiGet2 = data => axios({ url: `rights/list?${data}`})
  export {
    apiPost,
    apiDelete,
    apiPut,
    apiGet,
    apiGet2
  }
  ```
4. 引入api.js接口文件,在vue页面中使用请求函数
``` JavaScript
import { apiPost, apiDelete, apiPut,apiGet } from '@/api/api'
export default {
  data() {return { } },
  methods: {
    post: async function () {
      let res = await apiPost()
      if (res.code !== 200) {
        console.log(res.msg)
        return
      }
      console.log(res.data)
    }
  }
}
```