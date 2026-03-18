# 第03章 流程控制语句

## 41 流程控制 `if else`结构的基本使用

```java
/*
分支结构1: if-else条件判断结构

1. 格式

格式1:
if (条件表达式) {
    语句块;
}

格式2: "二选一"
if (条件表达式) {
    语句块1;
} else {
    语句块2;
}

格式3: "多选一"
if (条件表达式1) {
    语句块1;
} else if (条件表达式2) {
    语句块2;
}
...
} else if (条件表达式n) {
    语句块n;
} else {
    语句块n+1;
}
 */
class IfElseTest {
    public static void main(String[] args) {
        /*
        案例1: 成年人心率的正常范围是每分钟60 - 100次。体检时，
        如果心率不在此范围内，则提示需要做进一步的检查。
         */
        int heartBeats = 89;

        // if (60 <= heartBeats <= 100) 错误的写法

        if (heartBeats < 60 || heartBeats > 100) {
            System.out.println("你需要做进一步的检查");
        }

        System.out.println("体检结束"); // 输出

        // ******************************
        /*
        案例2: 定义一个整数，判定是偶数奇数。
         */
        int num = 13;
        if(num % 2 == 0) {
            System.out.println(num + "是偶数");
        } else {
            System.out.println(num + "是奇数"); // 13是奇数
        };
    }
}
```

```java
/*
岳小鹏参加Java考试，他和父亲岳不群达成承诺:
如果:
成绩为100分时，奖励一辆跑车;
成绩为(80 ,99]分时，奖励一辆山地自行车;
成绩为[60 ,80]分时，奖励环球影城一日游;
其它时，胖揍一顿。

说明: 默认成绩是在[0, 100]范围内。

结论:
1. 如果多个条件表达式之间没有交集(互斥关系)，则哪个条件表达式声明在上面，哪个声明在下面都可以。
   如果多个条件表达式之间是包含关系，则需要将范围小的条件表达式声明在范围大的条件表达式的上面。
   否则，范围小的条件表达式不可能被执行。
 */

class IfElseTest1 {
    public static void main(String[] args) {

        int score = 61;

        // 方式1:
        /*
        if(score == 100) {
            System.out.println("奖励一辆跑车");
        } else if (score > 80 && score <= 99) {
            System.out.println("奖励一辆山地自行车");
        } else if (score >= 60 && score <= 80) {
            System.out.println("奖励环球影城一日游");
        } else {
            System.out.println("胖揍一顿");
        }
        */

        // 方式2:
        score = 88;
        if(score == 100) {
            System.out.println("奖励一辆跑车");
        } else if (score > 80) {
            System.out.println("奖励一辆山地自行车");
        } else if (score >= 60) {
            System.out.println("奖励环球影城一日游");
        } else {
            System.out.println("胖揍一顿");
        }

        // 特别的:
        if(score == 100) {
            System.out.println("奖励一辆跑车");
        } else if (score > 80) {
            System.out.println("奖励一辆山地自行车");
        } else if (score >= 60) {
            System.out.println("奖励环球影城一日游");
        }
        /* else {
            System.out.println("胖揍一顿");
        }
        */
    }
}
```

## 42 流程控制 `if else`结构的嵌套使用及课后练习

```java
/*
测试if-else的嵌套使用

案例:
由键盘输入三个整数分别存入变量num1、num2、num3，对它们进行排序(使用if-else if-else)，并且从小到大输出。

拓展: 你能实现从大到小顺序的排列吗?

1. 从开发经验上讲，没有写过超过三层嵌套的if-else结构。
2. 如果if-else中的执行语句块中只有一行执行语句，则此执行语句所在的一对{}可以省略。但是，不建议省略。
 */
class IfElseTest2 {
    public static void main(String[] args) {

        int num1 = 30;
        int num2 = 21;
        int num3 = 44;

        // int num1 = 30, num2 = 21, num3 = 44;

        if(num1 >= num2) { // => num2  num1
            if(num3 >= num1) {
                System.out.println(num2 + ", " + num1 + " ," + num3); // 输出
            } else if(num3 <= num2) {
                System.out.println(num3 + ", " + num2 + " ," + num1);
            } else {
                System.out.println(num2 + ", " + num3 + " ," + num1);
            }
        } else { // num1 < num2 => num1  num2
            if(num3 >= num2) {
                System.out.println(num1 + ", " + num2 + ", " + num3);
            } else if(num3 <= num1) {
                System.out.println(num3 + ", " + num1 + ", " + num2);
            } else {
                System.out.println(num1 + ", " + num3 + ", " + num2);
            }
        }

        // 拓展: 从大到小排列
        if (num1 >= num2) { // => num1  num2
            if(num3 >= num1) {
                System.out.println(num3 + ", " + num1 + ", " + num2); // 输出
            } else if(num3 <= num2) {
                System.out.println(num1 + ", " + num2 + ", " + num3);
            } else {
                System.out.println(num1 + ", " + num3 + ", " + num2);
            }
        } else { // num1 < num2 => num2  num1
            if(num3 >= num2) {
                System.out.println(num3 + ", " + num2 + ", " + num1);
            } else if(num3 <= num1) {
                System.out.println(num2 + ", " + num1 + ", " + num3);
            } else {
                System.out.println(num2 + ", " + num3 + ", " + num1);
            }
        }
    }
}
```

## 43 流程控制 使用`Scanner`类从键盘获取数据

```java
/*
如何从键盘获取不同类型(基本数据类型、String类型)的变量: 使用Scanner类。

1. 使用Scanner获取不同类型数据的步骤:
  步骤1: 导包 import java.util.Scanner;
  步骤2: 提供(或创建)一个Scanner类的实例。
  步骤3: 调用Scanner类中的方法，获取指定类型的变量(nextXXX())。
  步骤4: 关闭资源，调用Scanner类的close()方法。

2. 案例: 小明注册某交友网站，要求录入个人相关信息。如下:
请输入你的网名、你的年龄、你的体重、你是否单身、你的性别等情况。

3. Scanner类中提供了获取byte \ short \ int \ long \ float \ double \ boolean \ String类型变量的方法。
   注意: 没有提供获取char类型变量的方法。需要使用next().charAt(0)。
 */

// 步骤1: 导包 import java.util.Scanner;
import java.util.Scanner;

class ScannerTest {
    public static void main(String[] args) {

        // 步骤2: 提供(或创建)一个Scanner类的实例。
        Scanner scan = new Scanner(System.in);

        System.out.println("欢迎光临你来我往交友网");
        System.out.print("请输入你的网名: ");
        // 步骤3: 调用Scanner类中的方法，获取指定类型的变量。
        String name = scan.next();

        System.out.print("请输入你的年龄: ");
        int age = scan.nextInt();

        System.out.print("请输入你的体重: ");
        double weight = scan.nextDouble();

        System.out.print("你是否单身(单身: true; 不单身: false): ");
        boolean isSingle = scan.nextBoolean();

        System.out.print("请输入你的性别(男\\女): ");
        char gender = scan.next().charAt(0);

        System.out.println("网名: " + name + ", 年龄: " + age + ", 体重: " + weight + ", 是否单身: "
                + isSingle + ", 性别: " + gender);

        System.out.println("注册完成，欢迎继续进入体验!");

        // 步骤4: 关闭资源，调用Scanner类的close()方法。
        scan.close();
    }
}
```

```java
/*
大家都知道，但大当婚，女大当嫁。那么女方家长要嫁女儿，当然要提出一定的条件:
  高: 180cm以上;
  富: 财富1千万以上;
  帅: 是。

如果这三个条件同时满足，则"我一定要嫁给他!!!";
如果三个条件有为真的情况，则"嫁吧，比上不足，比下有余。";
如果三个条件都不满足，则: "不嫁!"。
 */

import java.util.Scanner;

class ScannerExer {
    public static void main(String[] args) {

        Scanner scan = new Scanner(System.in);

        System.out.print("请输入你的身高(cm): ");
        int height = scan.nextInt();

        System.out.print("请输入你的财富(以千万为单位): ");
        double wealth  = scan.nextDouble();

        // 关于是否帅的问题，使用String类型接收
        System.out.print("帅否(是/否): ");
        String isHandsome = scan.next();

        // 判断
        if (height >= 180 && wealth >= 1.0 && isHandsome.equals("是")) { // 知识点: 判断两个字符串是否相等，使用String的equals()
            System.out.println("我一定要嫁给他!!!");
        } else if(height >= 180 || wealth >= 1.0 || isHandsome.equals("是")) {
            System.out.println("嫁吧，比上不足，比下有余。");
        } else {
            System.out.println("不嫁!");
        }

        // 关闭资源
        scan.close();
    }
}
```

## 44 流程控制 如何获取一个随机数

```java
/*
如何获取一个随机数?

1. 可以使用Java提供的API: Math类的random()。
2. random()调用以后，会返回一个[0.0, 1.0)范围的double型的随机数。
3. 需求1: 获取一个[0,100]范围的随机整数?
   需求2: 获取一个[1,100]范围的随机整数?

4. 需求: 获取一个[a, b]范围的随机整数?
    (int) (Math.random() * (b - a + 1)) + a
 */
class RandomTest {
    public static void main(String[] args) {

        double d1 = Math.random();

        System.out.println("d1 = " + d1);

        int num1 = (int) (Math.random() * 101); // [0.0, 1.0) -> [0.0, 101.0) -> [0, 100]
        System.out.println("num1 = " + num1);

        int num2 = (int) (Math.random() * 100) + 1;
        System.out.println("num2 = " + num2); // [0.0, 1.0) -> [0.0, 100.0) -> [0, 99] + 1 -> [1, 100]
    }
}
```
