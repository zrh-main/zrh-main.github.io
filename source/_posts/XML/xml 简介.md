---
title: XML
date: 2022-2-11 14:56:43
tags:
  - XML
categories: XML
---

## 简介

### 概述
XML 指可扩展标记语言（eXtensible Markup Language）
> 可扩展 : 标签可以自定义`<user> <person>`

XML 被设计用来传输和存储数据
XML 被设计用来结构化、存储以及传输信息

### XML与HTML的区别
XML   被设计用来传输和存储数据，其焦点是数据的内容
HTML  被设计用来显示数据，其焦点是数据的外观
#### 具体区别
  1. xml标签是自定义的,HTML标签是预定义
  2. xml语法比较严格, html语法比较松散
  3. xml是存储数据的,html是展示数据的.

### 作用
  把数据从 HTML 分离
  简化数据共享
  简化数据传输
  简化平台变更

#### 具体作用
存储数据:
  1. 配置文件
  2. 在网络中传输.

### 优势
XML 的优势之一，就是可以在不中断应用程序的情况下进行扩展
## 结构
XML 文档形成了一种树结构，它从"根部"开始，然后扩展到"枝叶"

``` xml
<!-- XML 声明。定义XML的版本(1.0)和所使用的编码(UTF-8 : 万国码, 可显示各种语言) -->
<?xml version="1.0" encoding="UTF-8"?>

<!-- 根元素 -->
<note>
<!-- 子元素 -->
  <to>Tove</to>
  <from>Jani</from>
  <heading>Reminder</heading>
  <body>Don't forget me this weekend!</body>
<!-- 根元素结尾 -->
</note>
```
> XML将数据组织成为一棵树,DOM 通过解析 XML 文档,为 XML 文档在逻辑上建立一个树模型,树的节点是一个个的对象。这样通过操作这棵树和这些对象就可以完成对 XML 文档的操作,为处理文档的所有方面提供了一个完美的概念性框架

## 语法
1. xml声明(版本号,字符编码等...),必须放在第一行
2. 必须有根元素
3. 所有元素必须有`闭合标签`;声明不属于元素
4. 大小写敏感(必须使用相同的大小写来编写打开标签和关闭标签)​
5. 必须正确嵌套
6. 属性值必须加引号
7. 实体引用
    - 一些字符拥有特殊的意义;如`<`字符,`<`放在XML元素中会发生错误,因为解析器会把它当作新元素的开始,为了避免这个错误，必须使用`实体引用`来代替 `<`字符
    - 预定义的实体引用  
      | &lt;	  | < | less than |
      | &gt;	  | > | greater than |
      | &amp;	  | & | ampersand |
      | &apos;  | ' | apostrophe |
      | &quot;  | " | quotation mark |
    - 使用案例：`<message>if salary &lt; 1000 then</message>`
8. 注释
  `<!-- 这里的内容不会展示 -->`
9. 空格会被保留
    - 在XML中，文档中的空格不会被删减
10. xml文档的后缀名,必须是.xml
11. 标签中的属性值必须使用引号(单双引号)引起来

### 文档声明
1. 格式:  必须放到第一行 `<?xml 属性列表?>`
2. 属性列表:
    - version  版本号   **`必须属性`**
    - encoding: 编码格式,告知解析引擎当前文档使用的字符集,默认值: ISO-8859-1
    - standalone：是否独立
      - 取值：
        - yes：不依赖其他文件
        - no：依赖其他文件

## 命名规则
- 名称可以包含字母、数字以及其他的字符
- 名称不能以数字或者标点符号开始
- 名称不能以字母 xml（或者 XML、Xml 等等）开始
- 名称不能包含空格
### 命名推荐习惯
使用下划线的名称
避免 "-" 字符。一些软件会认为您想要从 first 里边减去 name
避免 "." 字符。一些软件会认为 "name" 是对象 "first" 的属性
避免 ":" 字符。冒号会被转换为命名空间来使用

## 属性
属性（Attribute）提供有关元素的额外信息
属性必须加引号
在 XML 中，应该尽量避免使用属性。如果信息类似于数据，那么请使用元素
### 避免 XML 属性？
使用属性而引起的一些问题：
- 属性不能包含多个值（元素可以）
- 属性不能包含树结构（元素可以）
- 属性不容易扩展（为未来的变化）
- 属性难以阅读和维护。`请尽量使用元素来描述数据。而仅仅使用属性来提供与数据无关的信息`
元数据（有关数据的数据）应当存储为属性，而数据本身应当存储为元素
### id属性
向元素分配 ID 引用。这些 ID 索引可用于标识 XML 元素，它起作用的方式与 HTML 中 id 属性是一样的
``` xml
<messages>
<note id="501">
<to>Tove</to>
<from>Jani</from>
<heading>Reminder</heading>
<body>Don't forget me this weekend!</body>
</note>
<note id="502">
<to>Jani</to>
<from>Tove</from>
<heading>Re: Reminder</heading>
<body>I will not</body>
</note>
</messages>
```


## XML约束
约束 : 规定xml文档书写规则
作为框架的使用者(程序员):
  1. 能够在xml中引入约束文件
  2. 能够简单的读懂约束文档,(不要求会写)
作为框架的开发者(程序员):
  - 能够定义约束文档
- DTD 基础约束文件 
  - 通过 DTD 验证的XML是"合法"的 XML
  - 例:   student.data
  - 引入方式 
      本地:<!DOCTYPE students SYSTEM "student.dtd">
      远程:
  ```dtd
  <!ELEMENT students (student*) >
  <!ELEMENT student (name,age,sex)>
  <!ELEMENT name (#PCDATA)>
  <!ELEMENT age (#PCDATA)>
  <!ELEMENT sex (#PCDATA)>
  <!ATTLIST student number ID #REQUIRED>
  ```
  ``` XML
  <?xml version="1.0" encoding="UTF-8" ?>
  <!DOCTYPE students SYSTEM "student.dtd">
  <students>
    <student number="deyuan_0001">
      <name>tom</name>
      <age>18</age>
      <sex>male</sex>
    </student>
  </students>
  ```
- Schema 复杂的约束技术
例:
```xsd
<?xml version="1.0"?>
<xsd:schema xmlns="http://www.deyuan.com/xml"
        xmlns:xsd="http://www.w3.org/2001/XMLSchema"
        targetNamespace="http://www.deyuan.com/xml" elementFormDefault="qualified">
    <xsd:element name="students" type="studentsType"/>
    <xsd:complexType name="studentsType">
        <xsd:sequence>
            <xsd:element name="student" type="studentType" minOccurs="0" maxOccurs="unbounded"/>
        </xsd:sequence>
    </xsd:complexType>
    <xsd:complexType name="studentType">
        <xsd:sequence>
            <xsd:element name="name" type="xsd:string"/>
            <xsd:element name="age" type="ageType" />
            <xsd:element name="sex" type="sexType" />
        </xsd:sequence>
        <xsd:attribute name="number" type="numberType" use="required"/>
    </xsd:complexType>
    <xsd:simpleType name="sexType">
        <xsd:restriction base="xsd:string">
            <xsd:enumeration value="male"/>
            <xsd:enumeration value="female"/>
        </xsd:restriction>
    </xsd:simpleType>
    <xsd:simpleType name="ageType">
        <xsd:restriction base="xsd:integer">
            <xsd:minInclusive value="0"/>
            <xsd:maxInclusive value="256"/>
        </xsd:restriction>
    </xsd:simpleType>
    <xsd:simpleType name="numberType">
        <xsd:restriction base="xsd:string">
            <xsd:pattern value="deyuan_\d{4}"/>
        </xsd:restriction>
    </xsd:simpleType>
</xsd:schema> 
```
``` XML
<?xml version="1.0" encoding="UTF-8" ?>
<!-- 
	1.填写xml文档的根元素
	2.引入xsi前缀.  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	3.引入xsd文件命名空间.  xsi:schemaLocation="http://www.deyuan.com/xml  student.xsd"
	4.为每一个xsd约束声明一个前缀,作为标识  xmlns="http://www.deyuan.com/xml" 
	
	
 -->
 <students   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
 			 xmlns="http://www.deyuan.com/xml" 
 		   xsi:schemaLocation="http://www.deyuan.com/xml  student.xsd"
 		    >
 	<student number="deyuan_0001">
 		<name>tom</name>
 		<age>18</age>
 		<sex>male</sex>
 	</student>
		 
 </students>
```


## XML 查看
在主流的浏览器中，均能够查看原始的 XML 文件;XML 文件不会直接显示为 HTML 页面
查看无效的(有错的) XML 文件，浏览器会报告错误


## 解析XML
操作xml文件
  1. 解析(读取): 将文档中的数据读取到内存中
  2. 写入:将内存中的数据,写入到xml文档中,持久化存储

### 解析xml方式
  Java解析xml常用方式DOM4j
#### DOM4j解析
``` Java
package com.cxx.xml;
import org.dom4j.Attribute;
import org.dom4j.Document;
import org.dom4j.DocumentException;
import org.dom4j.Element;
import org.dom4j.io.SAXReader;

import java.io.File;
import java.util.Iterator;
import java.util.List;

/**
 * @Author: cxx
 * Dom4j解析xml
 * @Date: 2018/5/30 12:21
 */
public class Dom4JDemo {
    public static void main(String[] args) throws Exception {
        //1.创建Reader对象
        SAXReader reader = new SAXReader();
        //2.加载xml
        Document document = reader.read(new File("src/main/resources/demo.xml"));
        //3.获取根节点
        Element rootElement = document.getRootElement();
        Iterator iterator = rootElement.elementIterator();
        while (iterator.hasNext()){
            Element stu = (Element) iterator.next();
            List<Attribute> attributes = stu.attributes();
            System.out.println("======获取属性值======");
            for (Attribute attribute : attributes) {
                System.out.println(attribute.getValue());
            }
            System.out.println("======遍历子节点======");
            Iterator iterator1 = stu.elementIterator();
            while (iterator1.hasNext()){
                Element stuChild = (Element) iterator1.next();
                System.out.println("节点名："+stuChild.getName()+"---节点值："+stuChild.getStringValue());
            }
        }
    }
}
```

## XPath
### 概述
  XPath 是一门在 XML 文档中查找信息的语言， 可用来在 XML 文档中对元素和属性进行遍历。XPath 是 W3C XSLT 标准的主要元素，并且 XQuery 和 XPointer 同时被构建于 XPath 表达之上。因此，对 XPath 的理解是很多高级 XML 应用的基础
  XPath非常类似对数据库操作的SQL语言，或者说JQuery，它可以方便开发者抓起文档中需要的东西`(dom4j也支持xpath)`
### 案例:
``` Java
import java.io.IOException;
import java.io.InputStream;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.parsers.ParserConfigurationException;
import javax.xml.xpath.XPath;
import javax.xml.xpath.XPathConstants;
import javax.xml.xpath.XPathExpression;
import javax.xml.xpath.XPathExpressionException;
import javax.xml.xpath.XPathFactory;
import org.w3c.dom.Document;
import org.w3c.dom.NodeList;
import org.xml.sax.SAXException;
public class TestXPath {
  public static void main(String[] args) {
    read();
  }
  public static void read() {
    try {
      DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();
      DocumentBuilder builder = dbf.newDocumentBuilder();
      InputStream in = TestXPath.class.getClassLoader().getResourceAsStream("test.xml");
      Document doc = builder.parse(in);
      XPathFactory factory = XPathFactory.newInstance();
      XPath xpath = factory.newXPath();
      // 选取所有class元素的name属性
      // XPath语法介绍： http://w3school.com.cn/xpath/
      XPathExpression expr = xpath.compile("//class/@name");
      NodeList nodes = (NodeList) expr.evaluate(doc, XPathConstants.NODESET);
      for (int i = 0; i < nodes.length();i++){
        System.out.println("name = " + nodes.item(i).getNodeValue());
      }
    } catch (XPathExpressionException e) {
      e.printStackTrace();
    } catch (ParserConfigurationException e) {
      e.printStackTrace();
    } catch (SAXException e) {
      e.printStackTrace();
    } catch (IOException e) {
      e.printStackTrace();
    }
  }
}
```