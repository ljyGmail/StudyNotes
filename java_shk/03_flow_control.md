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
