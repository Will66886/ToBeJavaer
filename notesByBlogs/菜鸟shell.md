# 菜鸟Shell

https://www.runoob.com/linux/linux-shell.html

## 1、Shell脚本介绍

### shell脚本基本介绍

Shell 是一个用 C 语言编写的程序，它是用户使用 Linux 的桥梁。Shell 既是一种命令语言，又是一种程序设计语言。

Shell 是指一种应用程序，这个应用程序提供了一个界面，用户通过这个界面访问操作系统内核的服务。

Ken Thompson 的 sh 是第一种 Unix Shell，Windows Explorer 是一个典型的图形界面 Shell。

### 第一个shell脚本

```shell
#!/bin/bash
echo "Hello World !"
```

**#!** 是一个约定的标记，它告诉系统这个脚本需要什么解释器来执行，即使用哪一种 Shell。

### 运行shell脚本的两种方法

1. 作为可执行程序

   ```linux
   chmod +x ./test.sh  #使脚本具有执行权限
   ./test.sh  #执行脚本
   ```

2. 作为解释器参数

   ```
   /bin/sh test.sh
   /bin/php test.php
   ```

## 2、变量

### 定义变量

```shell
your_name="runoob.com"
```

变量名规则

- 命名只能使用英文字母，数字和下划线，首个字符不能以数字开头。
- 中间不能有空格，可以使用下划线（_）。
- 不能使用标点符号。
- 不能使用bash里的关键字（可用help命令查看保留关键字）。

除了显式地直接赋值，还可以用语句给变量赋值，如：

```shell
for file in `ls /etc`
或
for file in $(ls /etc)
```

以上语句将 /etc 下目录的文件名循环出来。

### 使用变量

```shell
your_name="qinjx"
echo $your_name
echo ${your_name}
```

花括号尽量不要省略

### 只读变量

```shell
#!/bin/bash
myUrl="https://www.google.com"
readonly myUrl
myUrl="https://www.runoob.com"
```

运行结果

```
/bin/sh: NAME: This variable is read only.
```

删除变量

```shell
#!/bin/sh
myUrl="https://www.runoob.com"
unset myUrl
echo $myUrl
```

### 变量类型

运行shell时，会同时存在三种变量：

- **1) 局部变量** 局部变量在脚本或命令中定义，仅在当前shell实例中有效，其他shell启动的程序不能访问局部变量。
- **2) 环境变量** 所有的程序，包括shell启动的程序，都能访问环境变量，有些程序需要环境变量来保证其正常运行。必要的时候shell脚本也可以定义环境变量。
- **3) shell变量** shell变量是由shell程序设置的特殊变量。shell变量中有一部分是环境变量，有一部分是局部变量，这些变量保证了shell的正常运行

## 3、shell字符串

### 单引号

```shell
str='this is a string'
```

单引号字符串的限制：

- 单引号里的任何字符都会原样输出，单引号字符串中的变量是无效的；
- 单引号字串中不能出现单独一个的单引号（对单引号使用转义符后也不行），但可成对出现，作为字符串拼接使用。

### 双引号

```shell
your_name='runoob'
str="Hello, I know you are \"$your_name\"! \n"
echo -e $str
```

输出结果为：

```
Hello, I know you are "runoob"! 
```

双引号的优点：

- 双引号里可以有变量
- 双引号里可以出现转义字符

### 拼接字符串

````shell
bin/bash
your_name="runoob"
#""
greeting="hello,"$your_name"!"
greeting_1="hello,${your_name}!"
echo ${greeting} ${greeting_1}
#''
greeting_2='hello,'your_name'!'
greeting_3='hello,${your_name}!'
echo ${greeting_2} ${greeting_3}
````

```
hello,runoob! hello,runoob!
hello,your_name! hello,${your_name}!
```

### 获取字符串长度

```shell
tring="abcd"
echo ${#string}
```

### 提取子字符串

```shell
tring="runoob is a greet site"
echo ${string:1:4}# 输出 unoo
```

索引从0开始

查找子字符串
