## 11 几个简单的内置shell命令

```bash
echo
# 常用参数
# -n: 不换行输出
# -e: 解析字符串中的特殊符号
    # \n 换行
    # \r 回车
    # \t 制表符
    # \b 退格
```

```bash
echo -n "aaa"; echo -n "bbb";
aaabbb

echo -e "aaa\nbbb"
aaa
bbb
```

```bash
printf # 打印命令
# 默认不换行，可以在""中写上面的特殊符号，"\n", "\t"等。
```

```bash
eval
# 执行后面的命令
```

```bash
exec
# 不创建子进程，执行后面的命令，且执行完毕后，自动exit。
```

## 12 变量字串的语法介绍

```text
${变量}: 返回变量值。
${#变量}: 返回变量的字符长度。
${变量:start}: 返回变量中索引为start之后的字符，索引从0开始。
${变量:start:length}: 返回变量中索引为start之后的length个字符。
${变量#word}: 从头匹配字符后删除，匹配最短的子串，也可以使用通配符。
${变量##word}: 从头匹配字符后删除，匹配最长的子串，也可以使用通配符。
${变量%word}: 从头匹配字符后删除，匹配最短的子串，也可以使用通配符。
${变量%%word}: 从头匹配字符后删除，匹配最长的子串，也可以使用通配符。
${变量/original/replacement}: 替换字符串，只替换第一个匹配的字符串。
${变量//original/replacement}: 替换字符串，替换所有匹配的字符串。
```

## 14 统计变量的长度

```text
获取变量长度的几种方式:
1) echo ${#name}
2) echo ${name} | wc -L # 找出行宽最大的那一行，并其打印长度
3) expr length ${name}
4) echo ${name} | awk '{ print length($0) }'
```

## 15 统计命令执行的时间

```text
# 使用time命令获取命令的执行时间
# 下面对比上面几种获取变量长度的方式所需要的时间，可以看到使用${#变量名}的方式是最快的。
$ time for n in {1..10000}; do str1=`seq -s "xxxxx" 100`; echo ${#str1} &>/dev/null; done
real	0m8.023s
user	0m3.681s
sys	0m4.641s
------------------------------------------------------------
$ time for n in {1..10000}; do str1=`seq -s "xxxxx" 100`; echo ${str1} | wc -L &>/dev/null; done
real	0m16.694s
user	0m9.096s
sys	0m10.625s
------------------------------------------------------------
$ time for n in {1..10000}; do str1=`seq -s "xxxxx" 100`; expr length ${str1} &>/dev/null; done
real	0m15.663s
user	0m7.078s
sys	0m9.166s
------------------------------------------------------------
$ time for n in {1..10000}; do str1=`seq -s "xxxxx" 100`; echo ${str1} | awk '{ print length($0) }' &>/dev/null; done
real	0m22.174s
user	0m9.445s
sys	0m15.626s
```

## 16 字符串截取和替换

```bash
$ name="Hello World Hello"
$ echo ${name}
Hello World Hello
$ echo ${name#hello}
Hello World Hello
$ echo ${name#Hello} # 从开始匹配子串
World Hello
$ echo ${name#H*o} # 使用通配符
World Hello
$ echo ${name##H*o}

$ echo ${name%H*o} # 从结尾匹配子串
Hello World
$ echo ${name%%H*o}

$ echo ${name/Hello/Goodbye} # 字符串替换，只替换第一个
Goodbye World Hello
$ echo ${name//Hello/Goodbye} # 字符串替换，全部替换
Goodbye World Goodbye
```

## 17 案例: 批量修改文件名

```bash
# 将文件test1_finished.png的改为test1.png。(多个文件)
touch test{1..5}_finished.png # 准备测试文件

for i in $(ls *.png); do mv ${i} ${i//_finished/}; done
```
