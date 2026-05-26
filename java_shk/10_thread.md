# 第10章 多线程

## 132 多线程 程序、进程、线程与并行、并发的概念

```text
1. 程序、进程和线程的区分:
程序(Program): 为完成特定任务，用某种语言编写的'一组指令的集合'。即指一段静态的代码。

进程(Process): 程序的一次执行过程，或是正在内存中运行的应用程序。程序是静态的，进程是动态的。
        进程是操作系统调度和分配资源的最小单位。

线程(Thread): 进程可进一步细化为线程，是程序内部的一条执行路径。
        线程是CPU调度和执行的最小单位。

2. 线程调度策略
分时调度: 所有线程'轮流使用'CPU的使用权，并且平均分配每个线程占用CPU的时间。

抢占式调度: 让优先级高的线程以较大的概率优先使用CPU。
        如果线程的优先级相同，那么会随机选择一个(线程随机性)，Java使用的是抢占式调度。
```

## 133 多线程 线程创建方式1: 继承Thread类

```java
1. 线程的创建方式一: 继承Thread类。
1.1 步骤:
1) 创建一个继承于Thread类的子类。
2) 重写Thread类的run() --> 将此线程要执行的操作，声明在此方法体中。
3) 创建当前Thread的子类的对象。
4) 通过对象调用start()。1. 启动线程 2. 调用当前线程的run()。

1.2 例题: 创建一个分线程1，用于遍历100以内的偶数。
[拓展]再创建一个分线程2，用于遍历100以内的偶数。
```

```java
package com.atguigu01.create;

// 单线程
public class SingleThread {

    public void method1(String str) {
        System.out.println(str);
    }

    public void method2(String str) {
        method1(str);
    }

    public static void main(String[] args) {
        SingleThread s = new SingleThread();
        s.method2("hello!");
    }
}
```

```java
package com.atguigu01.create.thread;

/**
 * 例题: 创建一个分线程1，用于遍历100以内的偶数。
 */

// 1) 创建一个继承于Thread类的子类
class PrintNumber extends Thread {

    // 2) 重写Thread类的run() --> 将此线程要执行的操作，声明在此方法体中。
    @Override
    public void run() {
        for (int i = 1; i <= 100; i++) {
            if (i % 2 == 0) {
                System.out.println(Thread.currentThread().getName() + ": " + i);
            }
        }
    }
}

public class EvenNumberTest {
    public static void main(String[] args) {
        // 3) 创建当前Thread的子类的对象。
        PrintNumber t1 = new PrintNumber();
        // 4) 通过对象调用start()。
        t1.start();
        // t1.run();

        /*
        问题1: 能否使用t1.run()替换t1.start()的调用，实现分线程的创建和调用？
        不能！如果直接调用run()方法，那么该方法会在当前线程中执行，而不是创建新的线程。
         */

        /*
        问题2: 再提供一个分线程，用于100以内偶数的变量。
        注意: 不能让已经start()的线程，再次执行start()方法，否则报异常IllegalThreadStateException。
         */
        // t1.start();

        PrintNumber t2 = new PrintNumber();
        t2.start();

        // main()所在的线程执行的操作
        for (int i = 1; i <= 100; i++) {
            if (i % 2 == 0) {
                System.out.println(Thread.currentThread().getName() + ": " + i);
            }
        }
    }
}
```

```text
练习: 创建两个分线程，其中一个线程遍历100以内的偶数，另一个线程遍历100以内的奇数。
```

```java
package com.atguigu01.create.exer1;

class EvenNumberPrint extends Thread {
    @Override
    public void run() {
        for (int i = 1; i <= 100; i++) {
            if (i % 2 == 0) {
                System.out.println(Thread.currentThread().getName() + ": " + i);
            }
        }
    }
}

class OddNumberPrint extends Thread {
    @Override
    public void run() {
        for (int i = 1; i <= 100; i++) {
            if (i % 2 != 0) {
                System.out.println(Thread.currentThread().getName() + "--> " + i);
            }
        }
    }
}

public class PrintNumberTest {
    public static void main(String[] args) {
        // 方式1:
        // EvenNumberPrint t1 = new EvenNumberPrint();
        // OddNumberPrint t2 = new OddNumberPrint();

        // t1.start();
        // t2.start();

        // 方式2: 创建Thread类的匿名子类的匿名对象。
        new Thread() {
            @Override
            public void run() {
                for (int i = 1; i <= 100; i++) {
                    if (i % 2 == 0) {
                        System.out.println(Thread.currentThread().getName() + ": " + i);
                    }
                }
            }
        }.start();

        new Thread() {
            @Override
            public void run() {
                for (int i = 1; i <= 100; i++) {
                    if (i % 2 != 0) {
                        System.out.println(Thread.currentThread().getName() + "--> " + i);
                    }
                }
            }
        }.start();
    }
}
```

## 134 多线程 线程创建方式2: 实现Runnable接口

```text
2. 线程的创建方式二: 实现Runnable接口
2.1 步骤:
1) 创建一个实现Runnable接口的类。
2) 实现接口中的run() --> 将此线程要执行的操作，声明在此方法中。
3) 创建当前实现类的对象。
4) 将此对象作为参数传递到Thread类的构造器中，创建Thread类的实例。
5) Thread类的实例调用start(): 1. 启动线程 2. 调用当前线程的run()

2.2 例题: 创建分线程遍历100以内的偶数

3. 对比两种方式？
    共同点: 1) 启动线程，使用的都是Thread类中定义的start()方法。
          2) 创建的线程对象，都是Thread类或其子类的实例。

    不同点: 一个是类的继承，一个是接口的实现。
        建议: 建议使用实现Runnable接口的方式。
        Runnable方式的好处: 1) 实现的方式，避免类的单继承的局限性。
                        2) 更适合处理有共享数据的问题。
                        3) 实现了代码和数据的分离。

    联系: public class Thread implements Runnable (代理模式)
```

```java
package com.atguigu01.create.runnable;

// 1) 创建一个实现Runnable接口的类。
class EvenNumberPrint implements Runnable {
    // 2) 实现接口中的run() --> 将此线程要执行的操作，声明在此方法中。
    @Override
    public void run() {
        for (int i = 0; i <= 100; i++) {
            if (i % 2 == 0) {
                System.out.println(Thread.currentThread().getName() + ": " + i);
            }
        }
    }
}

public class EvenNumberTest {
    public static void main(String[] args) {
        // 3) 创建当前实现类的对象。
        EvenNumberPrint p = new EvenNumberPrint();
        // 4) 将此对象作为参数传递到Thread类的构造器中，创建Thread类的实例。
        Thread t1 = new Thread(p);
        // 5) Thread类的实例调用start(): 1. 启动线程 2. 调用当前线程的run()
        t1.start();

        // main()方法对应的主线程执行的操作
        for (int i = 0; i <= 100; i++) {
            if (i % 2 == 0) {
                System.out.println(Thread.currentThread().getName() + ": " + i);
            }
        }

        /*
        拓展: 再创建一个线程，用于遍历100以内的偶数。
         */
        Thread t2 = new Thread(p);
        t2.start();
    }
}
```

```java
package com.atguigu01.create.exer2;

public class Exer {
    public static void main(String[] args) {
        A a = new A();
        a.start(); // 线程A的run()...

        B b = new B(a);
        b.start(); // 线程B的run()...
    }
}

// 创建线程类A
class A extends Thread {
    @Override
    public void run() {
        System.out.println("线程A的run()...");
    }
}

// 创建线程类B
class B extends Thread {
    private A a;

    public B(A a) {
        this.a = a;
    }

    @Override
    public void run() {
        System.out.println("线程B的run()...");
    }
}
```

```java
package com.atguigu01.create.exer2;

public class Exer_1 {
    public static void main(String[] args) {
        BB b = new BB();
        new Thread(b) {
            @Override
            public void run() {
                System.out.println("CC");
            }
        }.start(); // CC

        new Thread() {
        }.start(); // 无输出
    }
}

class AA extends Thread {
    @Override
    public void run() {
        System.out.println("线程AA的run()...");
    }
}

class BB implements Runnable {
    @Override
    public void run() {
        System.out.println("线程BB的run()...");
    }
}
```
