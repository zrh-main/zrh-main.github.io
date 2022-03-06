---
title: IO-字节流
date: 2022-2-9 16:56:43
tags:
  - Java SE基础语法
  - Java IO流
categories: Java
---


## 概述
一切文件数据(文本、图片、视频等)在存储时，都是以二进制数字的形式保存，都是一个一个的字节，那么传输时一样如此。所以，字节流可以传输任意文件数据。在操作流的时候，我们要时刻明确，无论使用什么样的流对象，底层传输的始终为二进制数据

## 字节输出流【OutputStream】

`java.io.OutputStream`抽象类是表示字节输出流的所有类的超类，将指定的字节信息写出到目的地。它定义了字节输出流的基本共性功能方法。

* `public void close()` ：关闭此输出流并释放与此流相关联的任何系统资源。  
* `public void flush() ` ：刷新此输出流并强制任何缓冲的输出字节被写出。  
* `public void write(byte[] b)`：将 b.length字节从指定的字节数组写入此输出流。  
* `public void write(byte[] b, int off, int len)` ：从指定的字节数组写入 len字节，从偏移量 off开始输出到此输出流
* `public abstract void write(int b)` ：将指定的字节输出流。

> <font color="red">注意：</font>
> close方法，当完成流的操作时，必须调用此方法，释放系统资源。

### FileOutputStream类

`java.io.FileOutputStream`是`OutputStream`的常用子类;是一个文件输出流，用于将数据写出到文件
#### 构造方法
- 创建流对象时，必须传入文件路径;该路径下，如果没有这个文件，会创建该文件;
1. 重写;清空数据
  * `public FileOutputStream(File file)`：创建文件输出流以写入由指定的 File对象表示的文件 
  * `public FileOutputStream(String name)`： 创建文件输出流以指定的名称写入文件  
2. 续写;追加续写数据
- `public FileOutputStream(File file, boolean append)`： 创建文件输出流以写入由指定的 File对象表示的文件。  
- `public FileOutputStream(String name, boolean append)`： 创建文件输出流以指定的名称写入文件
`boolean append`的值，`true` 表示追加数据，`false` 表示清空数据;这样创建的输出流对象，就可以指定是否追加续写

案例：

``` Java
public class FileOutputStreamConstructor throws IOException {
    public static void main(String[] args) {
   	 	  
        File file = new File("a.txt");
        //  使用File对象创建流对象
        FileOutputStream fos1 = new FileOutputStream(file);
        //  使用File对象创建流对象;启用  数据追加
        FileOutputStream fos11 = new FileOutputStream(file,true);

        //  使用文件名称创建流对象
        FileOutputStream fos2 = new FileOutputStream("b.txt");
        // 使用文件名称创建流对象;启用  数据追加
        FileOutputStream fos22 = new FileOutputStream("fos.txt"，true);
    }
}
```

#### 写出数据
 
`void write(int b)` 将指定的字节写入此文件输出流
`void write(byte[] b)` 将 b.length个字节从指定的字节数组写入此文件输出流  
`void write(byte[] b, int off, int len)` 将 len字节从位于偏移量 off的指定字节数组写入此文件输出流 
案例:
``` Java
public class FOSWrite {
    public static void main(String[] args) throws IOException {
        // 使用文件名称创建流对象
        FileOutputStream fos = new FileOutputStream("fos.txt");     

        //  写出1个字节的数据
      	fos.write(97); 
        // 注意：虽然参数为int类型四个字节，但是只会保留一个字节的信息写出。

        //  写出整个字节数组的数据
      	byte[] b = "德源程序员".getBytes();//  字符串转换为字节数组
      	//  写出字节数组数据
      	fos.write(b);

        //  写出指定长度字节数组的数据
      	byte[] b = "abcde".getBytes();//  字符串转换为字节数组
		    // 写出从索引2开始，2个字节。索引2是c，两个字节，也就是cd。
        fos.write(b,2,2);

      	// 关闭资源
        fos.close();
    }
}
输出结果：
abc
输出结果：
德源程序员
输出结果：
cd
```

#### 写出换行

Windows系统里，换行符号是`\r\n` 
案例：

``` Java
public class FOSWrite {
    public static void main(String[] args) throws IOException {
        // 使用文件名称创建流对象
        FileOutputStream fos = new FileOutputStream("fos.txt");  
      	// 定义字节数组
      	byte[] words = {97,98,99,100,101};
      	// 遍历数组
        for (int i = 0; i < words.length; i++) {
          	// 写出一个字节
            fos.write(words[i]);
          	// 写出一个换行, 换行符号转成数组写出
            fos.write("\r\n".getBytes());
        }
      	// 关闭资源
        fos.close();
    }
}

输出结果：
a
b
c
d
e
```

> * 回车符`\r`和换行符`\n` ：
>   * 回车符：回到一行的开头（return）。
>   * 换行符：下一行（newline）。
> * 系统中的换行：
>   * Windows系统里，每行结尾是 `回车+换行` ，即`\r\n`；
>   * Unix系统里，每行结尾只有 `换行` ，即`\n`；
>   * Mac系统里，每行结尾是 `回车` ，即`\r`。从 Mac OS X开始与Linux统一。

## 字节输入流【InputStream】

`java.io.InputStream`抽象类是表示字节输入流的所有类的超类，可以读取字节信息到内存中。它定义了字节输入流的基本共性功能方法。

- `public void close()` ：关闭此输入流并释放与此流相关联的任何系统资源。    
- `public abstract int read()`： 从输入流读取数据的下一个字节。 
- `public int read(byte[] b)`： 从输入流中读取一些字节数，并将它们存储到字节数组 b中 。

> 注意：
>
> close方法，当完成流的操作时，必须调用此方法，释放系统资源。

###  FileInputStream类

`java.io.FileInputStream `类是文件输入流，从文件中读取字节。

#### 构造方法

* `FileInputStream(File file)`： 通过打开与实际文件的连接来创建一个 FileInputStream ，该文件由文件系统中的 File对象 file命名。 
* `FileInputStream(String name)`： 通过打开与实际文件的连接来创建一个 FileInputStream ，该文件由文件系统中的路径名 name命名。  

当你创建一个流对象时，必须传入一个文件路径。该路径下，如果没有该文件,会抛出`FileNotFoundException` 。

- 例：

``` Java
public class FileInputStreamConstructor throws IOException{
    public static void main(String[] args) {
   	 	// 使用File对象创建流对象
        File file = new File("a.txt");
        FileInputStream fos = new FileInputStream(file);
      
        // 使用文件名称创建流对象
        FileInputStream fos = new FileInputStream("b.txt");
    }
}
```

#### 读取字节数据

**读取字节**：`read`方法，每次读取一个字节的数据，提升为int类型，读取到文件末尾，返回`-1`
案例：
``` Java
public class FISRead {
    public static void main(String[] args) throws IOException{
      	// 使用文件名称创建流对象
       	FileInputStream fis = new FileInputStream("read.txt");
      	// 读取一个字节数据，返回一个字节
        int read = fis.read();
        System.out.println((char) read);
        read = fis.read();
        System.out.println((char) read);
        read = fis.read();
        System.out.println((char) read);
        read = fis.read();
        System.out.println((char) read);
        read = fis.read();
        System.out.println((char) read);
      	// 读取到末尾,返回-1
       	read = fis.read();
        System.out.println( read);


        //  循环改进读取方式
        // 定义变量，保存数据
        int b ；
        // 循环读取
        while ((b = fis.read())!=-1) {
            System.out.println((char)b);
        }

        //  使用字节数组读取
        //  定义变量，作为有效个数
        int len ；
        // 定义字节数组，作为装字节数据的容器   
        byte[] b = new byte[2];
        // 循环读取
        while (( len= fis.read(b))!=-1) {
           	// 每次读取后,把数组的有效字节部分，变成字符串打印
            System.out.println(new String(b，0，len));//  len 每次读取的有效字节个数
        }
		    // 关闭资源
        fis.close();
    }
}

```
> 小贴士：
> 使用数组读取，每次读取多个字节，减少了系统间的IO操作次数，从而提高了读写的效率，开发中必须使用此方式

## 字节流案例(图片复制)

``` Java
public class Copy {
    public static void main(String[] args) throws IOException {
        // 1.创建流对象
        // 1.1 指定数据源
        FileInputStream fis = new FileInputStream("D:\\test.jpg");
        // 1.2 指定目的地
        FileOutputStream fos = new FileOutputStream("test_copy.jpg");

        // 2.读写数据
        // 2.1 定义数组
        byte[] b = new byte[1024];
        // 2.2 定义长度
        int len;
        // 2.3 循环读取
        while ((len = fis.read(b))!=-1) {
            // 2.4 写出数据
            fos.write(b, 0 , len);
        }

        // 3.关闭资源
        fos.close();
        fis.close();
    }
}
```
> 小贴士：
> 流的关闭原则：先开后关，后开先关