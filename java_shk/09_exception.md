# 第09章 异常处理

## 124 异常处理 异常的概述与常见异常的举例

```text
1. 什么是异常？
指的是程序在执行过程中，出现的非正常情况，如果不处理最终会导致JVM的非正常停止。

2. 异常的抛出机制
Java中把不同的异常用不同的类表示，一旦发生某种异常，就创建该异常类型的对象，并且抛出(throw)。
然后程序员可以捕获(catch)到这个异常对象，并处理; 如果没有捕获(catch)这个异常对象，那么这个异常对象将会导致程序终止。

3. 如何对待异常
对于程序出现的异常，一般有两种解决方法: 一是遇到错误就终止程序的运行。另一种方法是程序员在编写程序时，
就充分考虑到各种可能发生的异常和错误，极力预防和避免。实在无法避免，要编写相应的代码进行异常的检测、
以及'异常的处理'，保证代码的'健壮性'。

4. 异常的体系结构
java.lang.Throwable: 异常体系的根父类
    |---java.lang.Error: 错误。Java虚拟机无法解决的严重问题。如: JVM系统内部错误、资源耗尽等严重情况。
                        一般不编写针对性的代码进行处理。
                        |--- StackOverflowError、OutOfMemoryError

    |---java.lang.Exception: 异常。我们可以编写针对性的代码进行处理。
        |---编译时异常: (受检异常)在执行javac.exe命令时，出现的异常。
            |---ClassNotFoundException
            |---FileNotFoundException
            |---IOException

        |---运行时异常: (非受检异常)在执行java.exe命令时，出现的异常。
            |---ArrayIndexOutOfBoundsException
            |---NullPointerException
            |---CLassCastException
            |---NumberFormatException
            |---InputMismatchException
            |---ArithmeticException
```

```text
[面试题] 说说你在开发中常见的异常都有哪些？

开发1-2年:
|---编译时异常: (受检异常)在执行javac.exe命令时，出现的异常。
    |---ClassNotFoundException
    |---FileNotFoundException
    |---IOException
|---运行时异常: (非受检异常)在执行java.exe命令时，出现的异常。
    |---ArrayIndexOutOfBoundsException
    |---NullPointerException
    |---CLassCastException
    |---NumberFormatException
    |---InputMismatchException
    |---ArithmeticException

开发3年以上:
OOM。
```

```java
package com.atguigu01.throwable;

public class ErrorTest {
    public static void main(String[] args) {

        // 举例1: 栈内存溢出的错误 StackOverflowError
        // main(args);

        // 举例2: OutOfMemoryError: Java heap space
        byte[] arr = new byte[1024 * 1024 * 100]; // 100MB

        // 要让上面的代码发生错误，打开当前类的运行选项: Edit Configurations...
        // 点击Modify options -> Add VM options -> 填写: -Xms10m -Xmx10m
    }
}
```

```java
package com.atguigu01.throwable;

import org.junit.Test;

import java.io.File;
import java.io.FileInputStream;
import java.io.IOException;
import java.util.Date;
import java.util.Scanner;

public class ExceptionTest {

    // ArrayIndexOutOfBoundsException
    @Test
    public void test1() {
        int[] arr = new int[10];
        System.out.println(arr[10]);
    }

    // NullPointerException
    @Test
    public void test2() {
        // String str = "hello";
        // str = null;
        // System.out.println(str.toString());

        // int[] arr = new int[10];
        // arr = null;
        // System.out.println(arr[10]);

        int[][] arr1 = new int[10][];
        System.out.println(arr1[0][0]);
    }

    // ClassCastException
    @Test
    public void test3() {
        Object obj = new String();
        // String str = (String) obj;

        Date date = (Date) obj;
    }

    // NumberFormatException
    @Test
    public void test4() {
        String str = "123";
        str = "abc";
        int i = Integer.parseInt(str);
        System.out.println(i);
    }

    // InputMismatchException
    @Test
    public void test5() {
        Scanner scanner = new Scanner(System.in);
        int num = scanner.nextInt();
        System.out.println(num);
    }

    // ArithmeticException
    @Test
    public void test6() {
        int num = 10;
        System.out.println(num / 0);
    }

    // ********** 以上是运行时异常，以下是编译时异常 **********

    // CLassNotFoundException
    @Test
    public void test7() {
        // Class clazz = Class.forName("java.lang.String");
    }

    @Test
    public void test8() throws IOException {
        File file = new File("hello.txt");

        FileInputStream fis = new FileInputStream(file); // 可能报FileNotFoundException

        int data = fis.read(); // 可能报IOException
        while (data != -1) {
            System.out.print((char) data);
            data = fis.read(); // 可能报IOException
        }

        fis.close(); // 可能报IOException
    }
}
```

## 125 异常处理 异常处理方式一: try catch的使用

```text
1. 方式一 (抓抛模型): try-catch-finally

过程1: "抛"
    程序在执行的过程当中，一旦出现异常，就会在出现异常的代码处，生成对应异常类的对象，并将此对象抛出。
    一旦抛出，此程序就不执行其后的代码了。

过程2: "抓"
    针对于过程1中抛出的异常对象，进行捕获处理。此捕获处理的过程，就称为抓。
    一旦将异常进行了处理，代码就可以继续执行。

2. 基本结构:
try {
    ...... // 可能产生异常的代码
} catch (异常类型1 e) {
      ...... // 当产生异常类型1时的处置措施
} catch (异常类型2 e) {
    ...... // 当产生异常类型2时的处置措施
} finally {
    ...... // 无论是否发生异常，都无条件执行的语句
}

3. 使用细节:
> 将可能出现异常的代码声明在try语句中。一旦代码出现异常，就会自动生成一个对应异常的对象。并将此对象抛出。
> 针对于try中抛出的异常类的对象，使用之后的catch语句进行匹配。一旦匹配上，就进入catch语句块进行处理。
    一旦处理结束，代码就可以继续向下执行。
> 如果声明了多个catch结构，不同的异常类型在不存在子父类的情况下，谁声明在上面，谁声明在下面都可以。
    如果多个异常类型满足子父类的关系，则必须将子类声明在父类结构的上面。否则，报错。
> catch中异常处理的方式:
    1) 自己编写输出的语句。
    2) printStackTrace(): 打印异常的详细信息。(推荐)
    3) getMessage(): 获取发生异常的原因。
> try中声明的变量，出了try结构之后，就不可以进行调用了。

4. 开发体会:
    > 对于运行时异常:
        开发中，通常就不进行显式的处理了。
        一旦在程序执行中，出现了运行时异常，那么就根据异常的提示信息修改代码即可。

    > 对于编译时异常:
        一定要进行处理，否则编译不通过。
```

```java
package com.atguigu02.trycatchfinally;

import org.junit.Test;

import java.io.File;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;
import java.util.InputMismatchException;
import java.util.Scanner;

public class ExceptionHandleTest {

    @Test
    public void test1() {
        try {
            Scanner scanner = new Scanner(System.in);
            int num = scanner.nextInt();
            System.out.println(num);
        } catch (NullPointerException e) {
            System.out.println("出现了NullPointerException的异常");
        } catch (InputMismatchException e) {
            System.out.println("出现了InputMismatchException的异常");
        } catch (RuntimeException e) {
            System.out.println("出现了RuntimeException的异常");
        }

        System.out.println("异常处理结束，代码继续执行...");
    }

    @Test
    public void test2() {
        try {
            String str = "123";
            str = "abc";
            int i = Integer.parseInt(str);
            System.out.println(i);
        } catch (NumberFormatException e) {
            e.printStackTrace();
            // System.out.println(e.getMessage());
        }
        System.out.println("程序结束");
        // System.out.println(str);
    }

    // ********* 下面来处理编译时异常 *********
    @Test
    public void test3() {
        try {
            File file = new File("hello.txt");

            FileInputStream fis = new FileInputStream(file); // 可能报FileNotFoundException

            int data = fis.read(); // 可能报IOException
            while (data != -1) {
                System.out.print((char) data);
                data = fis.read(); // 可能报IOException
            }

            fis.close(); // 可能报IOException
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }

        System.out.println("读取数据结束...");
    }
}
```
