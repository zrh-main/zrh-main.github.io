---
title: HTML解析
date: 2022-2-11 14:56:43
tags:
  - Java jar包
categories: Java
---

## Jsoup解析
  Java解析HTML文件一种解析器  

### 使用步骤
  1. 导入jar包
  2. 获取Document对象
  3. 获取对应标签
  4. 获取数据

### 类
  - Document: 文档对象,代表内存中dom树
  - Element:  元素对象,表示文档中的元素
  - Elements: 元素element对象的集合,可以当做ArrayList使用
  - Node:     节点对象;是Document和Element的父类
### 常用方法
  1. parse: 解析html或者xml文档,返回Document对象;
  2. 获取子元素对象
    * getElementById​(String id)：根据id属性值获取唯一的element对象
    * getElementsByTag​(String tagName)：根据标签名称获取元素对象集合
    * getElementsByAttribute​(String key)：根据属性名称获取元素对象集合
    * getElementsByAttributeValue​(String key, String value)：根据对应的属性名和属性值获取元素对象集合
  3. 获取属性值
    * String attr(String key)：根据属性名称获取属性值
  4. 获取文本内容
    * String text():获取文本内容
    * String html():获取标签体的所有内容(包括字标签的字符串内容)
  5. 选择器
    * select(String str):使用选择器获取元素

**案例:**
``` Java
import org.jsoup.Jsoup;
import org.jsoup.nodes.Document;
import org.jsoup.nodes.Element;
import org.jsoup.select.Elements;

import java.io.File;
import java.io.IOException;
public class JsoupDemo04 {

  public static void main(String[] args) throws IOException {
    //  获取文件路径
    String path = JsoupDemo01.class.getClassLoader().getResource("student.xml").getPath();
    //  获取document对象;解析xml文件
    Document document = Jsoup.parse(new File(path), "utf-8");
    //  获取属性
    Elements elements1 = document.getElementsByAttribute("id");
    //  根据具体的属性名=属性值;获取元素
    //  number="deyuan_0001"
    Elements value = document.getElementsByAttributeValue("number", "deyuan_0001");
    //  根据id属性值获取唯一的element对象
    Element deyuan = document.getElementById("deyuan");
    //  根据标签获取Elements 集合;获取元素对象集合
    Elements elements = document.getElementsByTag("name");
    //  获取元素对象的长度
    System.out.println(elements.size());
    //  获取第一个name的对象 
    Element ele_name = elements.get(0);
    //  获取文本内容
    String text = ele_name.text();
    String html = ele_name.html();

    //  标签选择器
    Elements elements = document.select("name");
    //  id选择器,查询  deyuan
    Elements select = document.select("#deyuan");
    //  属性选择器
    //  获取属性  number=deyuan_0001的元素
    Elements select1 = document.select("student[number=\"deyuan_0001\"]");
    //  子标签选择器
    Elements select2 = document.select("student[number=\"deyuan_0001\"] > age");
  }
}
```


### Xpath方式解析文件

​    **XPath**即为[XML](https://baike.baidu.com/item/XML)路径语言（XML Path Language），它是一种用来确定XML文档中某部分位置的语言。

   使用jsoup的xpath需要额外引入jar包

``` Java
package com.deyuan.demo04.jsoup;

import cn.wanghaomiao.xpath.exception.XpathSyntaxErrorException;
import cn.wanghaomiao.xpath.model.JXDocument;
import cn.wanghaomiao.xpath.model.JXNode;
import org.jsoup.Jsoup;
import org.jsoup.nodes.Document;

import java.io.File;
import java.io.IOException;
import java.util.List;

public class JsoupXpath {
    public static void main(String[] args) throws IOException, XpathSyntaxErrorException {
        //获取文件路径
        String path = JsoupDemo01.class.getClassLoader().getResource("student.xml").getPath();
        //解析xml文件
        Document document = Jsoup.parse(new File(path), "utf-8");
        //根据document对象,创建JXDocument对象
        JXDocument jxDocument = new JXDocument(document);

        List<JXNode> jxNodes = jxDocument.selN("//student");
        for (JXNode jxNode : jxNodes) {
            System.out.println(jxNode);
        }
        System.out.println("=============");
        //查询所有的student元素中的name标签
        List<JXNode> jxNodes1 = jxDocument.selN("//student/name");
        for (JXNode jxNode : jxNodes1) {
            System.out.println(jxNode);
        }
        System.out.println("===================");
        //查询student标签下所有带id属性的name标签
        List<JXNode> jxNodes2 = jxDocument.selN("//student/name[@id]");
        for (JXNode jxNode : jxNodes2) {
            System.out.println(jxNode);
        }
        System.out.println("=========jxNodes3jxNodes3jxNodes3==========");
        //查询student标签下所有带id属性的为德源的name标签
        List<JXNode> jxNodes3 = jxDocument.selN("//student/name[@id='deyuan']");
        for (JXNode jxNode : jxNodes3) {
            System.out.println(jxNode);
        }
    }
}

```