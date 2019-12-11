---
title: Java多线程（一）
date: 2019-12-03 21:55:43
tags:
    - 多线程
categories: Java
---

# 多线程
1. 什么是线程
- 线程是程序执行的一条路径。一条进程可以包含多条线程。
- 多线程并发执行可以提高程序的执行效率，可以同时完成多项工作。
2. 多线程的应用场景
- 迅雷开启多线程下载
- qq同时和多人一起视频
- 服务端同时处理多个客户端请求

## 并发和并行
- 并行就是两个任务同时运行，甲任务进行的同时，乙任务也在进行。（需要多核CPU）
- 并发指两个任务都请求运行，而处理器只能接受一个任务，就把这两个任务安排轮流执行，由于切换时间比较快，使人感觉两个任务都在运行。

## Java程序运行原理
1. Java程序运行原理。
- Java命令会启动java虚拟机，启动jvm，等于启动了一个应用程序，也就是启动了一个线程。该进程会自动启动一个“主线程”，然后主线程去调用某个类的main方法。
2. Java启动是多线程
jvm至少启动了垃圾回收线程和主线程。
<!--more-->
## 多线程的实现方法
- 多次启动（start）一个线程是非法的
### 继承Thread
1. 定义类继承Thread
2. 重新run放法。
3. 把新线程要做的事写在run中。
4. 创建线程对象
5. 开启新线程，内部会自动执行run方法。

```java
public class ThreadDemo1 {

    public static void main(String[] args) {
        Thread1 t1 = new Thread1();
        t1.start();

        System.out.println("主进程执行");
        for (int i = 0; i < 100000; i++) {
            System.out.println("主进程执行.....................");
        }
    }

}

class Thread1 extends Thread{
    @Override
    public void run() {
        System.out.println("多进程开启");
        for (int i = 0; i < 100000; i++) {
            System.out.println(".....................进程1在执行");
        }
    }
}

```


### 实现Runnable接口
1. 定义类实现Runnable接口
2. 实现run方法
3. 把新线程要做的事情写在run中。
4. 创建自定义的Runnable的子类对象中
5. 创建Thread对象，传入Runnable
6. 调用start()开启新线程，内部会自动的调用Runnable的run方法。

```java
public class ThreadDemo2 {
    public static void main(String[] args) {

//        开启多进程
        Runnable target = new Thread2();
        Thread tr = new Thread(target);
        tr.start();

        for (int i = 0; i < 1000000; i++) {

            System.out.println("。。。。。。。。。。。。。。。。。。主进程");
        }
    }
}

class Thread2 implements Runnable{

    @Override
    public void run() {
        System.out.println("实现Runnable接口的多进程");
        for (int i = 0; i < 100000; i++) {
            System.out.println("多进程。。。。。。。。。。。。。。。。");
        }
    }
}



```

### 实现Runnable的原理
查看源码：
- 看Thread的构造方法，传递了Runnable接口的引用
- 通过init()方法找到传递的target给成员变量的target赋值
- 查看run方法，发现run方法中有判断，如果target不为null，就会调用Runnable接口子类对象的run方法。


### 两种实现方式的比较
1. 继承Thread：由于子类重写了Thread类的Run方法，当调用start时候，查找子类的run()
2. 实现Runnable：构造函数中传入了Runnable的引用，成员变量记住了它，start()调用run()方法时内部判断成员变量Runnable的引用是否为空，不为空编译时看的是Runnable()的run()，运行时执行的是子类的run()方法
#### 优缺点
继承Thread：
- 优点：可以直接使用Thread类中的方法，代码简单
- 缺点：如果已经有了父类，就不能用这种方法
实现Runnable：
- 优点：即使自己定义的线程类有了父类也没有关系，因为类是单继承，接口可以多实现。
- 缺点：不能直接使用Thread中的方法，需要先获取到线程对象后，才能得到Thread的方法，代码复杂。



### 匿名内部类的方式创建进程

```java
public class ThreadDemo3 {
    public static void main(String[] args) {
        new Thread() {
            public void run() {
                for (int i = 0; i < 10000; i++) {
                    System.out.println("匿名内部类的方式创建线程。。。。。");
                }
            }
        }.start();

        new Thread(new Runnable() {
            public void run() {
                for (int i = 0; i < 10000; i++) {
                    System.out.println(".............bbb");
                }
            }
        }).start();

    }
}

```

## 多线程获取和设置名字
1. 获取名字
- 通过getName()
2. 设置名字
- 通过构造函数可以传入String类型的名字
- 通过setName(String)可以设置对象的名字


## 获取当前线程

Thread.currentThread() 返回当前正在执行对象的引用


## 休眠线程

sleep(毫秒,纳秒)

```java
public class ThreadDemo4 {
    public static void main(String[] args) {

        new Thread(){
            public void run(){
                for (int i = 20; i > 0; i--) {
                    try {
                        sleep(1000);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    System.out.println("倒计时:" + i);
                }
            }
        }.start();

    }
}

```

## 守护线程（setDaemon）
设置一个线程为守护线程。该线程不会单独执行，当其他非守护线程都执行结束后，自动退出。


```java
public class ThreadDemo5 {
    public static void main(String[] args) {
        Thread t1 = new Thread(){
            public void run(){
                for (int i = 0; i < 2; i++) {
                    System.out.println("非守护线程");
                }
            }
        };

        Thread t2 = new Thread(){
            public void run(){
                for (int i = 0; i < 100; i++) {
                    System.out.println("守护线程");
                }
            }
        };

        t2.setDaemon(true);
        t1.start();
        t2.start();
    }
}

```

## 加入线程（join）
- join():当前线程暂停，等待指定的线程执行结束后，当前线程再继续。
- join(int),可以等待指定的毫秒之后继续。

```java
public class ThreadDemo6 {
    public static void main(String[] args) {
        final Thread t1 = new Thread(){
            public void run(){
                for (int i = 0; i < 10; i++) {
                    System.out.println("线程1.。。。。。。。。。。。。");
                }
            }
        };

        Thread t2 = new Thread(){
            public void run(){
                for (int i = 0; i < 100; i++) {
                    if(i == 2){
                        try {
//                            t1.join();
                            t1.join(1);
                        } catch (InterruptedException e) {
                            e.printStackTrace();
                        }
                    }
                    System.out.println("。。。。。。。。。。。。。。线程2");
                }
            }
        };

        t1.start();
        t2.start();
    }
}

```

## 礼让线程（yield）
yield让出CPU

```java
public class ThreadDemo7 {

    public static void main(String[] args) {
        Thread3 t1 = new Thread3();
        Thread3 t2 = new Thread3();

        t1.start();
        t2.start();
    }

}

class Thread3 extends Thread{

    @Override
    public void run() {
        System.out.println("实现Runnable接口的多进程");
        for (int i = 0; i < 1000; i++) {
            if (i % 10 == 0)
            {
                Thread.yield();
                System.out.println(getName() + "---" + i);
            }
        }
    }
}
```

## 设置优先级(setPriority)
setPriority()设置线程的优先级

```java
public class ThreadDemo7 {

    public static void main(String[] args) {
        Thread3 t1 = new Thread3();
        Thread3 t2 = new Thread3();

        t1.setPriority(10);
        t2.setPriority(1);
        t1.start();
        t2.start();
    }

}

class Thread3 extends Thread{

    @Override
    public void run() {
        System.out.println("实现Runnable接口的多进程");
        for (int i = 0; i < 1000; i++) {
                System.out.println(getName() + "---" + i);
        }
    }
}
```

## 同步代码块
1. 什么时候用到同步
- 当多线程开发，有多段代码同时执行时，我们希望某一段代码在执行过程中CPU不要切换到其他线程工作，这时就需要同步。
- 如果两段代码是同步的，那么同一时间只能执行一段，在一段代码没执行结束前，不会执行另一段代码。
2. 同步代码块
- 使用synchronized关键字加上一个锁对象来定义一段代码，这就叫同步代码块
- 多个同步代码块如果使用相同的锁对象，那么他们就是同步的。

**锁对象可以是任意对象。**
```java
public class ThreadDemo8 {

    public static void main(String[] args) {
        final Print p = new Print();
        new Thread() {
            public void run() {
                int i = 20;
                while (i>0) {
                    p.print1();
                    i--;
                }
            }
        }.start();
        new Thread() {
            public void run() {
                int i = 20;
                while (i>0) {
                    p.print2();
                    i--;
                }
            }
        }.start();
    }
}

class Print {
    Demo d = new Demo();
    public void print1() {
        synchronized (d){
            System.out.print("理");
            System.out.print("想");
            System.out.print("今");
            System.out.print("年");
            System.out.print("你");
            System.out.print("几");
            System.out.print("岁");
            System.out.println();
        }
    }

    public void print2() {
        synchronized (d){
            System.out.print("你");
            System.out.print("总");
            System.out.print("是");
            System.out.print("诱");
            System.out.print("惑");
            System.out.print("着");
            System.out.print("年");
            System.out.print("轻");
            System.out.print("的");
            System.out.print("朋");
            System.out.print("友");
            System.out.println();
        }
    }
}

class Demo {

}
```

### 非静态同步方法的锁对象是this

```java
class Print1 {
    Demo d = new Demo();
    public synchronized void print1() {
            System.out.print("理");
            System.out.print("想");
            System.out.print("今");
            System.out.print("年");
            System.out.print("你");
            System.out.print("几");
            System.out.print("岁");
            System.out.println();
    }

    public  void print2() {
            synchronized (this){
                System.out.print("你");
                System.out.print("总");
                System.out.print("是");
                System.out.print("诱");
                System.out.print("惑");
                System.out.print("着");
                System.out.print("年");
                System.out.print("轻");
                System.out.print("的");
                System.out.print("朋");
                System.out.print("友");
                System.out.println();
            }
    }
}
```


### 静态同步方法的锁对象是该类的字节码对象（类类型）

```java
class Print1 {
    Demo d = new Demo();
    public static synchronized void print1() {
            System.out.print("理");
            System.out.print("想");
            System.out.print("今");
            System.out.print("年");
            System.out.print("你");
            System.out.print("几");
            System.out.print("岁");
            System.out.println();
    }

    public static void print2() {
        synchronized (Print1.class){
            System.out.print("你");
            System.out.print("总");
            System.out.print("是");
            System.out.print("诱");
            System.out.print("惑");
            System.out.print("着");
            System.out.print("年");
            System.out.print("轻");
            System.out.print("的");
            System.out.print("朋");
            System.out.print("友");
            System.out.println();
        }
    }
}


```

## 线程安全
- 多线程操作同一数据时，就有可能会出现线程安全的问题
- 使用同步技术可以解决这种问题，把操作数据的代码进行同步，不要多个线程一起操作。

```java
public class ThreadDemo10 {
    public static void main(String[] args) {
        new SaleTicket().start();
        new SaleTicket().start();
        new SaleTicket().start();
        new SaleTicket().start();
    }
}


class SaleTicket extends Thread{
    private static int ticket = 100;
    @Override
    public void run() {
        synchronized(SaleTicket.class){
            while (ticket > 0){
                System.out.println(getName() + "号窗口: 第" + ticket + "张票");
                ticket--;
            }
        }
    }
}
```


## 多线程安全

线程安全|线程不安全
-|-
vector|ArrayList
StringBuffer|StringBuilder
Hashtable是|HashMap
