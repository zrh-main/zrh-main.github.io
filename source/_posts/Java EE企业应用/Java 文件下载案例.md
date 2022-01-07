---
title: Servlet文件下载案例
date: 2022-01-03 19:49:57
tags:
  - Java EE网络应用
categories:
  Java
---

## 代码实现
AdaptiveBrowserUtil文件 适配不同浏览器
``` Java
package cn.dy.util;
import sun.misc.BASE64Encoder;
import java.io.UnsupportedEncodingException;
import java.net.URLEncoder;
/**
 * @author zrh
 * @date 2022/1/6
 * @apiNote
 */
public class AdaptiveBrowserUtil {
    /**
     * 根据浏览器不同,配置  下载文件不同的名称 编码格式
     * @param agent 浏览器信息->可以在request的header中的user-agent查询
     * @param filename  文件名称
     * @return  文件名称
     * @throws UnsupportedEncodingException
     */
    public static String getFileName(String agent, String filename) throws UnsupportedEncodingException {
        if (agent.contains("MSIE")) {
            // IE浏览器
          filename = URLEncoder.encode(filename, "utf-8");
          filename = filename.replace("+", " ");
        } else if (agent.contains("Firefox")) {
            // 火狐浏览器
          BASE64Encoder base64Encoder = new BASE64Encoder();
          filename = "=?utf-8?B?" + base64Encoder.encode(filename.getBytes("utf-8")) + "?=";
        } else {
            // 其它浏览器
          filename = URLEncoder.encode(filename, "utf-8");
        }
        return filename;
    }
}

```

DownloadServlet文件 实现下载Servlet
``` Java
import cn.dy.util.AdaptiveBrowserUtil;

import javax.servlet.ServletContext;
import javax.servlet.ServletOutputStream;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.FileInputStream;
import java.io.IOException;

@WebServlet("/downloadServlet")
public class DownloadServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws javax.servlet.ServletException, IOException {

        //  获取文件名称
        String fileName = request.getParameter("fileName");

        //  获取项目的上下文
        ServletContext servletContext = request.getServletContext();

        //  找到文件的真实路径
        String realPath = servletContext.getRealPath(fileName);

        //  获取文件的网络通信格式
        String mimeType = servletContext.getMimeType(fileName);

        //  根据不同浏览器适配不同格式文件名称
        fileName = AdaptiveBrowserUtil.getFileName(request.getHeader("user-agent"), fileName);

        //  设置  响应的数据格式为附件形式    附件形式==文件下载
        response.setHeader("Content-disposition","attachment;filename="+fileName);
        //  设置内容类型为XXX文件类型的网络通信格式
        response.setHeader("content-type",mimeType);
        //  使用文件输入流读取文件到bytes
        FileInputStream fileInputStream = new FileInputStream(realPath);
        byte[] bytes = new byte[3072];
        int len = 0;
        //  网络输出流从bytes写入页面
        ServletOutputStream responseOutputStream = response.getOutputStream();
        while ((len = fileInputStream.read(bytes)) != -1) {
            responseOutputStream.write(bytes);
        }
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws javax.servlet.ServletException, IOException {
        this.doPost(request, response);
    }
}

```

