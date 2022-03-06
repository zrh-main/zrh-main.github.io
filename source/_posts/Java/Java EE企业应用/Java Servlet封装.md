---
title: Java Servlet封装
tags:
  - Java EE企业应用
categories:   Java
date: 2021-12-22 14:53:58
---
**封装DispatcherServlet.java**
``` Java
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Method;

/**
 * servlet封装
 * 通过反射的原理,执行方法分发
 * 在一个servlet中完成同一资源的操作
 */

public class DispatcherServlet extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //  设置字符编码
        req.setCharacterEncoding("utf-8");
        //  获取uri
        String requestURI = req.getRequestURI();
        //  截取请求资源
        String methodName = requestURI.substring(requestURI.lastIndexOf("/") + 1);

        Method method = null;
        try {
            //  使用反射,通过请求资源获取到与该资源同名的方法对象
            method = this.getClass().getMethod(methodName, HttpServletRequest.class, HttpServletResponse.class);
        } catch (NoSuchMethodException e) {
            e.printStackTrace();
            //  未找到该方法,返回404
            resp.sendRedirect(req.getContextPath() + "/404.jsp");
            return;
        }
        try {
            //  执行该方法对象
            method.invoke(this, req, resp);
        } catch (IllegalAccessException e) {
            e.printStackTrace();
        } catch (InvocationTargetException e) {
            e.printStackTrace();
        }
    }
}
```

**使用UserServlet.java**
``` Java
import cn.dy.entity.User;
import cn.dy.service.UserService;
import cn.dy.service.impl.UserServiceImpl;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

/**
 * @author zrh
 * @date 2022/2/18
 * @apiNote
 */
//  配置资源路径,所有user下的资源都访问这里
@WebServlet("/user/*")
public class UserServlet extends DispatcherServlet {
    private UserService userService = new UserServiceImpl();

    /**
     * 用户登录
     *
     * @param request
     * @param response
     */
    public void login(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        String username = request.getParameter("username");
        String password = request.getParameter("password");
        User user = userService.login(username, password);
        if (user == null) {
            request.setAttribute("msg","用户名或密码错误!");
            request.getRequestDispatcher("/fail.jsp").forward(request, response);
            return;
        }
        request.getSession().setAttribute("user", user);
        response.sendRedirect(request.getContextPath() + "/index.jsp");

    }
}
```
**测试**
``` Java
该服务启动后,浏览器访问该登录资源即可
```
