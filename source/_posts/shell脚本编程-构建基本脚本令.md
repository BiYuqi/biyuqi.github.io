---
title: shell脚本编程-构建基本脚本令
date: 2019-10-20 13:05:29
tags: [linux, shell]
categories: Linux
---

## 创建shell脚本文件

创建shell脚本文件时,需要在第一行指定要使用的shell:

```shell
#! /bin/bash
```
通常井(#)号用作注释行, 第一行是个例外.

**1.改变文件权限**

chmod命令用来改变文件和目录的安全性，格式如下：


```shell
chmod options mode file
```
mode 参数可以使用八进制模式或者符号进行安全设置

**2.八进制模式**
```shell
chmod 760 somefile
```
得到：
```shell
ls -l somefile
# -rwxrw----
```

**3.符号模式**
`[ugoa][[+-=][rwxXstigo]]`

```shell
u 代表用户
g 代表组
o 代表其他
a 代表上述所有
------
r read
w write
x 执行权限
X 如果对象是目录或者它已有执行权限，赋予执行权限
s 运行时重新设置UID or GID
t 包括文件或者目录
u 设置属主权限
g 设置属组权限
o 设置其他用户权限
```

**4.执行shell文件**
第一次执行文件可能会告知没有权限
```shell
permission denied: ./somefile.sh
```

这时候可以用到👆的方法进行赋予权限：
```shell
chmod u+x somefile.sh
```

**5.echo输出消息**
在echo命令后加上字符串，就可以显示该文本
```shell
echo "This is a test"
# This is a test
``` 

## 使用变量
set命令可以显示完整的当前环境变量

**1.环境变量**
```shell
set
```
如果我们想使用环境变量，只需在名称之前加上$美元符号即可
```shell
echo $HOME
```
**2.用户变量**
用户可以定义自己的变量,比如我们在方才的`somefile.sh`文件中写入：
```shell
#! /bin/bash

val1=5
val2=10
echo ${val1} ${val2}

# 5 10
```

## 重定向输入输出
**1.输出重定向**
基本的重定向是将命令输出发送到一个文件中.
```shell
command > outputfile
```
e.g.

```shell
date > test
```
创建了一个test文件, 并将日期写入到该文件中.
如果文件已经存在，会覆盖原有内容，如果不想覆盖原有内容，而是追加内容，可用双大于号(>>)

```shell
date >> test
```
**2.输入重定向**
输入重定向和输出刚好是相反的，将内容重定向到命令, 用小于号`<`

```shell
command < inputfile
```

e.g.

```shell
wc < inputfile
```

可以对文件中的数据进行计数，默认3个值， 行数,词数,字节数

还有一种输入重定向: 内联输入重定向. 内联重定向是远小于号(`<<`),该方法无需使用文件进行查询，只需要在命令行中指定用于输入重定向的数据, 除了该符号，还需要指定一个文本标记来划分输入数据的开始结尾.

## 数学运算

**1.方括号**
`$[ operation]`

```shell
val1=5
val2=10
val3=$[$val1 - $val2]
echo $val3

# -5
```

**2.浮点运算**
```shell
variable=$(echo "options; expression" | bc)
```

第一部分options允许你设置变量, 多个变量用分号隔开; expression参数定义了通过`bc`执行数学表达式

```shell
#! /bin/bash
val1=100
val2=45
val3=$(echo "scale=4; $val1 / $val2"| bc)

echo The answer for this is $val3

# The answer for this is 2.2222
```
scale变量设置成了四位小数，并在expression中指定了特定和运算

## 退出脚本

**查看退出状态码**
```shell
$?
```
e.g.
```shell
echo $?
# 0
```
linux退出状态码

状态码 | 描述 |
----- | ----- |
0 | 命令成功结束
1 | 一般性未知错误
2 | 不适合的shell命令
126 | 命令不可执行
127 | 没找到命令
128 | 无效的退出参数
128+x | 与linux信号相关的严重错误
130 | 通过 Ctrl + C 终止
255 | 正常范围之外的状态码

**exit命令**
exit命令允许在脚本结束时指定一个退出状态码