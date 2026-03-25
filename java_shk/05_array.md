# 第05章 数组

## 62 数组 数组的概述

## 63 数组 一维数组的初始化、遍历与元素默认初始化值

## 64 数组 一维数组的内存解析

```text
1. 数组的理解(Array)

概念: 是多个相同类型数据按照一定顺序排列的集合，并使用一个名字命名，并通过编号的方式对这些数据进行统一管理。

简称: 多个数据的组合。

Java中的容器: 数组、集合框架(第12章): 在内存中对多个数据的存储。

2. 几个相关的概念:
> 数组名
> 数组的元素(即内部存储的多个元素)
> 数组的下标、角标、下角标、索引、index(即找到指定数组元素所使用的编号)
> 数组的长度(即数组容器中存储的元素的个数)

3. 数组的特点
> 数组中元素在内存中是依次紧密排列的，有序的。
> 数组，属于引用数据类型的变量。数组的元素，既可以是基本数据类型，也可以是引用数据类型。
> 数组，一旦初始化完成，其长度就确定了，并且其长度不可更改。
> 创建数组对象会在内存中开辟一整块'连续的空间'。占据的空间的大小，取决于数组的长度和数组中元素的类型。

4. 复习: 变量按照数据类型的分类:
4.1 基本数据类型: byte \ short \ int \ long; float \ double; char \ boolean
4.2 引用数据类型: 类、数组、接口、枚举、注解、记录。

5. 数组的分类
5.1 按照元素的类型: 基本数据类型元素的数组; 引用数据类型元素的数组。
5.2 按照数组的维数来分: 一维数组，二维数组...

6. 一维数组的使用(6个基本点)
> 数组的声明和初始化
> 调用数组的指定元素
> 数组的属性: length，表示数组的长度
> 数组的遍历
> 数组元素的默认初始化值
> 一维数组的内存解析(难)

7. 数组元素的默认初始化值的情况
注意: 以数组的动态初始化方式为例说明。

> 整型数组元素的默认初始化值: 0
> 浮点型数组元素的默认初始化值: 0.0
> 字符型数组元素的默认初始化值: 0 (或理解为'\u0000')
> boolean型数组元素的默认初始化值: false
> 引用数据类型数组元素的默认初始化值: null

8. 一维数组的内存解析
8.1 Java中的内存结构是如何划分的？(主要关心JVM的运行时内存环境)
> 将内存区域划分为5个部分: 程序计数器、虚拟机栈、本地方法栈、堆、方法区。

> 与目前数组相关的内存结构:     比如: int[] arr = new int[]{1, 2, 3};
    > 虚拟机栈: 用于存放方法中声明的局部变量。 比如: arr。
    > 堆: 用于存放数组的实体(即数组中的所有元素)。比如: 1, 2, 3。

8.2 举例: 具体一维数组的代码的内存解析
```

```java
package com.atguigu1.one;

public class OneArrayTest {
    public static void main(String[] args) {

        // 1. 数组的声明与初始化
        // 复习: 变量的定义格式: 数据类型 变量名 = 变量值;
        int num1 = 10;
        int num2; // 声明
        num2 = 20; // 初始化

        // 1.1 声明数组
        double[] prices;
        // 1.2 数组的初始化
        // 静态初始化: 数组变量的赋值与数组元素的赋值操作同时进行。
        prices = new double[]{20.32, 43.21, 43.22};

        // String[] foods;
        // foods = new String[]{"拌海蜇", "龙须菜", "炝冬笋", "玉兰片"};

        // 数组的声明和初始化
        // 动态初始化: 数组变量的赋值与数组元素的赋值操作分开进行。
        String[] foods = new String[4];

        // 其它正确的方式
        int arr[] = new int[4];
        int[] arr1 = {1, 2, 3, 4}; // 类型推断，这种写法必须写在一行。

        // 错误的方式
        // int[] arr2 = new int[3]{1, 2, 3};
        // int[3] arr3 = new int[];

        // 2. 数组元素的调用
        // 通过角标的方式，获取数组的元素
        // 角标的范围从0开始，到数组的长度-1结束。
        System.out.println(prices[0]); // 20.32
        System.out.println(prices[2]); // 43.22
        // System.out.println(prices[3]); // 报异常: ArrayIndexOutOfBoundsException

        foods[0] = "拌海蜇";
        foods[1] = "龙须菜";
        foods[2] = "炝冬笋";
        foods[3] = "玉兰片";
        // foods[4]="酱茄子"; // 报异常: ArrayIndexOutOfBoundsException

        // 3. 数组的长度: 用来描述数组容器中容量的大小
        // 使用length属性表示
        System.out.println(foods.length); // 4
        System.out.println(prices.length); // 3

        // 4. 如何遍历数组
        // System.out.println(foods[0]);
        // System.out.println(foods[1]);
        // System.out.println(foods[2]);
        // System.out.println(foods[3]);

        for (int i = 0; i < foods.length; i++) {
            System.out.println(foods[i]);
        }

        for (int i = 0; i < prices.length; i++) {
            System.out.println(prices[i]);
        }
    }
}
```

```java
package com.atguigu1.one;

/**
 * ClassName: OneArrayTest1
 * Package: com.atguigu1.one
 * Description: 一维数组的基本使用(承接OneArrayTest.java)
 *
 * @Author ljy
 * @Create 2026. 3. 23. 오후 11:44
 * @Version 1.0
 */
public class OneArrayTest1 {
    public static void main(String[] args) {
        // 5. 数组元素的默认初始化值
        // > 整型数组元素的默认初始化值: 0
        int[] arr1 = new int[3];
        System.out.println(arr1[0]);

        short[] arr2 = new short[4];
        for (int i = 0; i < arr2.length; i++) {
            System.out.println(arr2[i]);
        }

        // > 浮点型数组元素的默认初始化值:
        double[] arr3 = new double[5];
        System.out.println(arr3[0]);

        // > 字符型数组元素的默认初始化值: 0(或理解为: '\u0000')
        char[] arr4 = new char[4];
        System.out.println(arr4[0]);

        if (arr4[0] == 0) {
            System.out.println("111"); // 输出
        }

        if (arr4[0] == '0') {
            System.out.println("222");
        }

        if (arr4[0] == '\u0000') {
            System.out.println("333");
        }

        System.out.println(arr4[0] + 1);

        // > boolean型数组元素的默认初始化值: false
        boolean[] arr5 = new boolean[4];
        System.out.println(arr5[0]);

        // > 引用数据类型数组元素的默认初始化值: null
        String[] arr6 = new String[5];
        for (int i = 0; i < arr6.length; i++) {
            System.out.println(arr6[i]);
        }

        if (arr6[0] == null) {
            System.out.println("aaa");
        }

        if (arr6[0] == "null") {
            System.out.println("bbb");
        }

        // 6. 数组的内存解析
        int[] a1 = new int[]{1, 2, 3};
        int[] a2 = a1;
        a2[1] = 10;
        System.out.println(a1[1]); // 10
        System.out.println(a1); // [I@3fee733d
        System.out.println(a2); // [I@3fee733d
    }
}
```

![alt text](images/image11.png)

![alt text](images/image12.png)

![alt text](images/image13.png)

## 65 数组 一维数组的课后练习 1-3

```java
package com.atguigu1.one.exer1;

/**
 * ClassName: ArrayExer
 * Package: com.atguigu1.one.exer1
 * Description:
 * 案例: "破解"房东电话
 * 升景坊单间短期出租4个月，550元/月(水电煤公摊，网费35元/月)，空调、卫生间、厨房齐全。屋内均是IT行业人士，喜欢安静。
 * 所以要求来租者最好是同行或者刚毕业的年轻人，爱干净、安静。
 */
public class ArrayExer {
    public static void main(String[] args) {
        int[] arr = new int[]{8, 2, 1, 0, 3};
        int[] index = new int[]{2, 0, 3, 2, 4, 0, 1, 3, 2, 3, 3};

        String tel = "";

        for (int i = 0; i < index.length; i++) {
            int value = index[i];
            tel += arr[value];
        }
        System.out.println("联系方式: " + tel); // 联系方式: 18013820100
    }
}
```

```text
案例: 输出英文星期几。

用一个数组，保存星期一到星期天到7个英语单词，从键盘输入1-7，显示对应的单词。
{"Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday"}

拓展: 一年12个月的存储

用一个数组，保存12个月的英语单词，从键盘输入1-12，显示对应的单词。
{"January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"}
```

```java
package com.atguigu1.one.exer2;

import java.util.Scanner;

public class ArrayExer02 {
    public static void main(String[] args) {
        // 定义包含7个单词的数组。
        String[] weeks = new String[]{"Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday"};

        // 从键盘获取指定的数值，使用Scanner。
        Scanner scan = new Scanner(System.in);
        System.out.print("请输入数值(1-7): ");
        int day = scan.nextInt();

        // 针对获取的数据进行判断即可
        if (day < 1 || day > 7) {
            System.out.println("你输入的数据有误。");
        } else {
            System.out.println(weeks[day - 1]);
        }

        scan.close();
    }
}
```

```text
案例: 学生考试等级划分

从键盘读入学生成绩，找出最高分，并输出学生成绩等级。
    成绩 >= 最高分-10    等级为'A'
    成绩 >= 最高分-20    等级为'B'
    成绩 >= 最高分-30    等级为'C'
    其余                等级为'D'

提示: 先读入学生人数，根据人数创建int数组，存放学生成绩。
```

![alt text](./images/image14.png)

```java
package com.atguigu1.one.exer3;

import java.util.Scanner;

public class ArrayExer03 {
    public static void main(String[] args) {
        // 1. 从键盘输入学生的人数，根据人数，成绩数组。(动态初始化)
        Scanner scan = new Scanner(System.in);
        System.out.print("请学生学生人数: ");
        int count = scan.nextInt();

        int[] scores = new int[count];

        // 2. 根据提示，依次输入学生成绩，并将成绩保存在数组元素中。
        System.out.println("请输入" + count + "个成绩:");
        for (int i = 0; i < scores.length; i++) {
            scores[i] = scan.nextInt();
        }

        // 3. 获取学生成绩的最大值
        int maxScore = scores[0];
        for (int i = 1; i < scores.length; i++) {
            if (maxScore < scores[i]) {
                maxScore = scores[i];
            }
        }
        System.out.println("最高分是: " + maxScore);

        // 4. 遍历数组元素，根据学生成绩与最高分的差值，得到每个学生的等级，并输出成绩和等级。
        for (int i = 0; i < scores.length; i++) {
            if (scores[i] >= maxScore - 10) {
                System.out.println("student " + i + " score is " + scores[i] + " grade is A");
            } else if (scores[i] >= maxScore - 20) {
                System.out.println("student " + i + " score is " + scores[i] + " grade is B");
            } else if (scores[i] >= maxScore - 30) {
                System.out.println("student " + i + " score is " + scores[i] + " grade is C");
            } else {
                System.out.println("student " + i + " score is " + scores[i] + " grade is D");
            }
        }

        scan.close();
    }
}
```

- 对上面程序的优化

```java
package com.atguigu1.one.exer3;

import java.util.Scanner;

public class ArrayExer03_1 {
    public static void main(String[] args) {
        // 1. 从键盘输入学生的人数，根据人数，成绩数组。(动态初始化)
        Scanner scan = new Scanner(System.in);
        System.out.print("请学生学生人数: ");
        int count = scan.nextInt();

        int[] scores = new int[count];

        // 2. 根据提示，依次输入学生成绩，并将成绩保存在数组元素中。
        // 优化点1: 在获取输入的成绩的同时，得到最大值。
        int maxScore = scores[0];
        System.out.println("请输入" + count + "个成绩:");
        for (int i = 0; i < scores.length; i++) {
            scores[i] = scan.nextInt();
            // 3. 获取学生成绩的最大值
            if (maxScore < scores[i]) {
                maxScore = scores[i];
            }
        }

        System.out.println("最高分是: " + maxScore);

        // 4. 遍历数组元素，根据学生成绩与最高分的差值，得到每个学生的等级，并输出成绩和等级。
        // 优化点2: 去掉大量的重复代码。
        char grade;
        for (int i = 0; i < scores.length; i++) {
            if (scores[i] >= maxScore - 10) {
                grade = 'A';
            } else if (scores[i] >= maxScore - 20) {
                grade = 'B';
            } else if (scores[i] >= maxScore - 30) {
                grade = 'C';
            } else {
                grade = 'D';
            }
            System.out.println("student " + i + " score is " + scores[i] + " grade is " + grade);
        }

        scan.close();
    }
}
```
