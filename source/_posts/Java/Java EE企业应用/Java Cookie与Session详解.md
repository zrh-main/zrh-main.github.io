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
  - cookie 是存储在客户端计算机上的文本文件，并保留了各种跟踪信息

### 使用
  Cookie 的使用需要对中文进行编码与解码
  ``` Java
  //  编码
  String str = java.net.URLEncoder.encode("中文"，"UTF-8");
  //  解码        
  String str = java.net.URLDecoder.decode("编码后的字符串","UTF-8");   
  ```
  Cookie 设置在 HTTP 头信息中;
#### 创建
  - `new Cookie(String name,String value)`  创建Cookie对象,绑定数据
#### 添加
  - `response.addCookie(Cookie cookie)` 向客户端添加Cookie(发送cookie对象)
#### 获取
  - `Cookie[]  request.getCookies()`    使用request获取Cookie,返回值为整个Cookie数组
#### 常用方法
  - `void setDomain(String pattern)`    设置 cookie 适用的域
  - `String getDomain()`                获取 cookie 适用的域
  - `void setMaxAge(int expiry)`        设置 cookie 过期的时间(以秒为单位);如果不设置,只在当前 session 会话中持续有效
  - `int getMaxAge()` 获取 cookie 的最大生存周期（以秒为单位）
  - `String getName()`  获取 cookie 的名称;名称创建后不可改变
  - `void setValue(String newValue)`  设置 cookie 关联的值
  - `String getValue()`     获取与 cookie 关联的值
  - `void setPath(String uri)`  设置 cookie 适用的路径;不设置时`当前页面相同目录`下的所有 URL 都会返回 cookie
  - `String getPath()`    获取 cookie 适用的路径
  - `void setSecure(boolean flag)`  设置 cookie 是否只在加密的（即 SSL）连接上发送
  - `void setComment(String purpose)` 设置cookie的注释
  - `String getComment()` 获取 cookie 的注释，没有注释则返回 null
#### Cookie案例
``` Java
@WebServlet("/servletDemo1")
public class ServletDemo1 extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        response.setContentType("text/html;charset=utf-8");
        //1.获取cookie数组
        Cookie[] cookies = request.getCookies();
        DateFormat df = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        //遍历Cookies数组，拿到每一个cookie
        for (Cookie cookie : cookies) {
            //获取cookie的名称
            String name = cookie.getName();
            //判断cookie是否为lastTime
            if("lastTime".equals(name)){
                //  获取当前时间格式化为字符串;并进行编码
                String lastTime = URLEncoder.encode(df.format(new  Date()), "utf-8");
                //  设置新的值
                cookie.setValue(lastTime);
                //  设置存活时间
                cookie.setMaxAge(60*2);
                //  响应给页面
                response.addCookie(cookie);
                //  获取上一次存入cookie的时间进行解码并给页面响应数据
                response.getWriter().write("欢迎回来，您上次的访问时间为：" + value = URLDecoder.decode(cookie.getValue(), "utf-8"));
                return
            }
        }

        //  第一次登录

        //  获取当前时间格式化为字符串;并进行编码
        String lastTime = URLEncoder.encode(df.format(new Date()), "utf-8");
        //  创建cookie对象
        Cookie cookie = new Cookie("lastTime",lastTime);
        //  设置存活时间
        cookie.setMaxAge(60*2);
        //  添加到响应中
        response.addCookie(cookie);
        //  给页面响应数据
        response.getWriter().write("您好，欢迎您第一次访问");
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
    案例：setDomain(".baidu.com"),那么tieba和news可以共享cookie

### cookie的特点和作用
  - cookie存储在客户端(浏览器)
  - 浏览器对单个cookie的大小是有限制的(4kb)以及对同一个域名下的总cookie数量有限制(20个)

### 作用：
  1. cookie一般用于存储少量的不太敏感的数据
  2. 在不登陆的情况下，完成服务器对客户端身份的识别

## Session
服务器端会话技术	Session
session以属性,值的方式存储
### HttpSession 对象
  - Servlet 提供了 HttpSession 接口，该接口提供了跨多个页面请求或访问网站时识别用户以及存储有关用户信息的方式

#### 获取HttpSession 对象
调用 HttpServletRequest 的公共方法 getSession() 来获取 HttpSession 对象
例:`HttpSession session = request.getSession();`

#### HttpSession 对象的重要方法
  - `Object getAttribute(String name)`  获取session中指定属性的值,没有返回null
  - `Enumeration getAttributeNames()` 获取session中所有属性的名称,返回类型为String 对象的枚举
  - `long getCreationTime()`  获取session 会话的创建时间;自格林尼治标准时间 1970 年 1 月 1 日午夜算起，以`毫秒`为单位
  - `String getId()`  获取该 session 会话的唯一标识符的字符串
  - `long getLastAccessedTime()`  获取客户端最后一次与该 session 会话相关的请求的时间;自格林尼治标准时间 1970 年 1 月 1 日午夜算起，以`毫秒`为单位
  - `int getMaxInactiveInterval()`  获取 session 会话打开的最大时间间隔，以`秒`为单位
  - `void invalidate()` 删除整个 session 会话
  - `boolean isNew()`   客户选择不参入该 session 会话，则该方法返回 true
  - `void removeAttribute(String name)` 移除一个特定的属性
  - `void setAttribute(String name, Object value)`  设置名称和值到该 session 会话
  - `void setMaxInactiveInterval(int interval)` 设置 session 会话过期时间,以`秒`为单位

5、session的特点？
答：
当客户端关闭时，在服务器关闭、前后，获取session是否为同一个
  - 默认情况下，不是同一个session。如果需要相同，则可以创建Cookie，键为JSESSIONID，设置最大存活时间，让cookie持久化保存。

客户端不关闭，在服务器关闭后，两次获取的session是同一个吗
  - 不是同一个
（1）Session的钝化：在服务器正常关闭之前，将session对象序列化到硬盘上。
（2）Session的活化：在服务器启动后，将session文件转化为内存中的session对象即可。
  tomcat自动的把session的钝化和活化做了