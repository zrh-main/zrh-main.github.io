---
title: Cookie与Session详解
date: 2022-01-03 19:49:57
tags:
  - Java EE网络应用
categories:
  Java
---

## 会话
  - 一次会话： 浏览器第一次给服务器发送请求，会话建立，直到有一方断开为止
  - 一次会话中包含多次请求和响应

### 功能
  - 在一次会话的范围内的多次请求中，共享数据


## Cookie
### 概述
  - 客户端会话技术 数据保存到客户端(浏览器)
### 使用
数据是创建cookie对象时绑定
#### 创建
  - `new Cookie(String name,String value)`  创建Cookie对象,绑定数据
#### 添加
  - `response.addCookie(Cookie cookie)` 向客户端添加Cookie(发送cookie对象)
#### 获取
  - `Cookie[]  request.getCookies()`    使用request获取Cookie,返回值为整个Cookie数组
#### Cookie案例
``` Java
@WebServlet("/servletDemo1")
public class ServletDemo1 extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        response.setContentType("text/html;charset=utf-8");
        //1.获取cookie数组
        Cookie[] cookies = request.getCookies();
        DateFormat df = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");

        boolean flag = false;
        if(cookies != null){
            //遍历数组，拿到每一个cookie
            for (Cookie cookie : cookies) {
                //获取cookie的名称
                String name = cookie.getName();
                //判断cookie是否为lastTime
                if("lastTime".equals(name)){      //说明不是第一次登录
                    flag = true;
                    //获取上一次的时间
                    String value = cookie.getValue();
                    //解码
                    value = URLDecoder.decode(value, "utf-8");

                    //创建当前时间
                    Date date = new  Date();
                    //把当前时间格式化为字符串
                    String lastTime = df.format(date);
                    //编码
                    lastTime = URLEncoder.encode(lastTime, "utf-8");
                    //设置新的值
                    cookie.setValue(lastTime);
                    //设置存活时间
                    cookie.setMaxAge(60*2);
                    //响应给页面
                    response.addCookie(cookie);
                    //给页面响应数据
                    response.getWriter().write("欢迎回来，您上次的访问时间为：" + value);
                    break;
                }
            }
        }

        if(cookies == null || cookies.length == 0 || !flag){  //说明是第一次登录
            //获取当前时间
            Date date = new Date();
            String lastTime = df.format(date);
            //编码
            lastTime = URLEncoder.encode(lastTime, "utf-8");

            Cookie cookie = new Cookie("lastTime",lastTime);
            //设置存活时间
            cookie.setMaxAge(60*2);
            response.addCookie(cookie);
            //给页面响应数据
            response.getWriter().write("您好，欢迎您第一次访问");

        }

    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doPost(request, response);
    }
}
```

### 实现原理
  - 基于响应头set-cookie和请求头cookie实现的

### Cookie的存储时间
  - 默认情况下，浏览器关闭后，cookie数据销毁
  - 持久化存储：
    - `Cookie对象.setMaxAge(int secounds)` 参数为以秒为单位的整数，该整数表示cookie的存储时长
      1. 正数：将cookie写入硬盘;并指定存储时间，时间到后，cookie文件会消失
      2. 负数：默认值
      3. 零：删除cookie信息

### cookie存储中文或特殊字符
  - cookie支持中文数据。不支持特殊字符，建议使用url编码存储，url解码解析
  - 例:
    ``` Java
    //  编码
    URLEncoder.encode(编码的数据, "utf-8");
    //  解码
    URLDecoder.decode(解码的数据, "utf-8");
    ```    
### cookie共享问题？
  - 默认情况下cookie不能共享
  - setPath(String path)：设置cookie的获取范围。默认情况，设置当前的虚拟目录
### 不同的tomcat服务器间cookie共享问题？
  - setDomain(String path)：如果一级域名相同，那么多个服务器之间可以共享
  - 例:
    https://tieba.baidu.com/
    http://news.baidu.com/
    baidu：一级域名
    tieba和news：二级域名
    案例：setDomain(".baidu.com"),那么tieba和news可以共享cookie。
### cookie的特点和作用
  - cookie存储在客户端(浏览器)
  - 浏览器对单个cookie的大小是有限制的(4kb)以及对同一个域名下的总cookie数量有限制(20个)

### 作用：
  1. cookie一般用于存储少量的不太敏感的数据
  2. 在不登陆的情况下，完成服务器对客户端身份的识别

## Session
服务器端会话技术	Session