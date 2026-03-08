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

## 24 变量与运算符 整型数据类型的使用

## 25 变量与运算符 浮点类型的使用及练习

```java
/*
测试整型和浮点型变量的使用
*/
class VariableTest1 {
    public static void main(String[] args) {

        // 1. 测试整型变量的使用
        // byte(1字节=8bit) / short(2字节) / int(4字节) /long(8字节)
        byte b1 = 12;
        byte b2 = 127;
        // 编译不通过，因为超出了byte的存储范围。
        // byte b3 = 128;

        short s1 = 1234;

        int i1 = 123234123;

        // 1️⃣  声明long类型变量时，需要提供后缀。后缀为'l'或'L'。
        long l1 = 123234123L;

        // 2️⃣  开发中，大家定义整型变量时，没有特殊情况的话，通常都声明为int类型。

        // 2. 测试浮点类型变量的使用
        // float / double
        double d1 = 12.3;
        // 1️⃣  声明float类型变量时，需要提供后缀。后缀为'f'或'F'。
        float f1 = 12.3F;
        System.out.println("f1 = " + f1);

        // 2️⃣  开发中，大家定义浮点型变量时，没有特殊情况的话，通常都声明为double类型，因为精度更高。

        // 3️⃣  float类型表数范围要大于long类型的表数范围，但是精度不高。

        // 测试浮点型变量的精度
        // 结论: 通过测试发现浮点型变量的精度不高。如果在开发中，需要极高的精度，需要使用BigDecimal类替换浮点型变量。
        // 测试1
        System.out.println(0.1 + 0.2); // 0.30000000000000004
                                       //
        // 测试2
        float ff1 = 123123123f;
        float ff2 = ff1 + 1;
        System.out.println(ff1);
        System.out.println(ff2);
        System.out.println(ff1 == ff2);

    }
}
/*
结果:
f1 = 12.3
0.30000000000000004
1.2312312E8
1.2312312E8
true
*/
```

```java
/*
案例1: 定义圆周率并赋值为3.14，现有3个圆的半径分别为1.2、2.5、6，求它们的面积。
*/
class FloatDoubleExer {
    public static void main(String[] args) {

        // 定义圆周率变量
        double pi = 3.14;

        // 定义3个圆的半径
        double radius1 = 1.2;
        double radius2 = 2.5;
        int radius3 = 6;

        // 计算面积
        double area1 = pi * radius1 * radius1;
        double area2 = pi * radius2 * radius2;
        double area3 = pi * radius3 * radius3;

        System.out.println("圆1的半径为: " + radius1 + "，面积为: " + area1);
        System.out.println("圆2的半径为: " + radius2 + "，面积为: " + area2);
        System.out.println("圆3的半径为: " + radius3 + "，面积为: " + area3);
    }
}
/*
结果:
圆1的半径为: 1.2，面积为: 4.521599999999999
圆2的半径为: 2.5，面积为: 19.625
圆3的半径为: 6，面积为: 113.03999999999999
*/
```

```java
/*
案例2: 小明要到美国旅游，可是那里的温度是以华氏度为单位记录的。
它需要一个程序将华氏温度(80度)转换为摄氏度，并以华氏度和摄氏度为单位分别显示该温度。

'C = ('F - 32) / 1.8
*/

class FloatDoubleExer1 {
    public static void main(String[] args) {

        double hua = 80.0;

        double she = (hua - 32) / 1.8;

        System.out.println("华氏度" + hua + "'F 对应的摄氏度为" + she + "'C");
    }
}
/*
结果:
华氏度80.0'F 对应的摄氏度为26.666666666666664'C
*/
```

## 26 变量与运算符 字符类型的使用

## 27 变量与运算符 布尔类型的使用

```java
/*
测试字符类型和布尔类型的使用
*/
class VariableTest2 {
    public static void main(String[] args) {

        // 1. 字符类型: char(2字节)

        // 表示形式1: 使用一对''表示，内部有且只有一个字符。
        char c1 = 'a';
        char c2 = '中';
        char c3 = '1';
        char c4 = '%';
        char c5 = '👏';

        // 编译不通过
        // char c6 = '';

        // char c7 = 'ab';

        // 表示形式2: 直接使用Unicode值来表示字符型常量。
        char c8 = '\u0036';
        System.out.println(c8); // 6

        // 表示形式3: 使用转义字符。
        char c9 = '\n';
        char c10 = '\t';
        System.out.println("hello" + c9 + "world"); // hello
                                                    // world
        System.out.println("hello" + c10 + "world"); // hello	world

        // 表示形式4: 使用具体字符对应的数值(比如ASCII码)。
        char c11 = 97;
        System.out.println(c11);

        char c12 = '1'; // 49
        char c13 = 1;
        System.out.println(c12 == c13); // false

        // 2. 布尔类型: boolean
        // 1️⃣  只有两个取值: true、false。
        boolean bo1 = true;
        boolean bo2 = false;

        // 编译不通过
        // boolean bo3 = 0;

        // 2️⃣  常使用在流程控制语句中。比如: 条件判断、选择结构、循环结构等。
        boolean isMarried = true;
        if(isMarried) {
            System.out.println("很遗憾，不能参加单身派对了");
        } else {
            System.out.println("可以多谈几个女朋友或男朋友");
        }

        // 3️⃣  了解: 我们不谈boolean类型占用的空间大小。但是，真正在内存中分配的话，使用的是4个字节。
    }
}
```

## 28 变量与运算符 基本数据类型变量间的自动类型提升规则

```java
/*
测试基本数据类型变量间的运算规则。

1. 这里提到可以做运算的基本数据类型有7种，不包含boolean类型。
2. 运算规则包括:
    1️⃣  自动类型提升
    2️⃣  强制类型转换
3. 此VariableTest3.java用来测试自动类型提升。

规则: 当容量小的变量与容量大的变量做运算时，结果自动转换为容量大的数据类型。

    byte 、short、char --> int --> long --> float --> double

    特别的: byte、short类型的变量之间做运算，结果为int类型。

说明: 此时的容量小或大，并非指占用的内存空间的大小，而是值表示数据的范围的大小。
    long(8字节)、float(4字节)
 */
class VariableTest3 {
    public static void main(String[] args) {

        int i1 = 10;
        int i2 = i1;

        long l1 = i1;

        float f1 = l1; // 由于float类型的表数范围比long类型的大，所以此处编译没有问题。

        byte b1 = 12;
        int i3 = b1 + i1;

        // 编译不通过
        // byte b2 = b1 + i1;

        // ************************************************************
        // 特殊的情况1: byte、short之间做运算。
        byte b3 = 12;
        short s1 = 10;
        // 编译不通过
        // short s2 = b3 + s1;
        i3 = b3 + s1;

        byte b4 = 10;
        // 编译不通过
        // byte b5 = b3 + b4;

        // 特殊的情况2: char。
        char c1 = 'a';
        // 编译不通过
        // char c2 = c1 + b3;
        int i4 = c1 + b3;

        // ************************************************************
        // 练习1:
        long l2 = 123L;
        long l3 = 123; // 自动类型提升(int --> long)

        // long l4 = 123123123123; // 123123123123理解为int类型，因为超出了int范围，所以报错。
        long l5 = 123123123123L; // 此时的123123123123就是使用8个字节存储的long类型的值。

        // 练习2:
        float f2 = 12.3F;
        // float f3 = 12.3; // 不满足自动类型提升的规则(double --> float)，所以报错。

        // 练习3:
        // 规定1: 整型常量，规定为int类型。
        byte b5 = 10;
        // byte b6 = b5 + 1;
        int ii1 = b5 + 1;

        // 规定2: 浮点型常量，规定为double类型。
        double dd1 = b5 + 12.3;

        // 练习4: 说明为什么不允许变量名是以数字开头的，为了"自洽"。
        // int 123L = 12;
        // long l6 = 123L;

        System.out.println("自动类型提升");
    }
}
```
