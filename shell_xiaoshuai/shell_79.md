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
