---
title: linux之bash shell基础
date: 2019-10-02 13:05:29
tags: [linux, bash, shell]
categories: Linux
---

# 前言

[文件和目录列表](#file-dir)
[处理文件](#handle-doc)
[处理目录](#handle-dir)
[查看文件](#cat-file)

## <span id="file-dir">文件和目录列表</span>

**显示当前文件和目录**

```shell
» ls
```
```js
README-zh_CN.md      commitlint.config.js example              node_modules
README.md            docs                 lerna.json           package-lock.json
```
ls是按字母排序的, 可以带参数 `-F` 用来区分文件和目录
```shell
» ls -F
```
```js
README-zh_CN.md       commitlint.config.js  example/              node_modules/
README.md             docs/                 lerna.json            package-lock.json
```
`-F`参数在目录后面加了正斜线(/)

**显示隐藏文件**
利用参数 `-a`来显示隐藏文件和普通文件, 所有以点号开始的隐藏文件都显示出来了
```shell
» ls -a
```
```js
.git                 README-zh_CN.md      commitlint.config.js example              node_modules
..                   .gitignore           README.md            docs                 lerna.json 
```
**显示长列表**
利用`ls -l`产生每个文件的更多信息, 比如目录(d), 文件(-), 字符型文件(c)或者块设备(b)
```shell
ls -l
```
```js
» ls -l
total 176
-rw-r--r--  1 yuqi.bi  staff   4453 Sep 25 23:10 _config.yml
-rw-r--r--  1 yuqi.bi  staff  77732 Sep 25 23:10 package-lock.json
-rw-r--r--  1 yuqi.bi  staff    667 Sep 25 23:10 package.json
drwxr-xr-x  5 yuqi.bi  staff    160 Sep 25 23:10 scaffolds
drwxr-xr-x  9 yuqi.bi  staff    288 Sep 25 23:10 source
```

**过滤输出列表**
`ls` 会输出很多信息，可以针对目标文件进行过滤搜索,这里支持的是正则表达式

```shell
# 搜索以——conf开头的文件
» ls -l _confi*
```
```js
» ls -l _confi*
-rw-r--r--  1 yuqi.bi  staff  4453 Sep 25 23:10 _config.yml
```

## <span id="handle-doc">处理文件</span>

**创建文件**

```shell
» touch test_one
» ls -l test_one
```
显示如下：
```js
-rw-r--r--  1 [Here is your user name]  staff  0 Oct  3 00:06 test_one
```

`ls -l` 不会直接显示访问时间，默认是显示修改时间，如需查看需啊哟加上`--time=attime`

```shell
» ls -l --time=attime test_one 
```

**复制文件**
`cp [-R [-H | -L | -P]] [-fi | -n] [-apvX] source_file target_file`

基本用法： `cp 命令需要两个参数--源对象和目标对象`

```shell
» cp source destination
```
当source和destination 参数是文件名时，cp命令将源文件复制成一个新文件，并命名为destination.

eg:

```shell
» cp test_one test_two
```
如果目标文件已经存在，cp命令并不会提示，所以可以加`-i`来强制shell提示是否覆盖.
```shell
~/Desktop » cp -i test_one test_two
overwrite test_two? (y/n [n])
```
cp命令`-R`参数可以递归的复制整个目录文件

```shell
» cp -R source/ target
```

也可以在cp命令中使用通配符

```bash
» cp *script new_script/
```
该命令静所有以script结尾的文件赋值到new_script目录中，可以通过`ls -l new_script`查看

**重命名文件**

`mv [-f | -i | -n] [-v] source target`

mv命令可以将文件和目录移动到另一个位置或者重新命名.

> Create directory `linux-demo` with demo file and demo dir

```bash
» ls -lF
total 0
drwxr-xr-x  2 yuqi.bi staff  64 Oct  4 15:48 demo/
-rw-r--r--  1 yuqi.bi staff   0 Oct  4 15:48 demo.txt
```

执行mv命令
```shell
» mv demo demo-with-mv

» ls -lF de*
total 0
drwxr-xr-x  2 yuqi.bi  staff  64 Oct  4 15:48 demo-with-mv/
-rw-r--r--  1 yuqi.bi  staff   0 Oct  4 15:48 demo.txt
```
也可以使用mv移动位置并修改文件名称

```shell
» mv linux-demo linux-demo2
```

**删除文件**

`rm [-dfiPRrvW] file ...`

```shell
» rm -i demo.txt
```
`-i` 提示是不是要真的删除文件

如果要删除很多文件不受提示干扰，可用`-f` 强制删除 `-rf`递归强制删除

```shell
» rm -rf demo.txt
```

## <span id="handle-dir">处理目录</span>

**创建目录**

`mkdir [-pv] [-m mode] directory_name ...`

```shell
» mkdir new_dir
```

如果想批量创建目录和子目录，需要加上`-p`, 可以根据需要创建缺失目录.

```shell
» mkdir -p new_dir/sub_dir_child_dir
```

**删除目录**

`rmdir [-p] directory ...`

默认情况下`rmdir`只删除非空目录, 只能先删除文件，才能删除目录

所以想要一口气删除所有文件，还是需要用到`rm -rf [target dir name]`

## <span id="cat-file">查看文件</span>

`file [-bcdDhiIkLnNprsvz] [--extension] [--mime-encoding] [--mime-type] [-f namefile] [-m magicfiles] [-P name=value]`

file命令是一个能够探测文件内部的工具

**查看文件类型**
```shell
» file demo.text
```
会返回文件的类型

**查看整个文件**

- cat命令

```shell
cat demo.txt
```

`-n` 可以给所有行加上行号
`-b` 可以只给文本行添加行号
`-T` 移除制表符

**查看部分文件**

- tail 命令
显示文件最后几行的内容

`tail [-F | -f | -r] [-q] [-b number | -c number | -n number] [file ...]`

`tail -n number [file]` 显示指定行数的内容

- head 命令

`head [-n count | -c bytes] [file ...]
`
查看文件起始内容

```shell
» head -n [number] [file] 
```