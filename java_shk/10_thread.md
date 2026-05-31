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

## 135 多线程 Thread的常用方法和声明周期

```text
一、线程的常用结构
1. 线程中的构造器
- public Thread(): 分配一个新的线程对象。
- public Thread(String name): 分配一个指定名字的新的线程对象。
- public Thread(Runnable target): 指定创建线程的目标对象，它实现了Runnable接口中的run方法。
- public Thread(Runnable target, String name): 分配一个带有指定目标新的线程对象并指定名字。

2. 线程中的常用方法:
> start(): 1) 启动线程 2) 调用线程的run()
> run(): 将线程要执行的操作，声明在run()中。
> currentThread(): 获取当前执行代码对应的线程。
> getName(): 获取线程名。
> setName(): 设置线程名。
> sleep(long millis): 静态方法，调用时，可以使得当前线程睡眠指定的毫秒数。
> yield(): 静态方法，一旦执行此方法，就释放CPU的执行权。
> join(): 在线程a中通过线程b调用join()，意味着线程a进入阻塞状态，直到线程b执行结束，线程a才结束阻塞状态，继续执行。
> isAlive(): 判断当前线程是否存活。

过时方法:
> stop(): 强行结束一个线程的执行，直接进入死亡状态。不建议使用。
> suspend() / resume(): 可能造成死锁，所以也不建议使用。

3. 线程的优先级:
getPriority(): 获取线程的优先级。
setPriority(): 设置线程的优先级，范围: [1, 10]。

Thread类内部声明的三个常量:
- MAX_PRIORITY (10): 最高优先级
- MIN_PRIORITY (1): 最低优先级
- NORM_PRIORITY (5): 普通优先级，默认情况下main线程具有普通优先级。

二、线程的生命周期
```

```java
package com.atguigu02.method_lifecycle;

/**
 * ClassName: EvenNumberTest
 * Package: com.atguigu02.method_lifecycle
 * Description:
 *
 * @Author: ljy
 * @Create: 2026. 5. 29. 오전 6:14
 * @Version 1.0
 */
class PrintNumber extends Thread {

    public PrintNumber() {
    }

    public PrintNumber(String name) {
        super(name);
    }

    @Override
    public void run() {
        for (int i = 1; i <= 100; i++) {
            // try {
            //     Thread.sleep(1000);
            // } catch (InterruptedException e) {
            //     throw new RuntimeException(e);
            // }
            if (i % 2 == 0) {
                System.out.println(Thread.currentThread().getName() + ", 优先级: " + Thread.currentThread().getPriority() + ": " + i);
            }

            // if (i % 20 == 0) {
            //     Thread.yield();
            // }
        }
    }
}

public class EvenNumberTest {
    public static void main(String[] args) {
        PrintNumber t1 = new PrintNumber("线程1");

        // 设置线程名
        t1.setName("子线程1");
        t1.setPriority(Thread.MIN_PRIORITY);

        t1.start();

        Thread.currentThread().setName("主线程");
        Thread.currentThread().setPriority(Thread.MAX_PRIORITY);

        for (int i = 1; i <= 100; i++) {
            if (i % 2 == 0) {
                System.out.println(Thread.currentThread().getName() + ", 优先级: "
                        + Thread.currentThread().getPriority() + ": " + i);
            }

            // if (i % 20 == 0) {
            //     try {
            //         t1.join();
            //     } catch (InterruptedException e) {
            //         throw new RuntimeException(e);
            //     }
            // }
        }

        // System.out.println("子线程1是否存活？ " + t1.isAlive()); // false
    }
}
```

![alt text](images/线程的执行周期-jdk5zh之前.png)

## 136 多线程 同步代码块解决两种线程创建方式的线程安全问题

```text
线程的安全问题与线程的同步机制

1. 多线程卖票，出现的问题: 出现了重票和错票的问题。

2. 什么原因导致的？线程1操作ticker的过程中，尚未结束的情况下，其它线程也参与进来，对ticket进行操作。

3. 如何解决？必须保证一个线程在操作ticket的过程中，其它线程必须等待，直到线程a操作ticket结束以后，其它线程才可以进来继续操作ticket。

4. Java是如何解决线程的安全问题的？

方式1: 同步代码块

synchronized(同步监视器) {
    // 需要被同步的代码
}

说明:
> 需要被同步的代码，即为操作共享数据的代码。
> 共享数据: 即多个线程都需要操作的数据。比如: ticketNum。
> 需要被同步的代码，在被synchronized包裹以后，就使得一个线程在操作这些代码的过程中，其它线程必须等待。
> 同步监视器，俗称锁。哪个线程获取了锁，哪个线程就能执行需要被同步的代码。
> 同步监视器，可以使用任何一个类的对象充当。但是，多个线程必须共用同一个同步监视器。

注意: 在实现Runnable接口的方式中，同步监视器可以考虑使用: this。
    在继承Thread类的方式中，同步监视器要慎用this，可以考虑使用: 当前类.class。
```

```java
package com.atguigu03.thread_safe.not_safe;

/**
 * 使用实现Runnable接口的方式，实现卖票。
 */

class SaleTicker implements Runnable {
    private int ticketNum = 100;

    @Override
    public void run() {
        while (true) {
            if (ticketNum > 0) {
                try {
                    Thread.sleep(10);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.println(Thread.currentThread().getName() + " is selling ticket " + ticketNum);
                ticketNum--;
            } else {
                break;
            }
        }
    }
}

public class WindowTest {
    public static void main(String[] args) {
        SaleTicker saleTicker = new SaleTicker();
        Thread thread1 = new Thread(saleTicker, "Window 1");
        Thread thread2 = new Thread(saleTicker, "Window 2");
        Thread thread3 = new Thread(saleTicker, "Window 3");

        thread1.start();
        thread2.start();
        thread3.start();
    }
}
```

```java
package com.atguigu03.thread_safe.not_safe;

/**
 * 使用继承Thread类的方式，实现卖票。
 */
class Window extends Thread {
    static int ticketNum = 100;

    @Override
    public void run() {
        while (true) {
            if (ticketNum > 0) {
                try {
                    Thread.sleep(10);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.println(Thread.currentThread().getName() + " is selling ticket " + ticketNum);
                ticketNum--;
            } else {
                break;
            }
        }
    }
}

public class WindowTest1 {
    public static void main(String[] args) {
        Window w1 = new Window();
        Window w2 = new Window();
        Window w3 = new Window();

        w1.setName("窗口1");
        w2.setName("窗口2");
        w3.setName("窗口3");

        w1.start();
        w2.start();
        w3.start();
    }
}
```

- 解决多线程问题

```java
package com.atguigu03.thread_safe.runnable_safe;

/**
 * 使用实现Runnable接口的方式，实现卖票。 --> 存在线程安全问题。
 * 使用同步代码块解决上述卖票中的线程安全问题。
 */

class SaleTicker implements Runnable {
    private int ticketNum = 100;

    Object obj = new Object();
    Dog dog = new Dog();

    @Override
    public void run() {
        // synchronized (this) { // this: 是唯一的？ yes，就是题目中的saleTicker
        while (true) {
            try {
                Thread.sleep(5);
            } catch (InterruptedException e) {
                throw new RuntimeException(e);
            }
            // synchronized (obj) { // obj: 是唯一的？ yes
            // synchronized (dog) { // dog: 是唯一的？ yes
            synchronized (this) { // this: 是唯一的？ yes，就是题目中的saleTicker
                if (ticketNum > 0) {
                    try {
                        Thread.sleep(10);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    System.out.println(Thread.currentThread().getName() + " is selling ticket " + ticketNum);
                    ticketNum--;
                } else {
                    break;
                }
            }
        }
    }
}

public class WindowTest {
    public static void main(String[] args) {
        SaleTicker saleTicker = new SaleTicker();
        Thread thread1 = new Thread(saleTicker, "Window 1");
        Thread thread2 = new Thread(saleTicker, "Window 2");
        Thread thread3 = new Thread(saleTicker, "Window 3");

        thread1.start();
        thread2.start();
        thread3.start();
    }
}

class Dog {

}
```

```java
package com.atguigu03.thread_safe.thread_safe;

/**
 * 使用继承Thread类的方式，实现卖票。
 * 使用同步代码块的方式解决线程安全问题。
 */
class Window extends Thread {
    static int ticketNum = 100;

    static Object obj = new Object();

    @Override
    public void run() {
        while (true) {
            // synchronized (this) { // this: 此时表示w1，w2，w3。不能保证锁的唯一性。
            // synchronized (obj) { // obj: 使用static修饰以后，就能保证其唯一性。
            synchronized (Window.class) { // 结构: Class clazz = Window.class;是唯一的。
                if (ticketNum > 0) {
                    try {
                        Thread.sleep(10);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    System.out.println(Thread.currentThread().getName() + " is selling ticket " + ticketNum);
                    ticketNum--;
                } else {
                    break;
                }
            }
        }
    }
}

public class WindowTest {
    public static void main(String[] args) {
        Window w1 = new Window();
        Window w2 = new Window();
        Window w3 = new Window();

        w1.setName("窗口1");
        w2.setName("窗口2");
        w3.setName("窗口3");

        w1.start();
        w2.start();
        w3.start();
    }
}
```
