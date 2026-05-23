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
> try-catch结构是可以嵌套使用的。

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

## 126 异常处理 finally的使用

```text
5. finally的使用说明:
5.1 finally的理解
> 我们将一定要被执行的代码声明在finally结构中。
> 更深刻的理解: 无论try中或catch中是否存在仍未被处理的异常，无论try中或catch中是否存在return语句等，
    finally中声明的语句都一定要被执行。

> finally语句和catch语句是可选的，但finally不能单独使用。

5.2 什么样的代码我们一定要声明在finally中呢？
> 我们在开发中，有一些资源(比如: 输入流，输出流，数据库连接，Socket连接等资源)，在使用完以后，必须显式地进行
    关闭操作，否则，GC不会自动地回收这些资源，进而导致内存等泄漏。
    为了保证这些资源在使用完以后，不管是否出现了未被处理的异常的情况下，这些资源能被关闭。我们必须将这些操作声明在finally中！

6. 面试题
final、finally、finalize的区别
```

```java
package com.atguigu02.trycatchfinally;

import org.junit.Test;

import java.io.File;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;

public class FinallyTest {

    @Test
    public void test1() {
        try {
            String str = "123";
            str = "abc";
            int i = Integer.parseInt(str);
            System.out.println(i);
        } catch (NumberFormatException e) {
            e.printStackTrace();

            System.out.println(10 / 0);// 在catch中存在异常
        }
        System.out.println("程序结束");
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

            System.out.println(10 / 0);// 在catch中存在异常
        } finally {
            System.out.println("程序结束");
        }
    }

    @Test
    public void test3() {
        try {
            String str = "123";
            str = "abc";
            int i = Integer.parseInt(str);
            System.out.println(i);
        } finally {
            System.out.println("程序结束");
        }
    }

    // 实际开发中，finally的使用。
    @Test
    public void test4() {
        FileInputStream fis = null;
        try {
            File file = new File("hello.txt");

            fis = new FileInputStream(file); // 可能报FileNotFoundException

            int data = fis.read(); // 可能报IOException
            while (data != -1) {
                System.out.print((char) data);
                data = fis.read(); // 可能报IOException
            }
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            // 重点: 将流资源的关闭操作声明于finally中
            try {
                if (fis != null)
                    fis.close(); // 可能报IOException
            } catch (IOException e) {
                e.printStackTrace();
            }
        }

    }
}
```

- 面试题

```java
package com.atguigu02.trycatchfinally.interview;

public class FinallyTest1 {
    public static void main(String[] args) {
        int result = test("12");
        System.out.println(result);
    }

    public static int test(String str) {
        try {
            Integer.parseInt(str);
            return 1;
        } catch (NumberFormatException e) {
            return -1;
        } finally {
            System.out.println("test结束");
        }
    }
}
```

```java
package com.atguigu02.trycatchfinally.interview;

public class FinallyTest2 {
    public static void main(String[] args) {
        int result = test("a");
        System.out.println(result);
    }

    public static int test(String str) {
        try {
            Integer.parseInt(str);
            return 1;
        } catch (NumberFormatException e) {
            return -1;
        } finally {
            System.out.println("test结束");
        }
    }
}
```

```java
package com.atguigu02.trycatchfinally.interview;

public class FinallyTest3 {
    public static void main(String[] args) {
        int result = test("a");
        System.out.println(result);
    }

    public static int test(String str) {
        try {
            Integer.parseInt(str);
            return 1;
        } catch (NumberFormatException e) {
            return -1;
        } finally {
            System.out.println("test结束");
            return 0;
        }
    }
}
```

```java
package com.atguigu02.trycatchfinally.interview;

public class FinallyTest4 {
    public static void main(String[] args) {
        int result = test(10);
        System.out.println(result);
    }

    public static int test(int num) {
        try {
            return num;
        } catch (NumberFormatException e) {
            return num--;
        } finally {
            System.out.println("test结束");
            // return ++num; // 11
            ++num; // 10
        }
    }
}
```

## 127 异常处理 异常处理方式二: throws

```text
异常处理的方式2: throws
1. 格式: 在方法的声明处，使用"throws 异常类型1, 异常类型2,..."

2. 举例:
public void test() throws 异常类型1, 异常类型2,... {
    // 可能存在编译时异常
}

3. 是否真正处理了异常？
> 从编译是否能通过的角度看，看成是给出了异常万一要是出现时候的解决方案。此方案就是: 继续向上抛出(throws)。
> 但是，此throws的方式，仅是将可能出现的异常抛给了此方法的调用者。此调用者仍然需要考虑如何处理相关异常。
    从这个角度来看，throws的方式不算是真正意义上处理了异常。

4. 方法的重写的要求: (针对于编译时异常来说)
子类重写的方法抛出的异常类型可以与父类被重写的方法抛出的异常类型相同，或是父类被重写的方法抛出的异常类型的子类。

5. 开发中，如何选择异常处理的两种方式？ (重要、经验之谈)
- 如果程序代码中，涉及到资源的调用(流、数据库连接、网络连接等)，则必须考虑使用try-catch-finally，
    保证不出现内存泄漏。
- 如果父类被重写的方法没有throws异常类型，则子类重写的方法中如果出现异常，只能考虑使用try-catch-finally进行处理，不能throws。
- 开发中，方法a中依次调用了方法b，c，d等方法，方法b，c，d之间是递进关系。此时，如果方法b，c，d中有异常，
    我们通常选择使用throws，而方法a中通常选择使用try-catch-finally。
```

```java
package com.atguigu03._throws;

import java.io.File;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;

public class ThrowsTest {
    public static void main(String[] args) {
        method3();

        // method2();
    }

    public static void method3() {
        try {
            method2();
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    public static void method2() throws FileNotFoundException, IOException {
        method1();
    }

    public static void method1() throws FileNotFoundException, IOException {
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

```java
package com.atguigu03._throws;

import java.io.FileNotFoundException;
import java.io.IOException;

public class OverrideTest {
    public static void main(String[] args) {

        Father f = new Son();

        try {
            f.method1();
        } catch (IOException e) {
            e.printStackTrace();
        }

        Number number = f.method4();
    }
}

class Father {

    public void method1() throws IOException {

    }

    public void method2() {
    }

    public void method3() {
    }

    public Number method4() {
        return null;
    }
}

class Son extends Father {
    public void method1() throws FileNotFoundException {
    }

    // public void method2() throws FileNotFoundException {
    // }

    public void method3() throws RuntimeException {
    }

    public Integer method4() {
        return null;
    }
}
```

## 128 异常处理 使用throw手动抛出异常对象

```text
1. 为什么需要手动抛出异常对象？

在实际开发中，如果出现不满足具体场景的代码问题，我们就有必要手动地抛出一个指定类型的异常对象。

2. 如何理解"自动 vs 手动"抛出异常对象？

过程1: "抛"
    "自动抛": 程序在执行的过程当中，一旦出现异常，就会在出现异常的代码处，自动生成对应异常类的对象，并将此对象抛出。

    "手动抛": 程序在执行的过程当中，不满足指定条件的情况下，我们主动地使用"throw + 异常类的对象"的方式抛出异常对象。

过程2: "抓"
    狭义上讲: try-catch的方式捕获异常，并处理。
    广义上讲: 把"抓"理解为"处理"。则此时对应着异常处理的两种方式: 1) try-catch-finally 2) throws。

3. 如何实现手动抛出异常？
在方法内部，满足指定条件的情况下，使用"throw 异常类的对象"的方式抛出。

4. 注意点: throw后的代码不能被执行，编译不通过。

5. 面试题: throw 和 throws的区别？ "上有排污，下游治污"
```

```java
package com.atguigu04._throw;

public class ThrowTest {
    public static void main(String[] args) {
        Student s1 = new Student();

        try {
            s1.regist(10);
            s1.regist(-10);
            System.out.println(s1);
        } catch (Exception e) {
            // e.printStackTrace();
            System.out.println(e.getMessage());
        }
    }
}

class Student {
    int id;

    /*
    public void regist(int id) {
        if (id > 0) {
            this.id = id;
        } else {
            // System.out.println("输入的id非法");
            // 手动抛出异常类的对象
            throw new RuntimeException("输入的id非法");
        }
    }
     */

    public void regist(int id) throws Exception {
        if (id > 0) {
            this.id = id;
        } else {
            // System.out.println("输入的id非法");
            // 手动抛出异常类的对象
            throw new Exception("输入的id非法");
            // System.out.println("此语句不能被执行");
        }
    }

    @Override
    public String toString() {
        return "Student{" +
                "id=" + id +
                '}';
    }
}
```

- 练习题

```text
修改chapter08_oop3中接口部分的exer2，在ComparableCircle接口的compareTo()中抛出异常。
```

```java
package com.atguigu05.exer;

public class Circle {
    private double radius; // 半径

    public Circle() {
    }

    public Circle(double radius) {
        this.radius = radius;
    }

    public double getRadius() {
        return radius;
    }

    public void setRadius(double radius) {
        this.radius = radius;
    }

    @Override
    public String toString() {
        return "Circle{" +
                "radius=" + radius +
                '}';
    }
}
```

```java
package com.atguigu05.exer;

public interface CompareObject {
    // 若返回值是0，代表相等; 若为正数，代表当前对象大; 负数代表当前对象小。
    public int compareTo(Object o) throws Exception;
}
```

```java
package com.atguigu05.exer;

public class ComparableCircle extends Circle implements CompareObject {

    public ComparableCircle() {
    }

    public ComparableCircle(double radius) {
        super(radius);
    }

    // 根据对象的半径的大小，比较对象的大小。
    @Override
    public int compareTo(Object o) throws Exception {
        if (this == o) {
            return 0;
        }

        if (o instanceof ComparableCircle) {
            ComparableCircle c = (ComparableCircle) o;
            return Double.compare(this.getRadius(), c.getRadius());
        } else {
            // throw new RuntimeException("输入的类型不匹配");
            throw new Exception("输入的类型不匹配");
        }
    }
}
```

```java
package com.atguigu05.exer;

public class InterfaceTest {
    public static void main(String[] args) {

        ComparableCircle c1 = new ComparableCircle(2.3);
        ComparableCircle c2 = new ComparableCircle(5.3);

        try {
            int compareValue = c1.compareTo(c2);

            if (compareValue > 0) {
                System.out.println("c1对象大");
            } else if (compareValue < 0) {
                System.out.println("c2对象大");
            } else {
                System.out.println("c1和c2一样大");
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

## 129 异常处理 如何自定义异常类及课后练习

```text
1. 如何自定义异常类？
1) 继承于现有的异常体系。通常继承于RuntimeException \ Exception。
2) 通常提供几个重载的构造器。
3) 提供一个全局常量，声明为: static final long serialVersionUID;

2. 如何使用自定义异常类？
> 在具体的代码中，满足指定条件的情况下，需要手动地使用"throw + 自定义异常类的对象"的方式，将异常对象抛出。
> 如何自定义异常类是非运行时异常，则必须考虑如何处理此异常类的对象。(具体的: [1] try-catch-finally [2] throws)

3. 为什么需要自定义异常类？
我们其实更关心的是，通过异常的名称就能直接判断此异常出现的原因。既然如此，我们就有必要在实际的开发场景中，
不满足我们指定的条件时，指明我们自己特有的异常类。通过此异常类的名称，就能判断出具体出现的问题。
```

```java
package com.atguigu04._throw;

public class BelowZeroException extends Exception {

    static final long serialVersionUID = -3387516999948L;

    public BelowZeroException() {
    }

    public BelowZeroException(String message) {
        super(message);
    }

    public BelowZeroException(String message, Throwable cause) {
        super(message, cause);
    }
}
```

- 练习题

```java
package com.atguigu05.exer.exer2;

/**
 * ClassName: ReturnExceptionDemo
 * Package: com.atguigu05.exer.exer2
 * Description:
 *
 * @Author: ljy
 * @Create: 2026. 5. 23. 오전 9:23
 * @Version 1.0
 */
public class ReturnExceptionDemo {

    static void methodA() throws Exception {
        try {
            System.out.println("进入方法A");
            throw new Exception("制造异常");
        } finally {
            System.out.println("调用A方法的finally");
        }
    }

    static void methodB() {
        try {
            System.out.println("进入方法B");
            return;
        } finally {
            System.out.println("调用B方法的finally");
        }
    }

    public static void main(String[] args) {
        try {
            methodA();
        } catch (Exception e) {
            System.out.println(e.getMessage());
        }

        methodB();
    }
}
/*
进入方法A
调用A方法的finally
制造异常
进入方法B
调用B方法的finally
*/
```

```text
案例：游戏角色

在一款角色扮演游戏中，每一个人都会有名字和生命值，角色的生命值不能为负数。

要求：当一个人物的生命值为负数的时候需要抛出自定义的异常

操作步骤描述：
（1）自定义异常类NoLifeValueException继承RuntimeException
①提供空参和有参构造
②在有参构造中，需要调用父类的有参构造，把异常信息传入

（2）定义Person类
①属性：名称(name)和生命值(lifeValue)
②提供setter和getter方法：
在setLifeValue(int lifeValue)方法中，首先判断，如果 lifeValue为负数,就抛出NoLifeValueException，
异常信息为：生命值不能为负数：xx；
然后再给成员lifeValue赋值。

③提供空参构造

④提供有参构造：使用setXxx方法给name和lifeValue赋值


（3）定义测试类Exer3

① 使用满参构造方法创建Person对象，生命值传入一个负数

由于一旦遇到异常,后面的代码的将不在执行,所以需要注释掉上面的代码

② 使用空参构造创建Person对象

调用setLifeValue(int lifeValue)方法,传入一个正数,运行程序

调用setLifeValue(int lifeValue)方法,传入一个负数,运行程序

③ 分别对①和②处理异常和不处理异常进行运行看效果
```

```java
package com.atguigu05.exer.exer3;

public class Person {

    private String name;
    private int lifeValue;

    public Person() {
    }

    public Person(String name, int lifeValue) {
        // this.name = name;
        setName(name);
        setLifeValue(lifeValue);
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getLifeValue() {
        return lifeValue;
    }

    public void setLifeValue(int lifeValue) {
        if (lifeValue < 0) {
            throw new NoLifeValueException("生命值不能为负数: " + lifeValue);
        }
        this.lifeValue = lifeValue;
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", lifeValue=" + lifeValue +
                '}';
    }
}
```

```java
package com.atguigu05.exer.exer3;

public class NoLifeValueException extends RuntimeException {

    static final long serialVersionUID = -70348971934506939L;

    public NoLifeValueException() {
    }

    public NoLifeValueException(String message) {
        super(message);
    }
}
```

```java
package com.atguigu05.exer.exer3;

public class NoLifeValueException extends RuntimeException {

    static final long serialVersionUID = -70348971934506939L;

    public NoLifeValueException() {
    }

    public NoLifeValueException(String message) {
        super(message);
    }
}
```

```text
编写应用程序DivisionDemo.java，接收命令行的两个参数，要求不能输入负数，计算两数相除。
    对数据类型不一致(NumberFormatException)、缺少命令行参数(ArrayIndexOutOfBoundsException)、
    除0(ArithmeticException)及输入负数(BelowZeroException: 自定义异常)进行异常处理。

提示:
    (1) 在主类(DivisionDemo)中定义异常方法(divide)完成两数相除的功能。
    (2) 在main()方法中调用divide方法，使用异常处理语句进行异常处理。
    (3) 在程序中，自定义对应输入负数的异常类(BelowZeroException)。
    (4) 运行时接受参数 java DivisionDemo 20 10 // args[0]="20" args[1]="10"
    (5) Integer类的static方法parseInt(String s)将s转换成对应的int值。
        如: int a = Integer.parseInt("314"); // a = 314
```

```java
package com.atguigu05.exer.exer4;

public class BelowZeroException extends Exception {

    static final long serialVersionUID = -3381283129039931222L;

    public BelowZeroException() {
    }

    public BelowZeroException(String message) {
        super(message);
    }
}
```

```java
package com.atguigu05.exer.exer4;

public class DivisionDemo {
    public static void main(String[] args) {
        try {
            int m = Integer.parseInt(args[0]);
            int n = Integer.parseInt(args[1]);
            int result = divide(m, n);
            System.out.println("结果为: " + result);
        } catch (BelowZeroException e) {
            System.out.println(e.getMessage());
        } catch (NumberFormatException e) {
            System.out.println("数据类型不一致");
        } catch (ArrayIndexOutOfBoundsException e) {
            System.out.println("缺少命令行参数");
        } catch (ArithmeticException e) {
            System.out.println("除数不能为0");
        }
    }

    public static int divide(int m, int n) throws BelowZeroException {
        if (m < 0 || n < 0) {
            // 手动抛出异常类的对象
            throw new BelowZeroException("输入负数了");
        }
        return m / n;
    }
}
```
