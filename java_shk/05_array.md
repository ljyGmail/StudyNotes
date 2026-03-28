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

## 66 数组 二维数组的初始化、遍历与元素默认初始化值

## 67 数组 二维数组的内存解析与课后练习1 2 3

```text
1. 二维数组的理解
- 对于二维数组的理解，可以看成是一维数组array1又作为另一个一维数组array2的元素而存在。
- 其实，从数组底层的运行机制来看，其实没有多维数组。
- 概念: 数组的外层元素; 数组的内层元素。


2. 二维数组的使用(6个基本点)
> 数组的声明和初始化
> 调用数组的指定元素
> 数组的属性: length，表示数组的长度
> 数组的遍历
> 数组元素的默认初始化值
> 二维数组的内存解析(难)


3. 二维数组元素的默认初始化值
3.1 动态初始化方式1: (比如: int[][] arr = new int[3][4])

1) 外层元素: 默认存储地址值。
2) 内存元素: 默认与一维数组元素的不同类型的默认值规定相同。
    > 整型数组元素的默认初始化值: 0
    > 浮点型数组元素的默认初始化值: 0.0
    > 字符型数组元素的默认初始化值: 0 (或理解为'\u0000')
    > boolean型数组元素的默认初始化值: false
    > 引用数据类型数组元素的默认初始化值: null

3.2 动态初始化方式2: (比如: int[][] arr = new int[3][])
1) 外层元素: 默认存储null。
2) 内存元素: 不存在的。如果调用会报错 (NullPointerException)
```

```java
package com.atguigu2.two;
/**
 * ClassName: TwoArrayTest
 * Package: com.atguigu1.one
 * Description:
 * 二维数组的基本使用(难点)
 *
 * @Author ljy
 * @Create 2026. 3. 28. 오후 8:57
 * @Version 1.0
 */
public class TwoArrayTest {
    public static void main(String[] args) {
        // 1. 数组的声明与初始化
        // 复习
        int[] arr1 = new int[]{1, 2, 3};

        // 方式1: 静态初始化: 数组变量的赋值和数组元素的赋值同时进行。
        int[][] arr2 = new int[][]{{1, 2, 3}, {4, 5}, {6, 7, 8, 9}};

        // 方式2: 动态初始化1: 数组变量的赋值和数组元素的赋值分开进行。
        String[][] arr3 = new String[3][4];
        // 方式2: 动态初始化2
        double[][] arr4 = new double[2][];

        // 其它正确的写法:
        int arr5[][] = new int[][]{{1, 2, 3}, {4, 5}, {6, 7, 8, 9}};
        int[] arr6[] = new int[][]{{1, 2, 3}, {4, 5}, {6, 7, 8, 9}};
        int arr7[][] = {{1, 2, 3}, {4, 5}, {6, 7, 8, 9}};
        String arr8[][] = new String[3][4];

        // 错误的写法:
        // int[][] arr9 = new int[3][3]{{1, 2, 3}, {4, 5, 6}, {7, 8, 9}};
        // int[3][3] arr10 = new int[][]{{1, 2, 3}, {4, 5, 6}, {7, 8, 9}};
        // int[][] arr11 = new int[][10];

        // 2. 数组元素的调用
        // 针对于arr2来说，外层元素{1, 2, 3}, {4, 5}, {6, 7, 8, 9}; 内层元素: 1, 2, 3, 4, 5, 6, 7, 8, 9。
        // 调用内层元素
        System.out.println(arr2[0][0]); // 1
        System.out.println(arr2[2][1]); // 7

        // 调用外层元素
        System.out.println(arr2[0]); // [I@3fee733d

        // 测试arr3和arr4
        arr3[0][1] = "Tom";
        System.out.println(arr3[0][1]); // Tom
        System.out.println(arr3[0]); // [Ljava.lang.String;@5acf9800

        arr4[0] = new double[4];
        arr4[0][0] = 1.0;

        // 3. 数组的长度
        System.out.println(arr2.length); // 3
        System.out.println(arr2[0].length); // 3
        System.out.println(arr2[1].length); // 2
        System.out.println(arr2[2].length); // 4

        // 4. 如何遍历数组
        for (int i = 0; i < arr2.length; i++) {
            for (int j = 0; j < arr2[i].length; j++) {
                System.out.print(arr2[i][j] + "\t");
            }
            System.out.println();
        }
    }
}
```

```java
package com.atguigu2.two;
/**
 * ClassName: TwoArrayTest1
 * Package: com.atguigu2.two
 * Description:
 * 二维数组的基本使用(难点) (承接TwoArrayTest.java)
 *
 * @Author ljy
 * @Create 2026. 3. 28. 오후 9:53
 * @Version 1.0
 */
public class TwoArrayTest1 {
    public static void main(String[] args) {

        // 5. 数组元素的默认初始化值
        // 以动态初始化方式1说明:
        int[][] arr1 = new int[3][2];
        // 外层元素默认值:
        System.out.println(arr1[0]); // 地址值 [I@3fee733d
        System.out.println(arr1[1]); // 地址值 [I@5acf9800
        // 内层元素默认值:
        System.out.println(arr1[0][0]); // 0

        boolean[][] arr2 = new boolean[3][4];
        // 外层元素默认值:
        System.out.println(arr2[0]); // 地址值 [Z@4617c264
        // 内层元素默认值:
        System.out.println(arr2[0][1]); // false

        String[][] arr3 = new String[4][2];
        // 外层元素默认值:
        System.out.println(arr3[0]); // 地址值 [Ljava.lang.String;@36baf30c
        // 内层元素默认值:
        System.out.println(arr3[0][1]); // null

        // ******************************
        // 以动态初始化方式2说明:
        int[][] arr4 = new int[4][];
        // 外层元素默认值:
        System.out.println(arr4[0]); // null
        // 内层元素默认值:
        // System.out.println(arr4[0][0]); // 报错: 空指针异常

        String[][] arr5=new String[5][];
        // 外层元素默认值:
        System.out.println(arr5[0]); // null
        // 内层元素默认值:
        // System.out.println(arr5[0][0]); // 报错: 空指针异常

        // 6. 数组的内存解析
    }
}
```

![alt text](images/二维数组的内存解析1.png)

![alt text](images/二维数组的内存解析2.png)

- 练习1

```text
案例1: 获取arr数组中所有元素的和。

提示: 使用for的嵌套循环即可。
```

![alt text](image.png)

```java
package com.atguigu2.two.exer1;

public class ArrayExer01 {
    public static void main(String[] args) {
        // 初始化数组: 静态初始化
        int[][] arr = new int[][]{{3, 5, 8}, {12, 9}, {7, 0, 6, 4}};

        /* 这种情况下，动态初始化没有静态初始化方便:
        int[][] arr = new int[3][];
        arr[0] = new int[]{3, 5, 8};
        arr[1] = new int[]{12, 9};
        arr[2] = new int[]{7, 0, 6, 4};
         */

        int sum = 0; // 记录元素的总和

        for (int i = 0; i < arr.length; i++) {
            for (int j = 0; j < arr[i].length; j++) {
                sum += arr[i][j];
            }
        }

        System.out.println("总和为: " + sum);
    }
}
```

- 练习2

```text
案例: 声明 int[] x,y[]; 在给x, y变量赋值以后，以下选项允许通过编译的是: x: 一维int[], y: 二维int[][]。

a)   x[0] = y;              no
b)   y[0] = x;              yes
c)   y[0][0] = x;           no
d)   x[0][0] = y;           no
e)   y[0][0] = x[0];        yes
f)   x = y;                 no

提示:
一维数组: int[] x 或者 int x[]。
二维数组: int[][] y 或者 int[] y[] 或者 int y[][]。
```

```java
package com.atguigu2.two.exer2;

public class ArryExer02 {
    public static void main(String[] args) {
        // =: 赋值符号。
        int i = 10;
        int j = i;
        byte b = (byte) i;// 强制类型转换

        long l = i; // 自动类型提升


        // 举例: 数组
        int[] arr1 = new int[10];
        byte[] arr2 = new byte[10];
        // arr1 = arr2; // 编译不通过。原因: int[]和byte[]是两种不同类型的引用变量。

        System.out.println(arr1); // [I@3fee733d
        System.out.println(arr2); // [B@5acf9800

        int[][] arr3 = new int[3][2];
        // arr3 = arr1; // 编译不通过。

        arr3[0] = arr1;
        System.out.println(arr3[0]); // [I@3fee733d
        System.out.println(arr1); // [I@3fee733d

        System.out.println(arr3); // [[I@4617c264
    }
}
```

- 练习3

```text
案例：二维数组存储数据，并遍历

String[][] employees = {
    {"10", "1", "段 誉", "22", "3000"},
    {"13", "2", "令狐冲", "32", "18000", "15000", "2000"},
    {"11", "3", "任我行", "23", "7000"},
    {"11", "4", "张三丰", "24", "7300"},
    {"12", "5", "周芷若", "28", "10000", "5000"},
    {"11", "6", "赵 敏", "22", "6800"},
    {"12", "7", "张无忌", "29", "10800","5200"},
    {"13", "8", "韦小宝", "30", "19800", "15000", "2500"},
    {"12", "9", "杨 过", "26", "9800", "5500"},
    {"11", "10", "小龙女", "21", "6600"},
    {"11", "11", "郭 靖", "25", "7100"},
    {"12", "12", "黄 蓉", "27", "9600", "4800"}
};

其中"10"代表普通职员，"11"代表程序员，"12"代表设计师，"13"代表架构师。显示效果如图。
```

![alt text](员工列表显示.png)

```java
package com.atguigu2.two.exer3;

public class ArrayExer03 {
    public static void main(String[] args) {
        // 定义二维employees数组:
        String[][] employees = {
                {"10", "1", "段 誉", "22", "3000"},
                {"13", "2", "令狐冲", "32", "18000", "15000", "2000"},
                {"11", "3", "任我行", "23", "7000"},
                {"11", "4", "张三丰", "24", "7300"},
                {"12", "5", "周芷若", "28", "10000", "5000"},
                {"11", "6", "赵 敏", "22", "6800"},
                {"12", "7", "张无忌", "29", "10800", "5200"},
                {"13", "8", "韦小宝", "30", "19800", "15000", "2500"},
                {"12", "9", "杨 过", "26", "9800", "5500"},
                {"11", "10", "小龙女", "21", "6600"},
                {"11", "11", "郭 靖", "25", "7100"},
                {"12", "12", "黄 蓉", "27", "9600", "4800"}
        };

        System.out.println("员工类型\t编号\t姓名\t\t年龄\t薪资\t\t奖金\t\t股票");

        for (int i = 0; i < employees.length; i++) {
            String employeeType = employees[i][0];
            switch (employeeType){
                // "10"代表普通职员，"11"代表程序员，"12"代表设计师，"13"代表架构师。显示效果如图。
                case "10":
                    System.out.print("普通职员\t");
                    break;
                case "11":
                    System.out.print("程序员\t");
                    break;
                case "12":
                    System.out.print("设计师\t");
                    break;
                case "13":
                    System.out.print("架构师\t");
                    break;
            }
            for (int j = 1; j < employees[i].length; j++) {
                System.out.print(employees[i][j] + "\t");
            }
            System.out.println();
        }
    }
}
```
