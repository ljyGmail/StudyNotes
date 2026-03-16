# 第03章 流程控制语句

## 41 流程控制 `if else`结构第基本使用

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
