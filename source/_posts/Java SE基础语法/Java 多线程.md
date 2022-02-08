---
title: 多线程
tags:
  - Java SE基础语法
categories: Java
---

## 进程与线程
  - 进程:在内存中运行的程序称为进程;一个进程可以包含多个线程。进程一直运行，直到所有的非守护线程都结束运行后才能结束
  - 线程:进程的一个执行单元,没有自己的独立的内存空间;它必须是进程的一部分
  - 多线程：多个线程同时执行，我们称为多线程程序。
## 多线程的优势
  - 多线程能高效率的运行程序,达到充分利用 CPU 的目的

## 并发和并行
  - 并发：在同一个时间段内同时运行。
  - 并行：在同一个时间点同时运行。
（并发是并行的假象）

## 线程调度
  - 分时调度：
    多个线程轮流使用cpu的执行权。
  - 抢占式调度：
    多个线程之间会相互抢夺cpu的执行权。
  - 优先级：
    线程的优先级越高，说明抢夺cpu的概率越高。
    java是抢占式调度

## 线程的生命周期
![](java-thread.jpg)
- 新建状态:
使用 new 关键字和 Thread 类或其子类建立一个线程对象后，该线程对象处于新建状态

- 就绪状态:
当线程对象调用了start()方法之后，该线程就进入就绪状态。就绪状态的线程处于就绪队列中，要等待JVM里线程调度器的调度。

- 运行状态:
如果就绪状态的线程获取 CPU 资源，就可以执行 run()，此时线程便处于运行状态。运行状态的线程，可以变为阻塞状态、就绪状态和死亡状态。

- 阻塞状态:
如果一个线程执行了sleep（睡眠）、suspend（挂起）等方法，失去所占用资源之后，该线程就从运行状态进入阻塞状态。在睡眠时间已到或获得设备资源后可以重新进入就绪状态。可以分为三种：

等待阻塞：运行状态中的线程执行 wait() 方法，使线程进入到等待阻塞状态。

同步阻塞：线程在获取 synchronized 同步锁失败(因为同步锁被其他线程占用)。

其他阻塞：通过调用线程的 sleep() 或 join() 发出了 I/O 请求时，线程就会进入到阻塞状态。当sleep() 状态超时，join() 等待线程终止或超时，或者 I/O 处理完毕，线程重新转入就绪状态

- 死亡状态:
一个运行状态的线程完成任务或者其他终止条件发生时，该线程就切换到终止状态。

**<font color='red'>main方法是Java的主线程</font>**

## 线程的创建方式
- 方式一：
  1. 定义一个类继承Thread类，重写run方法，run方法中就是线程要执行的代码。
  2. 创建该类的对象，调用start方法开启线程
- 方式二：
  1. 定义一个类实现Runnable接口，重写run方法，run方法中就是线程要执行的代码。
  2. 创建Thread类的对象，把Runnable接口的实现类对象传递过来作为线程任务，调用start方法开启线程
- 方式二使用匿名内部类:
  Thread类的构造方法中需要一个线程任务（Runnable接口），接口作为参数，可以使用匿名内部类去代表接口的实现类的对象。
- 方式二的优势：
  1. 可以避免java单继承的局限性。
  2. 让线程的创建和线程任务相分离。解耦。
  3. 线程池只能接收第二种方式创建的线程。
- 例:
  1. 方式一
  ``` Java
  public class MyThread extends Thread {
    //设置线程名称
    public MyThread(String name){
        super(name);
    }
    @Override
    public void run() {
        for (int i = 0; i < 200; i++) {
          //  获取线程名称
          System.out.println("线程名称为："+getName() + "-"+ i);
          try {
              //  休眠1秒
              Thread.sleep(1000);
          } catch (InterruptedException e) {
              e.printStackTrace();
          }
        }
    }
  }
  ```
  2. 方式二
  ``` Java
  //  线程任务类
  public class MyRunnable implements Runnable {
      @Override
      public void run() {
          for (int i = 0; i < 200; i++) {
              System.out.println("刘亦菲..." + i);
          }
      }
  }
  //  运行类
  public class Test1 {
    public static void main(String[] args) {
        //创建一个线程任务
        MyRunnable mr  = new MyRunnable();
        //创建一个线程
        Thread tr = new Thread(mr);
        //开启线程
        tr.start();
    }
  }
  ```
  3. 方式二匿名内部类
  ``` Java
  public class Thread2 {
      public static void main(String[] args) {
          //创建一个线程对象，参数中需要一个Runnable接口
          Thread tr = new Thread(new Runnable() {
              @Override
              public void run() {
                  System.out.println("这是一个新的线程...");
              }
          });
          tr.start();
      }
  }
  ```
## Thread类
### 概述
  java提供的一个线程的根类

### 构造方法
Thread()		//创建一个线程对象
Thread(String name)		//创建一个线程对象，可以指定线程名称
Thread(Runnable )			
Thread(Runnable,String name )
### 常用方法
`String getName()`获取线程的名称
`void start()`开启线程，在新线程内部会调用run方法
`void run()`线程要执行的代码
`static void sleep(long millis)`线程休眠..毫秒
`static Thread currentThread()`获取当前线程的引用。可以获取主线程的名称。



## 线程安全
### 概述
多个线程在同时访问同一块资源时，会引发线程安全问题。
### 同步代码块
synchronized(锁对象){
}
锁对象可以是任意对象。
### 同步方法
修饰符 synchronized  返回值类型 方法名(){}
同步方法的锁对象是this对象
静态同步方法的锁对象是class对象（.class字节码文件）
### Lock锁
是jdk1.5出的新特性。它可以解决线程安全问题。
void lock()  		加锁
void unlock() 		释放锁

### 案例
问题
``` Java
public class MyPiao implements Runnable {
    private int piao = 100;     //代表总票数
    @Override
    public void run() {
        for(;;){
            //判断，只要总票数>0，可以卖票
            if(piao > 0){
                try {
                    Thread.sleep(100);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }       //   1   2   3
                System.out.println(Thread.currentThread().getName() + "正在卖第"+ piao +"张票");
                piao--;
            }
        }
    }
}
public class Test1 {
    public static void main(String[] args) {
        //创建线程任务
        MyPiao mp = new MyPiao();
        Thread tr1 = new Thread(mp,"窗口一");
        Thread tr2 = new Thread(mp,"窗口二");
        Thread tr3 = new Thread(mp,"窗口三");

        //开启线程
        tr1.start();
        tr2.start();
        tr3.start();
    }
}
// 同时访问同一块资源时，会引发的安全问题
  //1. 卖重复票
  //2. 卖不存在的票
```
解决方案1 同步代码块
``` Java
public class MyPiao implements Runnable {

    private int piao = 100;     //代表总票数
    private Object object = new Object();

    @Override
    public void run() {
        for(;;){
            //同步代码块  接收任意对象作为锁
            synchronized (object){
                //判断，只要总票数>0，可以卖票
                if(piao > 0){
                    try {
                        Thread.sleep(100);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }       //   1   2   3
                    System.out.println(Thread.currentThread().getName() + "正在卖第"+ piao +"张票");
                    piao--;
                }
            }
        }
    }
}
public class Test1 {
    public static void main(String[] args) {
        //创建线程任务
        MyPiao mp = new MyPiao();
        Thread tr1 = new Thread(mp,"窗口一");
        Thread tr2 = new Thread(mp,"窗口二");
        Thread tr3 = new Thread(mp,"窗口三");

        //开启线程
        tr1.start();
        tr2.start();
        tr3.start();
    }
}
```
解决方案2 同步方法
``` Java
public class MyPiao implements Runnable {

    private  int piao = 100;     //代表总票数

    @Override
    public void run() {
        for(;;){
            //判断，只要总票数>0，可以卖票
            fun();
        }

    }

    //同步方法
    public  synchronized void fun(){
        if(piao > 0){
            try {
                Thread.sleep(100);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }       //   1   2   3
            System.out.println(Thread.currentThread().getName() + "正在卖第"+ piao-- +"张票");

        }
    }
}
public class Test1 {
    public static void main(String[] args) {
        //创建线程任务
        MyPiao mp = new MyPiao();
        Thread tr1 = new Thread(mp,"窗口一");
        Thread tr2 = new Thread(mp,"窗口二");
        Thread tr3 = new Thread(mp,"窗口三");

        //开启线程
        tr1.start();
        tr2.start();
        tr3.start();
    }
}
```
解决方案3 lock锁
``` Java
public class MyPiao implements Runnable {

    private int piao = 100;     //代表总票数
    Lock lock = new ReentrantLock();        //创建Lock锁对象
    @Override
    public void run() {

        for(;;){
            lock.lock();        //加锁
            //判断，只要总票数>0，可以卖票
            if(piao > 0){
                try {
                    Thread.sleep(100);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }       //   1   2   3
                System.out.println(Thread.currentThread().getName() + "正在卖第"+ piao +"张票");
                piao--;
            }
            lock.unlock();      //释放锁
        }
    }
}
public class Test1 {
    public static void main(String[] args) {
        //创建线程任务
        MyPiao mp = new MyPiao();
        Thread tr1 = new Thread(mp,"窗口一");
        Thread tr2 = new Thread(mp,"窗口二");
        Thread tr3 = new Thread(mp,"窗口三");

        //开启线程
        tr1.start();
        tr2.start();
        tr3.start();
    }
}
```