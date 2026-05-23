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