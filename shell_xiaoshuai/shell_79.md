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

