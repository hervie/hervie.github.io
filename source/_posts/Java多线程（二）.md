---
title: Java多线程（二）
date: 2019-12-03 21:57:43
tags:
    - 多线程
categories: Java
---

# 多线程
## 单例设计模式
饿汉式和懒汉式区别：
- 饿汉式是空间换时间，懒汉式是时间换空间。
- 在多线程访问时，饿汉式不会创建多个对象，但是懒汉式可能会创建多个对象。
### 饿汉式
```java
class Singleton{
    
    private Singleton(){}

    private static Singleton s = new Singleton();
    
    public static Singleton getInstance() {
        return s;
    }
}
```
<!--more-->
### 懒汉式
```java
class Singleton{
    
    private Singleton(){}
    
    private static Singleton s;
    
    public static Singleton getInstance(){
        if(s == null){
            s = new Singleton();
        }
        return s;
    }
    
}
```


## Runtime

```java
import java.io.IOException;

public class ThreadDemo15 {
    public static void main(String[] args) {
        Runtime rt = Runtime.getRuntime();
        try {
            rt.exec("shutdown -a");
        } catch (IOException e) {
            e.printStackTrace();
        }

    }
}
```

## Timer
执行定时任务
```java
import java.util.Date;
import java.util.Timer;
import java.util.TimerTask;

public class ThreadDemo14 {
    public static void main(String[] args) throws InterruptedException {
        Timer t = new Timer();
        Date date = new Date(119, 6, 16, 17, 0, 50);
        System.out.println(date);
        t.schedule(new MyTask(), date, 3000);
        int i = 100;
        while (i-- > 0){
            Thread.sleep(1000);
            System.out.println(i);
        }
    }
}

class MyTask extends TimerTask {

    @Override
    public void run() {
        System.out.println("定时任务");
    }
}

```

## 线程间通信
1. 什么时候需要线程间通信？
- 多个线程并发执行时，在默认情况下CPU是随机切换线程的
- 如果我们希望进程有规律的执行，就可以使用通信，例如每个线程执行一次打印
2. 怎么通信？
- 如果希望线程等待，就用wait()
- 如果希望唤醒等待的线程，就调用notify()
- 这两个方法必须在同步代码中执行，并且使用同步锁对象来调用


```java
public class ThreadDemo16 {
    static int i = 100;
    public static void main(String[] args) {

        Print2 p = new Print2();
        new Thread() {
            public void run() {
                while (i-- >0){
                    try {
                        p.print1();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
            }
        }.start();
        new Thread() {
            public void run() {
                while (i-- >0){
                    try {
                        p.print2();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
            }
        }.start();
    }

}


class Print2 {
    int flag = 1;
    public void print1() throws InterruptedException {
        synchronized (this) {
            if (flag != 1){
                this.wait();
            }
            System.out.print("理");
            System.out.print("想");
            System.out.print("今");
            System.out.print("年");
            System.out.print("你");
            System.out.print("几");
            System.out.print("岁");
            System.out.println();
            flag = 2;
            this.notify();
        }
    }

    public void print2() throws InterruptedException {
        synchronized (this) {
            if (flag != 2){
                this.wait();
            }
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
            flag = 1;
            this.notify();
        }
    }
}

```
三个或者三个以上，需要用notifyAll();

1. 在同步代码块中，用哪个对象锁，就用哪个对象调用wait方法
2. 为什么wait和notify定义在Object这个类中
因为锁对象可以是任意对象，Object是所有类的基类，所以wait和notify需要定义在Object中
3. sleep和wait的区别
- sleep必须传递参数，参数就是时间，时间到了自动醒来；wait方法可以传入参数，也可以不传入参数。传入参数就是参数的时间结束后等待，不传入参数就是直接等待。
- sleep在同步函数，或者同步代码块中，不释放锁；wait在同步函数，或者代码块中释放锁。


## 互斥锁

1. 同步
- 使用ReentrantLock类的lock()和unlock()进行同步
2. 通信
- 使用ReentrantLock类的newCondition()方法可以获取Condition对象
- 需要等待的时候，使用Condition的await()方法，唤醒时使用signal()方法
- 不同的线程使用不同的Condition，这样就能区分唤醒的时候找哪个线程了


```java
import java.util.concurrent.locks.Condition;
import java.util.concurrent.locks.ReentrantLock;

public class ThreadDemo17 {
    static int i = 10;

    public static void main(String[] args) {
        Print3 p =new Print3();

        new Thread(){
            public void run(){
                while (i-- > 0 ){
                    try {
                        p.print1();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
            }
        }.start();        new Thread(){
            public void run(){
                while (i-- > 0 ){
                    try {
                        p.print2();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
            }
        }.start();
        new Thread() {
            public void run() {
                while (i-- > 0) {
                    try {
                        p.print3();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
            }
        }.start();
    }
}

class Print3 {
    private ReentrantLock r = new ReentrantLock();
    private Condition c1 = r.newCondition();
    private Condition c2 = r.newCondition();
    private Condition c3 = r.newCondition();
    private int flag = 1;

    public void print1() throws InterruptedException {
        r.lock();
        if (flag != 1) {
            c1.await();
        }
        System.out.print("理");
        System.out.print("想");
        System.out.print("今");
        System.out.print("年");
        System.out.print("你");
        System.out.print("几");
        System.out.print("岁");
        System.out.println();
        flag = 2;
        c2.signal();
        r.unlock();
    }

    public void print2() throws InterruptedException {
        r.lock();
        if (flag != 2) {
            c2.await();
        }
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
        flag = 3;
        c3.signal();
        r.unlock();
    }

    public void print3() throws InterruptedException {
        r.lock();
        if (flag != 3){
            c3.await();
        }
        System.out.print("理");
        System.out.print("想");
        System.out.println();
        flag = 1;
        c1.signal();
        r.unlock();
    }
}
```

## 线程组
- java中使用ThreadGroup来表示线程组，它可以对一批线程进行分类管理，Java允许程序直接对线程组进行控制。
- 默认情况下，所有的线程都属于主线程组
    - public final ThreadGroup getThreadGroup()//通过线程对象获取它所属的组。
    - public fianl String getName()//通过线程组对象获取它组的名字
- 给线程设置分组
    - ThreadGroup(String name)创建线程组对象并给其赋值名字
    - 创建线程对象
    - Thread(ThreadGroup group, Runnable target, String name)
    - 设置整组的优先级或者守护线程
    

```java
import java.util.TreeSet;

public class ThreadDemo18 {
    public static void main(String[] args) {

        ThreadGroup tg = new ThreadGroup("线程组aaaa");

        TestThread tst = new TestThread();
        new Thread(tst,"线程1").start();
        new Thread(tst,"线程2").start();
        new Thread(tst,"线程3").start();
        new Thread(tg,tst,"线程a").start();
    }
}

class TestThread implements Runnable{

    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName() + "：这个进程的进程组是" + Thread.currentThread().getThreadGroup().getName());
    }
}
```