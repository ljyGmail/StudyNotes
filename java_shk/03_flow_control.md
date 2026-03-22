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

## 48 流程控制 `for`循环结构的课后练习

```java
/*
题目: 输出所有的水仙花数，所谓水仙花数是指一个3位数，其各个位上数字的立方和等于其本身。
例如: 153 = 1*1*1 + 5*5*5 + 3*3*3;
 */
class ForTest1 {
    public static void main(String[] args) {

        // 遍历所有的3位数
        for(int i = 100; i <= 999; i++) {
            // 针对于每一个3位数i，获取其各个位上的数值。
            int ge = i % 10;
            int shi = i / 10 % 10; // 或 int shi = i % 100 / 10;
            int bai = i / 100;
            // System.out.println(i + ": " + bai + ", " + shi + ", " + ge);

            // 判断是否满足水仙花数的规则
            if(i == ge * ge * ge + shi * shi * shi + bai * bai * bai) {
                System.out.println(i);
            }
        }
    }
}
```

```java
/*
案例: 输入两个正整数m和n，求其最大公约数和最小公倍数。

比如: 12和20的最大公约数是4，最小公倍数是60。

约数: 以12为例，约数有1, 2, 3, 4, 6, 12。
      以20为例，约数有1, 2, 4, 5, 10, 20。

倍数: 以12为例，倍数有12, 24, 36, 48, 60, 72...
      以20为例，倍数有20, 40, 60, 80, 100...

说明:
1. 我们可以在循环结构中使用break。一旦执行break，就跳出(或结束)当前循环结构。
2. 如何结束一个循环结构?
    方式1: 循环条件不满足。(即循环条件执行完以后是false)
    方式2: 在循环体中执行了break。
 */
class ForTest2 {
    public static void main(String[] args) {

        int m = 12;
        int n = 20;

        // 获取m和n中的较小值
        int min = (m < n) ? m : n;

        // 需求1: 最大公约数
        // 方式1:
        int result = 1;
        for(int i = 1; i <= min; i++) {
            if (m % i == 0 && n % i == 0) {
                // System.out.println(i);
                result = i;
            }
        }

        System.out.println(result);

        // 方式2: 推荐
        for(int i = min; i >= 1; i--) {
            if (m % i == 0 && n % i == 0) {
                System.out.println("最大公约数为: " + i);
                break; // 一旦执行，就跳出当前的循环结构
            }
        }

        // 需求2: 最小公倍数
        int max = (m > n) ? m : n;
        for(int i = max; i <= m * n; i++) {
            if(i % m == 0 && i % n == 0) {
                System.out.println("最小公倍数为: " + i);
                break;
            }
        }
    }
}
```

## 49 流程控制 `while`循环的使用及课后练习

```java
/*
随机生成一个100以内的数，猜这个随机数是多少?

从键盘输入数，如果大了，提示大了; 如果小了，提示小了。如果对了，就不在猜了，并统计一共猜了多少次。

提示: 生成一个[a, b]范围的随机数的方式: (int) (Math.random() * (b - a + 1)) + a。
 */
import java.util.Scanner;

class WhileTest1 {
    public static void main(String[] args) {

        // 1. 生成一个[1, 100]范围的随机整数。
        int target = (int) (Math.random() * 100) + 1;
        // System.out.println("target: " + target);

        // 2. 使用Scanner，从键盘获取数据。
        Scanner scan = new Scanner(System.in);
        System.out.print("请输入1~100范围的一个整数: ");
        int guess = scan.nextInt();

        // 3. 声明一个变量，记录猜的次数。
        int guessCount = 1;

        // 3. 使用循环结构，进行多次循环的对比和获取数据。
        while(target != guess) {
            if(guess > target) {
                System.out.println("你输入的数据大了");
            } else if(guess < target) {
                System.out.println("你输入的数据小了");
            }

            System.out.print("请输入1~100范围的一个整数: ");
            guess = scan.nextInt();
            guessCount++;
        }

        // 能结束循环，就意味着target和guess相等了。
        System.out.println("恭喜你!猜对了!");
        System.out.println("共猜了" + guessCount + "次");

        scan.close();
    }
}
```

```java
/*
世界最高山峰是珠穆朗玛峰，它的高度是8848.86米，假如我们有一张足够大的纸，他的厚度是0.1毫米。
请问，我折叠多少次，可以折成珠穆朗玛峰的高度?
 */
class whiletest2 {
    public static void main(string[] args) {

        // 1. 声明珠峰的高度、纸的默认高度。
        double paper = 0.1; // 单位: 毫米
        double zf = 8848860; // 单位: 毫米

        // 2. 定义一个变量，记录折纸的次数
        int count = 0;

        // 3. 通过循环结构，不断调整纸的厚度。(当纸的厚度超过珠峰高度时，停止循环)
        while(paper <= zf) {
            paper *= 2;
            count++;
        }

        system.out.println("paper的高度为: " + (paper / 1000) + ", 超过了珠峰的高度: " + (zf / 1000));
        system.out.println("共折纸" + count + "次");
    }
}
```

## 50 流程控制 `do while`循环的使用及课后练习

```java
/*
循环结构之一: do-while循环。

1. 凡是循环结构，就一定会有4个要素:
  1️⃣  初始化条件
  2️⃣  循环条件 --> 一定是boolean类型的变量或表达式
  3️⃣  循环体
  4️⃣  迭代部分

2. do-while循环的格式

1️⃣
do {
    3️⃣
    4️⃣
} while(2️⃣ );

执行过程: 1️⃣  -  3️⃣  - 4️⃣  - 2️⃣  - 3️⃣  - 4️⃣  - ... - 2️⃣

3. 说明:
  1️⃣  do-while循环至少执行一次循环体。
  2️⃣  for、while、do-while循环三者之间是可以相互转换的。
  3️⃣  do-while循环结构，在开发中，相较于for、while循环来讲，使用的较少。
 */
class DoWhileTest {
    public static void main(String[] args) {

        // 需求: 遍历100以内的偶数，并输出偶数的个数和总和。
        int i = 1;
        int count = 0; // 记录偶数的个数
        int sum = 0; // 记录偶数的总和

        do {
            if(i % 2 ==0) {
                System.out.println(i);
                count++;
                sum += i;
            }
            i++;
        } while(i <= 100);

        System.out.println("偶数的个数为: " + count);
        System.out.println("偶数的总和为: " + sum);

        // ******************************
        int num1 = 10;
        while(num1 > 10) {
            System.out.println("while:hello");
            num1--;
        }

        int num2 = 10;
        do {
            System.out.println("do-while:hello");
            num2--;
        } while(num2 > 10);
    }
}
```

```java
/*
题目: 模拟ATM取款。

声明变量balance并初始化为0，用以表示银行账户的余额，下面通过ATM机程序实现存款，取款等功能。

========ATM========
   1、存款
   2、取款
   3、显示余额
   4、退出

请选择(1-4):

 */
import java.util.Scanner;

class DoWhileTest1 {
    public static void main(String[] args) {

        // 1. 定义balance变量，记录账户余额。
        double balance = 0;

        boolean flag = true; // 控制循环的结束

        Scanner scan = new Scanner(System.in); // 实例化Scanner

        do {
            // 2. 声明ATM取款的界面
            System.out.println("========ATM========");
            System.out.println("   1、存款");
            System.out.println("   2、取款");
            System.out.println("   3、显示余额");
            System.out.println("   4、退出");
            System.out.print("请选择(1-4): ");

            // 3. 使用Scanner获取用户的选择
            int selection = scan.nextInt();

            // 4. 根据用户的选择，决定执行存款、取款、显示余额、退出的操作
            switch(selection) {
                case 1:
                    System.out.print("请输入存款的金额: ");
                    double money1 = scan.nextDouble();
                    if(money1 > 0) {
                        balance += money1;
                    }
                    break;
                case 2:
                    System.out.print("请输入取款的金额: ");
                    double money2 = scan.nextDouble();

                    if(money2 > 0 && money2 <= balance) {
                        balance -= money2;
                    } else {
                        System.out.println("输入的数据有误或余额不足!");
                    }
                    break;
                case 3:
                    System.out.println("账户余额为: " + balance);
                    break;
                case 4:
                    flag = false;
                    System.out.println("感谢使用，欢迎下次光临^_^");
                    break;
                default:
                    System.out.println("输入有误，请重新输入。");
                    // break;
            }
        } while(flag);

        // 关闭资源
        scan.close();
    }
}
```

## 51 流程控制 无限循环结构的使用

```java
/*
"无限"循环结构的使用

1. 格式: while(true)  或  for(;;)

2. 开发中，有时并不确定需要循环多少次，需要根据循环体内部的某些条件，来控制循环的结束(使用break)。

3 如果此循环结构不能终止，则构成了死循环! 开发中要避免出现死循环。
 */
class forwhiletest {
    public static void main(string[] args) {

        /*
        for(;;) { // while(true) {
            system.out.println("i love you!");
        }
        */

        // 死循环的后面不能有执行语句
        // System.out.println("end");
    }
}
```

```java
/*
案例: 从键盘读入个数不确定的整数，并判断读入的整数和负数的个数，输入为0时结束程序。
 */
import java.util.Scanner;

class ForWhileTest1 {
    public static void main(String[] args) {

        Scanner scan = new Scanner(System.in);

        int positiveCount = 0; // 记录正数的个数
        int negativeCount = 0; // 记录负数的个数

        for(;;) {// while(true) {
            System.out.print("请输入一个整数(输入为0时结束程序): ");
            int num = scan.nextInt(); // 获取用户输入的整数

            if(num > 0) { // 正数
                positiveCount++;
            } else if(num < 0) { // 负数
                negativeCount++;
            } else { // 零
                System.out.println("程序结束!");
                break;
            }
        }

        System.out.println("正数的个数为: " + positiveCount);
        System.out.println("负数的个数为: " + negativeCount);

        scan.close();
    }
}
```

## 52 流程控制 嵌套循环的使用

## 53 流程控制 使用嵌套for循环显示菱形、九九乘法表

```java
/*
嵌套循环的使用

1. 嵌套循环: 是指一个循环结构A的循环体是另一个循环结构B。
  - 外层循环: 循环结构A
  - 内层循环: 循环结构B

2. 说明:
  1️⃣ ) 内层循环充当了外层循环的循环体。
  2️⃣ ) 对于两层嵌套循环来说，外层循环控制行数，内层循环控制列数。
  3️⃣ ) 举例: 外层循环执行m次，内层循环执行n次，则内层循环的循环体共执行m * n次。
  4️⃣ )  实际开发中，我们不会出现三层以上的循环结构，三层的循环结构都很少见。
 */
class ForForTest {
    public static void main(String[] args) {

        // ******
        for(int i = 1; i <= 6; i++) {
            System.out.print('*');
        }

        System.out.println("\n##############################");

        /*
         ******
         ******
         ******
         ******
         ******
         */
        for(int j = 1; j <= 5; j++) {
            for(int i = 1; i <= 6; i++) {
                System.out.print('*');
            }

            System.out.println();
        }

        System.out.println("##############################");
        /*
                            i(第几行)              j(每一行中*的个数)
         *                  1                      1
         **                 2                      2
         ***                3                      3
         ****               4                      4
         *****              5                      5
         */
        for(int i = 1; i <= 5; i++) {
            for(int j = 1; j <= i; j++) {
                System.out.print('*');
            }
            System.out.println();
        }

        System.out.println("##############################");
        /*
                            i(第几行)              j(每一行中*的个数)
         ******             1                      6
         *****              2                      5
         ****               3                      4
         ***                4                      3
         **                 5                      2
         *                  6                      1

         */
        for(int i = 1; i <= 6; i++) {
            for(int j = 1; j <= 7 - i; j++) {
                System.out.print('*');
            }
            System.out.println();
        }

        /*
                            i(第几行)         j(每一行中-的个数)         k(每一行中*的个数)
--------*                   1                 8                          1
------* * *                 2                 6                          3
----* * * * *               3                 4                          5
--* * * * * * *             4                 2                          7
* * * * * * * * *           5                 0                          9

                            i(第几行)         j(每一行中-的个数)         k(每一行中*的个数)
--* * * * * * *             1                 2                          7
----* * * * *               2                 4                          5
------* * *                 3                 6                          3
--------*                   4                 8                          1

        */
        // 上半部分
        for(int i = 1; i <= 5; i++) {
            // -
            for(int j = 1; j <= 10 - 2 * i; j++) {
                System.out.print(" ");
            }
            // *
            for(int k = 1; k <= 2 * i - 1; k++) {
                System.out.print("* ");
            }
            System.out.println();
        }

        // 下半部分
        for(int i = 1; i <= 4; i++) {
            for(int j = 1; j <= 2 * i; j++) {
                System.out.print(" ");
            }

            for(int k = 1; k <= 9 - 2 * i; k++) {
                System.out.print("* ");
            }
            System.out.println();
        }
    }
}
```

```java
/*
练习: 九九乘法表
1*1=1
2*1=2	2*2=4
3*1=3	3*2=6	3*3=9
4*1=4	4*2=8	4*3=12	4*4=16
5*1=5	5*2=10	5*3=15	5*4=20	5*5=25
6*1=6	6*2=12	6*3=18	6*4=24	6*5=30	6*6=36
7*1=7	7*2=14	7*3=21	7*4=28	7*5=35	7*6=42	7*7=49
8*1=8	8*2=16	8*3=24	8*4=32	8*5=40	8*6=48	8*7=56	8*8=64
9*1=9	9*2=18	9*3=27	9*4=36	9*5=45	9*6=54	9*7=63	9*8=72	9*9=81
 */
class NineNineTable {
    public static void main(String[] args) {

        for(int i = 1; i <= 9; i++) {
            for(int j = 1; j <= i; j++) {
                System.out.print(i + "*" + j + "=" + (i * j) + "\t");
            }
            System.out.println();
        }
    }
}
```

## 54 流程控制 关键字`break`和`continue`的使用

```java
/*
1. break和continue关键字的使用

                      使用范围               在循环结构中的作用               相同点
break:                switch-case
                      循环结构中             结束(或跳出)当前循环结构         在此关键字的后面不能声明执行语句。

continue:             循环结构中             结束(或跳出)当次循环             在此关键字的后面不能声明执行语句。

2. 了解带标签的break和continue的作用。

3. 开发中，break的使用频率要远高于continue。
 */
class BreakContinueTest {
    public static void main(String[] args) {

        for(int i = 1; i <= 10; i++) {
            if(i % 4 == 0) {
                // break;
                continue;

                // 编译不通过: unreachable statement
                // System.out.println("haha");
            }
            System.out.print(i); // 123567910
        }
        System.out.println();
        // ******************************
        label:for(int j = 1; j <= 4; j++) {
            for(int i = 1; i <= 10; i++) {
                if(i % 4 == 0) {
                    break;
                    // continue;

                    // 了解:
                    // break label; // 123
                    // continue label; // 123123123123
                }
                System.out.print(i);
            }
            System.out.println();
        }
        /*
        123
        123
        123
        123
         */
    }
}
```

## 55 流程控制 通过质数的输出体会算法的魅力

```java
/*
题目: 找出100以内所有的素数(质数)? 100000以内的呢?

质数: 只能被1和它本身整出的自然数。比如: 2, 3, 5, 7, 11, 13, 17, 19...
    --> 换句话说，从2开始到这个自然数-1为止，不存在此自然数的约数。
 */
class PrimeNumberTest {
    public static void main(String[] args) {

        // 方式1:
        /*
        for(int i = 1; i <= 100; i++) { // 遍历100以内的自然数

            int number = 0; // 记录i的约数的个数(从2开始，到i-1为止)
            // 判定i是否是质数
            for(int j = 2; j < i; j++) {
                if(i % j == 0) {
                    number++;
                }
            }

            if(number == 0) {
                System.out.println(i);
            }
        }
        */

        boolean isFlag = true;
        // 方式2:
        for(int i = 1; i <= 100; i++) { // 遍历100以内的自然数

            // 判定i是否是质数
            for(int j = 2; j < i; j++) {
                if(i % j == 0) {
                    isFlag = false;
                }
            }

            if(isFlag) {
                System.out.println(i);
            }

            isFlag = true; // 重置isFlag
        }
    }
}
```

```java
/*
遍历100000以内的所有的质数，体会不同的算法实现，其性能的差别。

此PrimeNumberTest1.java是实现方式1。
 */
class PrimeNumberTest1 {
    public static void main(String[] args) {

        // 获取系统当前的时间
        long start = System.currentTimeMillis();

        boolean isFlag = true;

        int count = 0; // 记录质数的个数

        for(int i = 2; i <= 100000; i++) {
            for(int j = 2; j < i; j++) {
                if(i % j == 0) {
                    isFlag = false;
                }
            }

            if(isFlag) {
                count++;
            }

            // 重置isFlag
            isFlag = true;
        }

        // 获取系统当前的时间
        long end = System.currentTimeMillis();

        System.out.println("质数的总个数为: " + count); // 9592
        System.out.println("花费的时间为: " + (end - start)); // 3000多
    }
}
```

```java
/*
遍历100000以内的所有的质数，体会不同的算法实现，其性能的差别。

此PrimeNumberTest2.java是实现方式2，是针对于PrimeNumberTest1.java中算法的优化。
 */
class PrimeNumberTest2 {
    public static void main(String[] args) {

        // 获取系统当前的时间
        long start = System.currentTimeMillis();

        boolean isFlag = true;

        int count = 0; // 记录质数的个数

        for(int i = 2; i <= 100000; i++) {
            for(int j = 2; j <= Math.sqrt(i); j++) { // 此处Math.sqrt()提高效率
                if(i % j == 0) {
                    isFlag = false;
                    break; // 针对于非质数有效果 // 此处的break提高效率
                }
            }

            if(isFlag) {
                count++;
            }

            // 重置isFlag
            isFlag = true;
        }

        // 获取系统当前的时间
        long end = System.currentTimeMillis();

        System.out.println("质数的总个数为: " + count); // 9592
        System.out.println("花费的时间为: " + (end - start)); // 260左右 -> 加上break
                                                              // 3 -> 加上Math.sqrt()
    }
}
```

## 56 流程控制 项目一: 谷粒记账软件的演示及代码实现

`Utility.java`

```java
/**
Utility工具类：
将不同的功能封装为方法，就是可以直接通过调用方法使用它的功能，而无需考虑具体的功能实现细节。
*/
import java.util.Scanner;
public class Utility {
    private static Scanner scanner = new Scanner(System.in);
    /**
	用于界面菜单的选择。该方法读取键盘，如果用户键入’1’-’4’中的任意字符，则方法返回。返回值为用户键入字符。
	*/
	public static char readMenuSelection() {
        char c;
        for (; ; ) {
            String str = readKeyBoard(1);
            c = str.charAt(0);
            if (c != '1' && c != '2' && c != '3' && c != '4') {
                System.out.print("选择错误，请重新输入：");
            }
			else
				break;
        }
        return c;
    }
	/**
	用于收入和支出金额的输入。该方法从键盘读取一个不超过4位长度的整数，并将其作为方法的返回值。
	*/
    public static int readNumber() {
        int n;
        for (; ; ) {
            String str = readKeyBoard(4);
            try {
                n = Integer.parseInt(str);
                break;
            } catch (NumberFormatException e) {
                System.out.print("数字输入错误，请重新输入：");
            }
        }
        return n;
    }
	/**
	用于收入和支出说明的输入。该方法从键盘读取一个不超过8位长度的字符串，并将其作为方法的返回值。
	*/
    public static String readString() {
        String str = readKeyBoard(8);
        return str;
    }

	/**
	用于确认选择的输入。该方法从键盘读取‘Y’或’N’，并将其作为方法的返回值。
	*/
    public static char readConfirmSelection() {
        char c;
        for (; ; ) {
            String str = readKeyBoard(1).toUpperCase();
            c = str.charAt(0);
            if (c == 'Y' || c == 'N') {
                break;
            } else {
                System.out.print("选择错误，请重新输入：");
            }
        }
        return c;
    }


    private static String readKeyBoard(int limit) {
        String line = "";

        while (scanner.hasNext()) {
            line = scanner.nextLine();
            if (line.length() < 1 || line.length() > limit) {
                System.out.print("输入长度（不大于" + limit + "）错误，请重新输入：");
                continue;
            }
            break;
        }

        return line;
    }
}
```

`GuliAccount.java`

```java
/*
阶段一的项目: 谷粒记账软件的实现。
 */
class GuLiAccount {
    public static void main(String[] args) {

        boolean isFlag = true; // 控制循环的结束

        // 初始金额
        int balance = 10000;

        String info = ""; // 记录收支的信息

        while(isFlag) {
            System.out.println("---------------谷粒记账软件---------------\n");
            System.out.println("                1 收支明细");
            System.out.println("                2 登记收入");
            System.out.println("                3 登记支出");
            System.out.println("                4 退出\n");
            System.out.print("                请选择(1-4): ");

            char selection = Utility.readMenuSelection(); // 获取用户的选择

            switch(selection) {
                case '1':
                    System.out.println("---------------当前收支明细记录---------------");
                    System.out.println("收支\t账户金额\t收支金额\t说  明");
                    System.out.println(info);
                    System.out.println("----------------------------------------------");
                    break;
                case '2':
                    System.out.print("本次收入金额: ");
                    int money1 = Utility.readNumber();
                    if(money1 > 0) {
                        balance += money1;
                    }
                    System.out.print("本次收入说明: ");
                    String addDesc = Utility.readString();

                    info += "收入\t" + balance + "\t\t" + money1 + "\t\t" + addDesc + "\n";

                    System.out.println("---------------登记完成---------------");
                    break;
                case '3':
                    System.out.print("本次支出金额: ");
                    int money2 = Utility.readNumber();
                    if(money2 > 0 && balance >= money2) {
                        balance -= money2;
                    }
                    System.out.print("本次支出说明: ");
                    String minusDesc = Utility.readString();

                    info += "支出\t" + balance + "\t\t" + money2 + "\t\t" + minusDesc + "\n";

                    System.out.println("---------------登记完成---------------");
                    break;
                case '4':
                    System.out.print("\n确认是否退出(Y/N): ");
                    char isExit = Utility.readConfirmSelection();
                    if(isExit == 'Y') {
                        isFlag = false;
                    }
                    break;
            }
        }
    }
}
```
