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

## 45 流程控制 `switch case`的基本使用

```java
/*
分支结构之switch-case的使用

1. 语法格式

switch(表达式) {
    case 常量1:
        // 执行语句1
        break;
    case 常量2:
        // 执行语句2
        break;
    ...
    default:
        // 执行语句
        break;
}

2. 执行过程:
根据表达式中的值，依次匹配case语句。一旦与某一个case中的常量相等，那么就执行此case中的执行语句。
执行完此执行语句之后:
    情况1: 遇到break，则执行break后，跳出当前的switch-case结构。
    情况2: 没有遇到break，则继续执行其后的case中的执行语句。 --> case穿透
           ...
           直到遇到break或者执行完所有的case及default中的语句，退出当前的switch-case结构。

3. 说明:
  1️⃣  switch中的表达式只能是特定的数据类型。如下: byte \ short \ char \ int \ 枚举(JDK5.0新增) \ String(JDK57.0新增)
  2️⃣  case后都是跟的常量，使用表达式与这些常量做相等的判断，不能进行范围的判断。
  3️⃣  开发中，使用switch-case时，通常case匹配的情况都有限。
  4️⃣  break: 可以使用在switch-case中。一旦执行此break关键字，就跳出当前的switch-case结构。
  5️⃣  default: 类似于if-else中的else结构。
        default是可选的，而且位置是灵活的。

4. switch-case与if-else之间的转换
  1️⃣  开发中，凡是可以使用switch-case结构的场景，都可以改写为if-else。反之，不成立。
  2️⃣  开发中，如果一个具体的问题既可以使用switch-case，又可以使用if-else，推荐使用switch-case。
      为什么? switch-case相较于if-else效率稍高。
 */
class SwitchCaseTest {
    public static void main(String[] args) {

        int num = 1;

        switch(num) {
            case 0:
                System.out.println("zero");
                break;
            case 1:
                System.out.println("one");
                break; // 结束当前的switch-case结构
            case 2:
                System.out.println("two");
                break;
            case 3:
                System.out.println("three");
                break;
            default:
                System.out.println("other");
                break;
        }

        // 另例:
        String season = "summer";
        switch (season) {
            case "spring":
                System.out.println("春暖花开");
                break;
            case "summer":
                System.out.println("夏日炎炎");
                break;
            case "autumn":
                System.out.println("秋高气爽");
                break;
            case "winter":
                System.out.println("冬雪皑皑");
                break;
            default:
                System.out.println("季节输入有误");
                break;
        }

        // 错误的例子: 编译不通过
        /*
        int number = 20;

        switch(number) {
            case number > 0:
                System.out.println("正数");
                break;
            case number < 0:
                System.out.println("负数");
                break;
            default:
                System.out.println("零");
                break;
        }
        */
    }
}
```

## 46 流程控制 `switch case`的课后练习

```java
/*
案例3: 使用switch-case实现: 对学生成绩大于60分的，输出"合格"。低于60分的，输出"不合格"。
 */
class SwitchCaseTest1 {
    public static void main(String[] args) {

        // 定义一个学生成绩的变量
        int score = 78;

        // 根据需求，进行分支。
        // 方式1:
        /*
        switch(score) {
            case 0:
                System.out.println("不及格");
                break;
            case 1:
                System.out.println("不及格");
                break;
            // ...
            case 100:
                System.out.println("及格");
                break;
            default:
                System.out.println("成绩输入有误");
                break;
        }
        */
        // 方式2: 体会case穿透
        switch(score / 10) {
            case 0:
            case 1:
            case 2:
            case 3:
            case 4:
            case 5:
                System.out.println("不及格");
                break;
            case 6:
            case 7:
            case 8:
            case 9:
            case 10:
                System.out.println("及格");
                break;
            default:
                System.out.println("成绩输入有误");
                break;
        }
        // 方式3:
        switch(score / 60) {
            case 0:
                System.out.println("不及格");
                break;
            case 1:
                System.out.println("及格");
                break;
            default:
                System.out.println("成绩输入有误");
                break;
        }
    }
}
```

```java
/*
案例: 编写程序: 从键盘上输入2023年的"month"和"day“，要求通过程序输出输入的日期为2023年的第几天。
 */
import java.util.Scanner;

class SwitchCaseTest2 {
    public static void main(String[] args) {

        // 1. 使用Scanner，从键盘获取2023年的month和day
        Scanner input = new Scanner(System.in);

        System.out.println("请输入2023年的月份: ");
        int month = input.nextInt(); // 阻塞式方法
        System.out.println("请输入2023年的天: ");
        int day = input.nextInt(); // 阻塞式方法

        // 假设用户输入的数据是合法的。后期在开发中，使用正则表达式进行校验。
        // 2. 使用switch-case实现分支结构
        int sumDays = 0; // 记录总天数

        // 方式1: 不推荐，存在数据的冗余。
        /*
        switch(month) {
            case 1:
                sumDays = day;
                break;
            case 2:
                sumDays = 31 + day;
                break;
            case 3:
                sumDays = 31 + 28 + day;
                break;
            case 4:
                sumDays = 31 + 28 + 31 + day;
                break;
            // ...
            case 12:
                sumDays = 31 + 28 + 31 ... + 30 + day;
                break;
        }
        */
        // 方式2: 利用case的穿透特性
        switch(month) {
            case 12:
                sumDays += 30; // 30: 11月份的总天数
            case 11:
                sumDays += 31; // 31: 10月份的总天数
            case 10:
                sumDays += 30; // 30: 9月份的总天数
            case 9:
                sumDays += 31; // 31: 8月份的总天数
            case 8:
                sumDays += 31; // 31: 7月份的总天数
            case 7:
                sumDays += 30; // 30: 6月份的总天数
            case 6:
                sumDays += 31; // 31: 5月份的总天数
            case 5:
                sumDays += 30; // 30: 4月份的总天数
            case 4:
                sumDays += 31; // 31: 3月份的总天数
            case 3:
                sumDays += 28; // 28: 2月份的总天数
            case 2:
                sumDays += 31; // 31: 1月份的总天数
            case 1:
                sumDays += day;
                // break;
        }


        System.out.println("2023年" + month + "月" + day + "日是当前的第" + sumDays + "天");

        input.close(); // 为了防止内存泄漏
    }
}
```

## 47 流程控制 `for`循环结构的基本使用

```java
/*
循环结构之一: for循环。

1. Java中规范了3种循环结构: for、while、do-while。
2. 凡是循环结构，就一定会有4个要素:
  1️⃣  初始化条件
  2️⃣  循环条件 --> 一定是boolean类型的变量或表达式
  3️⃣  循环体
  4️⃣  迭代部分

3. for循环的格式

for (1️⃣ ; 2️⃣ ; 4️⃣ ) {
    3️⃣
}

执行过程: 1️⃣  - 2️⃣  - 3️⃣  - 4️⃣  - 2️⃣  - 3️⃣  - 4️⃣  - ... - 2️⃣
 */
class ForTest {
    public static void main(String[] args) {

        // 需求1: 题目: 输出5行HelloWorld。
        /*
        System.out.println("HelloWorld");
        System.out.println("HelloWorld");
        System.out.println("HelloWorld");
        System.out.println("HelloWorld");
        System.out.println("HelloWorld");
        */

        for(int i = 1; i <= 5; i++) {
            System.out.println("HelloWorld");
        }

        // 此时编译不通过。因为i已经出了其作用域范围。
        // System.out.println(i);

        // 需求2:
        int num = 1;
        for(System.out.print("a"); num < 3; System.out.print("c"), num++) {
            System.out.print("b");
        }
        // 输出结果: abcbc
        System.out.println(); // 换行

        // 需求3: 遍历1-100以内的偶数，并获取偶数的个数，获取所有的偶数的和。
        int count = 0;  // 记录偶数的个数
        int sum = 0; // 记录所有偶数的和

        for(int i = 1; i <= 100; i++) {
            if(i % 2 == 0) {
                System.out.println(i);
                count++;
                sum += i; // sum = sum + i;
            }
        }

        System.out.println("偶数的个数为: " + count);
        System.out.println("偶数的总和为: " + sum);
    }
}
```
