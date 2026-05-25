```bash
# 玩Linux脚本命令时使用的Docker命令
$ docker run -it nicolaka/netshoot /bin/bash
```

## 4. 变量-全局变量

- env命令查看全局变量

```bash
$ env | less
```

- printenv命令查看全局变量

```bash
$ printenv
$ printenv PATH # 查看某个全局变量的值，不需要在前面加$
$ printf "$PATH\n"
```

- 使用export把局部变量改为全局变量

```bash
$ export VAR1="tz is good"
```

- 使用set命令输出全局变量和局部变量

```bash
$ set | less
```

- 使用declare命令输出所有的变量、函数、导出的变量

```bash
$ declare | less
```

```text
declare: 函数(方法)、局部变量、全局变量
set: 局部变量、全局变量
printenv: 全局变量
```

## 5. 变量-环境变量

环境变量: 运行程序的环境的变量
用户: admin / zhangsan / li...
系统: 所有的用户是通用的

```bash
# 自定义临时环境变量的三种方式
$ export NAME=hello
$ NAME=hello; export NAME
$ declare -x NAME=hello

# 自定义永久环境变量的三种方式
`~/.bashrc`: 用户相关的环境变量
`/etc/profile`: 所有用户通用的环境变量的配置位置

# 使用unset移除本地跟环境变量
$ export NAME=hello
$ echo $NAME
hello
$ unset NAME
$ echo $NAME
```

## 6. 变量-局部变量

```bash
$ name=hello
$ name='hello'
$ name="hello"

# 使用变量的值给其它的变量赋值
$ value2=$value
$ echo $value2
10

# 加单引号输出，会原样输出，不会解析变量
$ echo 'my name is $name'
my name is $name

# 使用双引号时，默认解析变量，如果不想解析变量，就在$前加反斜杠
$ echo "my name is \$name"
my name is $name

# 将命令的结果作为值赋给变量
# 方式1:
$ myls=`ls`
$ echo $myls
a.txt b.txt c.txt

# 方式2:
$ myls2=$(ls)
$ echo $myls2
a.txt b.txt c.txt

# 变量后面连接其它字符时需要加上大括号
$ echo $name_pc
# No result

$ echo ${name}_pc
hello_pc
```

## 7. Shell特殊变量 (位置变量)

```text
$0获取当前执行脚本的文件名，包括脚本路径和命令本身。
$n获取当前脚本的第n个参数，当n大于10时，用${10}表示。
```

```bash
# print.sh
#!/bin/bash
echo "脚本名称: $0"
echo "第一个参数: $1"
echo "第二个参数: $2"
echo "第三个参数: $3"
echo "第四个参数: $4"

################
$ chmod +x print.sh
$ ./print.sh 1 2 3 4
脚本名称: ./print.sh
第一个参数: 1
第二个参数: 2
第三个参数: 3
第四个参数: 4
```

## 8. Shell特殊变量 (特殊变量)

```bash
# special_vars.sh
#!/bin/bash

echo '$* 这个脚本的所有参数:' $*
echo '$# 这个脚本的参数的个数:' $#
echo '$$ 这个脚本的进程ID:' $$

touch /tmp/a.txt &
echo '/tmp/a.txt &'
echo '$! 上一个后台脚本的PID:' $!
echo '$? 上一个脚本执行的结果:' $?

##############################
$ ./special_vars.sh 1 2 3
$* 这个脚本的所有参数: 1 2 3
$# 这个脚本的参数的个数: 3
$$ 这个脚本的进程ID: 109
/tmp/a.txt &
$! 上一个后台脚本的PID: 110
$? 上一个脚本执行的结果: 0
```

## 9. Shell特殊变量 (数组变量)

```bash
# 定义数组
$ myarray=(one two three four five)

# 输出数组内容
$ echo ${myarray[@]}
# 或者
$ echo ${myarray[*]}

# 输出数组的长度
echo ${#myarray[@]}

# [@] → 每个元素独立处理（推荐）
# [*] → 整个数组当成一个字符串

# 使用索引访问数组元素
$ echo ${myarray[0]}
$ echo ${myarray[1]}
$ echo ${myarray[2]}

# 遍历数组
for item in "${myarray[@]}"
do
    echo $item
done
```

## 10. Shell脚本以及重定向

```bash
# 内联输入重定向
$ cat << qqq || wc -l
> aaa
> bbb
> ccc
> qqq
3
```

## 11. 初级部分总结

## 12. 数学运算符

```text
expr命令: 对数字做比较时，假为0，真为1，特殊符号需要转义。
只支持整数运算，注意运算符的两边要有空格。
```

```bash
#!/bin/bash

num1=5
num2=3

echo "num1: $num1, num2: $num2"
echo '******************************'
echo 'expr $num1 \| $num2:' $(expr $num1 \| $num2) # num1不为空且非0，返回num1;否则返回num2
echo 'expr $num1 \& $num2:' $(expr $num1 \& $num2) # num1不为空且非0，返回num1;否则返回0
echo '******************************'
echo 'expr $num1 \< $num2:' $(expr $num1 \< $num2)
echo 'expr $num1 \<= $num2:' $(expr $num1 \<= $num2)
echo 'expr $num1 \> $num2:' $(expr $num1 \> $num2)
echo 'expr $num1 \>= $num2:' $(expr $num1 \>= $num2)
echo 'expr $num1 = $num2:' $(expr $num1 = $num2)
echo 'expr $num1 != $num2:' $(expr $num1 != $num2)
echo '******************************'
echo 'expr $num1 + $num2:' $(expr $num1 + $num2)
echo 'expr $num1 - $num2:' $(expr $num1 - $num2)
echo 'expr $num1 \* $num2:' $(expr $num1 \* $num2)
echo 'expr $num1 / $num2:' $(expr $num1 / $num2)
echo 'expr $num1 % $num2:' $(expr $num1 % $num2)
```
