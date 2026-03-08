# 第02章 变量与运算符

## 21 变量与运算符 关键字的使用

## 22 变量与运算符 标识符的使用

1. 什么是标识符? Java中变量、方法、类等要素命名时使用的字符序列，成为标识符。

- 技巧: 凡是自己可以起名字的地方都叫标识符。比如: 类名、方法名、变量名、包名、常量名等。

2.  标识符的命名规则 (必须要遵守！！否则，编译不通过)

    > 由26个英文字母大小写，0-9，\_或$组成。
    > 数字不可以开头。
    > 不可以使用关键字和保留字，但能包含关键字和保留字。
    > Java中严格区分大小写，长度无限制。
    > 标识符不能包含空格。

3.  标识符的命名规范 (建议遵守。如果不遵守，编译和运行都能正常执行。只是容易被人鄙视)

    > 包名: 多单词组成时所有字母都小写： xxxyyyzzz。
    - 例如: java.lang, com.atguigu.bean

    > 类名、接口名: 多单词组成时，所有单词的首字母大写: XxxYyyZzz
    - 例如：HelloWorld,String,System等

    > 变量名，方法名: 多单词组成时，第一个单词首字母小写，第二个单词开始每个单词首字母大写: xxxYyyZzz
    - 例如: age,name,bookName,main,binarySearch,getName

    > 常量名: 所有字母都大写。多单词时每个单词用下划线连接: XXX_YYY_ZZZ
    - 例如: MAX_VALUE,PI,DEFAULT_CAPACITY

说明: 大家在定义标识符时，要注意"见名知意"。

```java
class IdentifierTest {
    public static void main(String[] args) {
        int abc = 12;
        int age = 12; // age: 标识符

        char gender = '男';
        char c1 = '女';

        // 不推荐的写法
        int myage = 12;

        System.out.println(myage);
    }

    public static void main1(String[] args) {

    }
}

class _a$bc {
}

/*
class 1abc {
}
*/

class Public {
}

class publicstatic {
}

class BiaoShiFuTest {
}
```

## 23 变量与运算符 变量的基本使用

- 测试变量的基本使用
  1.  变量的理解: 内存中的一个存储区域，该区域的数据可以在同一个类型范围内不断变化。
  2.  变量的构成包含三个要素: 数据类型，变量名，存储的值。
  3.  Java中变量声明的格式: 数据类型 变量名 = 变量值。
  4.  Java中的变量按照数据类型来分类:
      - 基本数据类型(8种):
        整型: byte \ short \ int \ long  
        浮点型: float \ double  
        字符型: char  
        布尔型: boolean

      - 引用数据类型:
        类(class)  
        数组(array)  
        接口(interface)  
        枚举(enum)  
        注解(annotation)  
        记录(record)

  5.  定义变量时，变量名要遵循标识符命名的规则和规范。

  6.  说明:
      1️⃣ 变量都有其作用域。变量只在作用域内是有效的，出了作用域就失效了。  
      2️⃣ 在同一个作用域内，不能声明两个同名的变量。  
      3️⃣ 定义好变量以后，就可以通过变量名的方式对变量进行调用和运算。  
      4️⃣ 变量值在赋值时，必须满足变量的数据类型，并且在数据类型有效的范围内变化。

```java
class VariableTest {
    public static void main(String[] args) {

        // 定义变量的方式1
        char gender; // 过程1: 变量的声明
        gender = '男'; // 过程2: 变量的赋值(或初始化)
        gender = '女';

        // 定义变量的方式2: 声明与初始化合并
        int age = 10;

        System.out.println(age);
        System.out.println("age = " + age);

        System.out.println("gender = " + gender);

        // 在同一个作用域内，不能声明两个同名的变量
        // char gender = '女';
        gender = '女';

        // 由于number前没有声明类型，即当前number变量没有提前定义。所以编译不通过。
        // number = 10;

        byte b1 = 127;
        // b1超过了byte的范围，编译不通过。
        // b1 = 128;
    }

    public static void main123(String[] args) {
        // System.out.println("gender = " + gender);

        char gender = '女';
    }
}
```
