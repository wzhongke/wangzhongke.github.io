---
title: linux 常用命令
date: 2017-06-17 19:42:25
tags: [linux]
categories: linux
---

使用ubuntu的时候经常会把常用的一些命令忘掉或不知道有些参数的意思，又懒得看那枯燥的文档。因此记录下来备忘。<br>
<!-- more -->
## 目录类
1. cd ：切换目录
``` bash
$cd [相对路径或者绝对路径]
#回到自己的主文件夹
$cd [or cd ~]
#回到上层目录
$cd ..
#回到刚才的目录
$cd -
```
2. pwd : 显示当前目录
```bash
$pwd [-P]
-P:显示当前的路径，而非使用连接路径
```
3. mkdir : 新建目录
```bash
$mkdir [-mp] 目录名称
-m:配置文件夹的权限，忽略默认权限（umask)
-p:递归地创建目录
#新建权限为rwx--x--x的目录
$mkdir -m 711 dir_name
```
## 复制删除移动
1. 复制 cp
```bash
#只复制一个文件或文件夹
$cp [-adfipru] 源文件 目标文件
-a: 相当于 -pdr，看后文
-d：若文件为连接文件，则只复制连接文件的属性
-f: 若目标文件已经存在且无法开启，则删除后再试一次
-i: 若目标文件已存在，覆盖时会先询问是否覆盖
-p: 连同文件的属性一起复制，备份常用
**-r: 递归复制，用于目录的复制**
-u: 若目标文件比源文件旧才更新目标文件

#复制多个文件到某一文件夹下
$ cp [options] 源文件1 源文件2 ... 目标文件
```
2. 移除文件或目录
```bash
$rm [-fir] 文件或目录
-f: 忽略不存在的文件
-i: 删除前再次确认
-r: 递归删除，主要用来删除目录
```
3. 移动文件目录或者更名
```bash
$mv [-fiu] 文件或目录 目标文件或目录
-f: 目标文件存在时，不询问直接覆盖
-i: 目标文件存在，询问是否覆盖
-u: 目标文件存在，且较新时，才会更新
```
## 命令与文件查询
1. which：可以查询脚本文件的位置，比如 `ifconfig` 命令的位置。但是不能够查询bash内置的命令，比如`cd`
```bash
which [-a] command
-a: 列出所有 PATH 目录中包含的命令，没有该参数，只会列出第一个
```
2. whereis： 定位命令的二进制，源文件和帮助文件
```bash
whereis [-bmsu] 文件或目录名
-b : 只找二进制文件
-m : 在menu下查找
-s : 只找源文件
-u : 其他文件
```
3. locate： 根据文件名搜索文件，输出所有的文件。因为是从存储文件记录的数据库文件`/var/lib/mlocate`中读取的，所以速度快。但是数据库文件是定时更新的，所以新增的文件查询不到。可以通过`updatedb`来更新文件，因为该命令是查找硬盘的，所以执行比较慢。
```bash
locate [-ir] 文件名
-i: 忽略大小写差异
-r: 可以接正则表达式
```
4. find: 在目录下搜索文件，与xargs一起使用，功能强大
```bash
$ find [PATH] [option] [action]
-mtime n: 在n天之前的 一天内 被更改过的文件
-mtime +n: 在n天之前（不含n天）被更改过的文件
-mtime -n: 在n天之内（含n天） 被更改过的文件
-newer file: file问一个文件的路径，列出比file新的文件

-name filename: 查找文件名为filename的文件， 使用通配符表示文件名时，需要加上 ''
-size [+-]SIZE: 查找比size还要大(+)或小(-)的文件 ,可以是用K\M\G

-exec command: command为命令，该命令可以处理查找结果，不支持别名。 相对来说配合xargs更强大
$ find / -exec ls -l {}\;

```
## 更改权限
权限分数为： r(read)=4, w(write)=2, x(execute)=1
```bash
$ chgrp [-R（递归更改)] groupname dirname/filename: 改变文件所属用户组
$ chown [-R（递归更改)] username[:groupname] dirname/filename：改变文件所有者
$ chmod [-R（递归更改)] [options] dirname/filename：改变文件所有者
$ chmod 761 file: 将文件权限更改为 =rwxrw---x
$ chmod u=rwx,g=rw,o=x file: u(user) g(group) o(others) =(设置) +(增加) -(取消)

```


## 查看文件内容
1. cat
```bash
$cat [-AbEnTv] 文件
-A: 相当于-vET 的整合，可列出一些特殊字符而不是空白
-b: 列出行号，仅针对非空白行做行号显示
-E: 将结尾的断行字符以 $ 显示出来
-n: 打印行号，连同空白行也会有行号
-T: 将[Tab]以 ^I显示出来
-v: 列出一些看不出来的特殊字符
```
2. 翻页查看more
```bash
$more 文件路径
#空格键    向下翻一页
#Enter    向下滚动一行
#/字符串   在内容中向下查找字符串
#:f       显示文件名以及目前显示的行数
#q        离开more，不再显示内容
#b        向回翻页，只对文件有效
```
3. 翻页查看 less
```bash
$less 文件路径
#空格键         向下翻一页
#[PageDown]    向下翻一页
#[PageUp]      向上翻一页
#/字符串   在内容中向下查找字符串
#?字符串   在内容中向上查找字符串
#n        重复前一个查询
#N        向上重复前一个查询
#q        离开
````
4. 数据选取 head
```bash
$head [-n number] 文件
-n: 接数字，表示显示头几行，默认显示前10行
```
5. 数据选取tail
```bash
tail [-n number] 文件
-n: 表示显示几行
```
另外可以修改 /etc/issue文件来改变终端的提示信息
## 压缩
### zip
1. 使用zipsplit分割压缩的zip文件
2. 

## 其他快捷方式
1. 使用快捷键 `ctrl+r` 可以快速使用历史命令
```
zipsplit (选项) (参数)
-n：指定分割后每个zip文件的大小，是字节大小；
-t：报告将要产生的较小的zip文件的大小；
-b：指定分割后的zip文件的存放位置。
```


## 时间
date +%Y%m%d