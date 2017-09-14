---
title: linux 常用命令
date: 2017-06-17 19:42:25
tags: ["linux"]
categories: ["linux"]
---

使用ubuntu的时候经常会把常用的一些命令忘掉或不知道有些参数的意思，又懒得看那枯燥的文档。因此记录下来备忘。<br>
<!-- more -->

## 目录类
1. ls: 查看文件与目录
```bash
ls [-aAdfFhilnrRSt] 目录或文件
ls [--color={never,auto,always}] 目录或文件
ls [--full-time] 目录或文件
-a: 列出全部文件，包括隐藏文件
-A: 列出全部文件，包括隐藏文件，但不包括 . 与 .. 两个目录
-d: 仅列出目录，不列出目录内的文件
-f: 直接列出结果，不进行排序 (ls默认会以文件名排序)
-F: 根据文件、目录等信息给予附件数据结构 ( *:代表可执行文件，/: 代表目录，=: 代表socket文件，|: 代表FIFO文件)
-h: 将文件容量以易读的方式列出
-i: 列出inode号码
-l: 列出文件属性权限等
-n: 列出UID与GID，而非用户与用户组名
-r: 将排序结果反向输出
-R: 连同子目录内容一起列出来
-S: 以文件容量大小排序
-t: 以时间排序
-color: never(不要依据文件特性给予颜色显示)，always(显示颜色)，auto(系统判定是否显示颜色)
--full-time: 以完整的时间模式输出 年、月、日、时、分
--time={atime, ctime}: 输出访问时间或改变权限属性时间(ctime)，而非内容更改时间
```
2. cd: 切换目录
``` bash
cd [相对路径或者绝对路径]
#回到自己的主文件夹
cd [or cd ~]
#回到上层目录
cd ..
#回到刚才的目录
cd -
```
3. pwd : 显示当前目录
```bash
pwd [-P]
-P:显示当前的路径，而非使用连接路径
```
4. mkdir : 新建目录
```bash
mkdir [-mp] 目录名称
-m:配置文件夹的权限，忽略默认权限（umask)
-p:递归地创建目录
#新建权限为rwx--x--x的目录
mkdir -m 711 dir_name
```
## 复制删除移动
1. 复制 cp
```bash
#只复制一个文件或文件夹
cp [-adfilprsu] 源文件 目标文件
-a: 相当于 -pdr，看后文
-d：若文件为连接文件，则只复制连接文件的属性
-f: 若目标文件已经存在且无法开启，则删除后再试一次
-i: 若目标文件已存在，覆盖时会先询问是否覆盖
-l: 创建文件的硬链接
-p: 连同文件的属性一起复制，备份常用
**-r: 递归复制，用于目录的复制**
-s: 创建文件的软连接
-u: 若目标文件比源文件旧才更新目标文件

#复制多个文件到某一文件夹下
$ cp [options] 源文件1 源文件2 ... 目标文件
```
2. 移除文件或目录
```bash
rm [-fir] 文件或目录
-f: 忽略不存在的文件
-i: 删除前再次确认
-r: 递归删除，主要用来删除目录
```
3. 移动文件目录或者更名
```bash
mv [-fiu] 文件或目录 目标文件或目录
-f: 目标文件存在时，不询问直接覆盖
-i: 目标文件存在，询问是否覆盖
-u: 目标文件存在，且较新时，才会更新
```
4. 非纯文本 `od`
```bash
od [-t TYPE] 文件
-t: 后面可以接各种类型输出：
   a : 利用默认字符输出
   c : 使用ASCII字符输出
   d[size]: 用十进制输出数据，每个整数占用 size bytes
   f[siez]: 用浮点数输出数据，每个整数占用 size bytes
   o[size]: 用八进制来输出数据，每个整数占用 size bytes
   x[size]: 用十六进制来输出数据，每个整数占用 size bytes
```
5. 连接文件 `ln`
硬连接是将文件对应到同一个inode号码上的连接。硬连接不能跨文件系统，不能连接到目录。符号连接就是windows下的快捷方式。
```bash
ln [-sf] 源文件 目标文件(符合连接文件)
    -s: 创建符号连接，而不是硬连接
    -f: 如果目标文件存在，就将目标文件删除后再创建
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
find [PATH] [option] [action]
-mtime n: 在n天之前的 一天内 被更改过的文件
-mtime +n: 在n天之前（不含n天）被更改过的文件
-mtime -n: 在n天之内（含n天） 被更改过的文件
-newer file: file问一个文件的路径，列出比file新的文件

-name filename: 查找文件名为filename的文件， 使用通配符表示文件名时，需要加上 ''
-size [+-]SIZE: 查找比size还要大(+)或小(-)的文件 ,可以是用K\M\G

-exec command: command为命令，该命令可以处理查找结果，不支持别名。
find / -exec ls -l {}\;
find命令会将所有匹配到的文件一起传递给exec执行，但有些系统对能够传递给exec的命令有长度限制，会出现溢出错误。这时候可以使用xargs。
find . | xargs grep xxx: 查找当前目录下含有x的文件
```
## 更改权限
权限分数为： r(read)=4, w(write)=2, x(execute)=1
```bash
chgrp [-R（递归更改)] groupname dirname/filename: 改变文件所属用户组
chown [-R（递归更改)] username[:groupname] dirname/filename：改变文件所有者
chmod [-R（递归更改)] [options] dirname/filename：改变文件所有者
chmod 761 file: 将文件权限更改为 =rwxrw---x
chmod u=rwx,g=rw,o=x file: u(user) g(group) o(others) =(设置) +(增加) -(取消)
```

## 数据流重定向
- `1>` : 以覆盖的方式将正确的数据输出到指定的文件或设备上
- `1>>` : 以累加的方式将正确的数据输出到指定的文件或设备上
- `2>` : 以覆盖的方式将错误的数据输出到指定的文件或设备上
- `2>>` : 以累加的方式将错误的数据输出到指定的文件或设备上
- `command > list 2>&1` ： 将正确信息和错误信息都输入到list文件中
- `command 2> /dev/null` : 不保存错误信息
## 命令执行判断依据： `;` `&&` `||`
- `com1;com2`  不考虑命令的相关性，连续执行命令 
- `com1&&com2` 前一个命令执行正确($?=0)，才执行第二个命令
- `com1||com2` 前一个命令执行不正确($?!=0)，才执行第二个命令


## 查看文件内容
1. cat
```bash
cat [-AbEnTv] 文件
-A: 相当于-vET 的整合，可列出一些特殊字符而不是空白
-b: 列出行号，仅针对非空白行做行号显示
-E: 将结尾的断行字符以 $ 显示出来
-n: 打印行号，连同空白行也会有行号
-T: 将[Tab]以 ^I显示出来
-v: 列出一些看不出来的特殊字符
```
2. 翻页查看more
```bash
more 文件路径
#空格键    向下翻一页
#Enter    向下滚动一行
#/字符串   在内容中向下查找字符串
#:f       显示文件名以及目前显示的行数
#q        离开more，不再显示内容
#b        向回翻页，只对文件有效
```
3. 翻页查看 less
```bash
less 文件路径
#空格键         向下翻一页
#[PageDown]    向下翻一页
#[PageUp]      向上翻一页
#/字符串   在内容中向下查找字符串
#?字符串   在内容中向上查找字符串
#n        重复前一个查询
#N        向上重复前一个查询
#q        离开
```
4. 数据选取 head
```bash
head [-n number] 文件
-n: 接数字，表示显示头几行，默认显示前10行
```
5. 数据选取tail
```bash
tail [-n number] 文件
-n: 表示显示几行
```
另外可以修改 /etc/issue文件来改变终端的提示信息
## 压缩
1. zip
```bash
zip [-AcdDfFghjJKlLmoqrSTuvVwXyz$] [-b <工作目录>] [-ll] [-n <字尾字符串>] [-t <日期时间>] [-<压缩效率>] [压缩文件名] [待压缩文件...] [-i <范本样式>] [-x <范本样式>]
-A: 调整可执行的自动解压缩文件。
-b: <工作目录> 指定暂时存放文件的目录。
-c: 替每个被压缩的文件加上注释。
**-d: 从压缩文件内删除指定的文件**
-D: 压缩文件内不建立目录名称。
-F: 尝试修复已损坏的压缩文件。
-g: 将文件压缩后附加在既有的压缩文件之后，而非另行建立新的压缩文件.
**-i <范本样式>: 只压缩符合条件的文件。**
**-x <范本样式>: 压缩时排除符合条件的文件。**
-X: 不保存额外的文件属性。
-j: 只保存文件名称及其内容，而不存放任何目录名称。
-J: 删除压缩文件前面不必要的数据。
-l: 压缩文件时，把LF字符置换成LF+CR字符。
-ll: 压缩文件时，把LF+CR字符置换成LF字符。
**-m: 将文件压缩并加入压缩文件后，删除原始文件，即把文件移到压缩文件中**
**-n<字尾字符串>: 不压缩具有特定字尾字符串的文件。**
-q: 不显示指令执行过程
**-r: 递归处理，将指定目录下的所有文件和子目录一并处理。**
-t<日期时间>: 把压缩文件的日期设成指定的日期
-y: 直接保存符号连接，而非该连接所指向的文件，本参数仅在UNIX之类的系统下有效。
## 示例
zip -r search.zip search/  ## 将search目录打包的zip文件中
zip -r -x *.css search.zip search/ ## 打包search目录，单不包含css文件
```
2. 使用zipsplit分割压缩的zip文件
```bash
zipsplit (选项) (参数)
-n: 指定分割后每个zip文件的大小，是字节大小；
-t: 报告将要产生的较小的zip文件的大小；
-b: 指定分割后的zip文件的存放位置。
```
3. tar压缩
tar参数中 -x,-c,-t不能同时出现。
```bash
# 打包
tar [-j|-z] [cv] [-f 新建的文件名] filename/dirname
# 查看文件名
tar [-j|-z] [tv] [-f 新建的文件名]
# 解压
tar [-j|-z] [xv] [-f 新建的文件名] [-C 目录]
-c: 新建打包文件
-t: 查看打包文件的内容有哪些文件名
-x: 解压功能，可以配合 C 解压到特定目录
-j: 通过bzip2来压缩/解压文件，此时文件名最好是 *.tar.bz2
-z: 通过gzip来压缩/解压文件，此时文件名最好是 *.tar.gz
-v: 在压缩/解压的过程中，将处理的文件名显示出来
-f filename: -f后接要处理的文件名
-C: 解压缩时，将文件解压到特定的目录

-p: 保留备份数据的原本权限与属性，常用于备份重要的配置文件
-P: 保留绝对路径
--exclude=FILE: 在压缩过程中，不打包FILE

## 示例
tar-zcv -f filename.tar.gz 要压缩的文件或目录名 #压缩
tar  -zxv -f filename.tar.gz -C 欲解压的目录  # 解压
tar -jcv -f system.tar.bz2 --exclude=/etc* --exclude=gz* /etc/root
```

## 访问网络内容 `wget` `curl`
`wget` 用于从网络上下载资源，若没有指定目录，默认为当前目录
```bash
wget [参数] [url地址]
    -o, –output-file=FILE: 把**记录**写到FILE文件中
    -a, –append-output=FILE: 把**记录**追加到FILE文件中
    -d, –debug: 打印调试输出
    -q, –quiet: 安静模式(没有输出)
    -v, –verbose: 冗长模式(这是缺省设置)
    -nv, –non-verbose: 关掉冗长模式，但不是安静模式
    -i, –input-file=FILE: 下载在FILE文件中出现的URLs
    -F, –force-html: 把输入文件当作HTML格式文件对待
    -B, –base=URL: 将URL作为在-F -i参数指定的文件中出现的相对链接的前缀
    –sslcertfile=FILE: 可选客户端证书
    –sslcertkey=KEYFILE: 可选客户端证书的KEYFILE
    –egd-file=FILE: 指定EGD socket的文件名
    -t, –tries=NUMBER 设定最大尝试链接次数(0 表示无限制).

    -O –output-document=FILE 把文档写到FILE文件中
    -nc, –no-clobber 不要覆盖存在的文件或使用.#前缀
    -c, –continue 接着下载没下载完的文件
    –progress=TYPE 设定进程条标记
    -N, –timestamping 不要重新下载文件除非比本地文件新
    -S, –server-response 打印服务器的回应
    –spider 不下载任何东西
    -T, –timeout=SECONDS 设定响应超时的秒数
    -w, –wait=SECONDS 两次尝试之间间隔SECONDS秒
    –waitretry=SECONDS 在重新链接之间等待1…SECONDS秒
    –random-wait 在下载之间等待0…2*WAIT秒
    -Y, –proxy=on/off 打开或关闭代理
    -Q, –quota=NUMBER 设置下载的容量限制
    –limit-rate=RATE 限定下载输率
```

## 管道命令 `|` - `cut` `grep` `sort` `uniq`
每个`|`后面接的第一个数据必须是能够接受standard input数据的命令，而且管道命令只能处理standard output，对于error output会忽略
1. `cut` 
```bash
cut –d '分隔符' –f fields
    -d : 后边接分隔符， 与-f一起使用
    -f : 依据-d的分隔符将一段信息切割成为数段，有-f取出第几段的意思
last | cut –d ' ' –f 1,2
```
2. `grep` 可用正则
```bash
grep [-cinv] [--color=auto] '查找字符串(正则)' filename
    -c : 计算找到字符串的次数
    -i : 忽略大小写
    -n : 给出行号
    -v : 反向选择
grep -–color=auto 'manpath' /etc/man.config
```
3. `sort` 排序命令
```bash
sort [-fbMnrtuk] [file or stdin]
    -f: 忽略大小写
    -b: 忽略最前面的空格
    -M: 以月份的名字排序
    -n: 使用“纯数字”进行排序（默认是文字类型）
    -r: 反向排序
    -u: 同umiq
    -t: 分隔符，默认是[tab]分割
    -k: 以哪个区间来进行排序
```
4. `uniq` 计数
```bash
uniq [-ic]
    -i: 忽略大小写字符的不同 
    -c: 进行计数
```
## 时间
`date`命令能够通过`date +Format`设置输出格式
```bash
date +Format
    - %Y : 年份
    - %y : 年份的最后两位
    - %d : 按月计的日期(例如：01)
    - %D : 按月计的日期；等于%m/%d/%y
    - %H : 小时(00-23)
    - %I : 小时(00-12)
    - %m : 月份
    - %M : 分钟
    - %S : 秒(00-60)
date +%Y%m%d%H%M%S   => 20170617194225
```
通过 `date -s` 设置时间
```bash
date -s 06/17/2017
date -s 19:42:25
```
## 磁盘与目录容量
1. `df` : 列出文件系统的整体磁盘使用量
```shell
df [-ahikHTm] [目录或文件名]
    -a : 列出所有文件系统，包括系统特有的/proc等文件系统
    -h : 以人们较易阅读的GB、MB、KB等格式显示
    -i : 不用硬盘容量，而以inode的数量来显示
    -k/m : 以MB/KB的容量显示文件系统
```
2. `du` : 评估文件系统的磁盘使用量
```shell
du [-ahskm] 文件或目录名称
    -a : 列出所有的文件与目录的容量，因为默认仅统计目录下面的文件量 
    -h : 以较易阅读的格式显示
    -s : 列出总量，不列出每个个别的目录占用量
    -S : 尚不理解
    -k/m : 以MB/KB的容量显示文件系统
```

## `lsof` (list open files)
在linux下，任何事物都以文件的形式存在，通过文件不仅可以访问常规数据，还可以访问网络连接和硬件。如TCP和UDP套接字等。系统在后台都为该应用程序分配了一个文件描述符，该文件描述符提供了大量关于这个应用程序本身的信息。
```bash
lsof
    -a : 列出打开文件存在的进程
    -c<进程名>: 列出指定进程所打开的文件
    -g : 列出GID号进程详情
    -d<文件号> : 列出占用该文件号的进程
    +d<目录> : 列出目录下被打开的文件
    +D<目录> : 递归列出目录下被打开的文件
    -n<目录> : 列出使用NFS的文件
    -i<条件> : 列出符合条件的进程。（4、6、协议、:端口、 @ip ）
    -p<进程号> : 列出指定进程号所打开的文件
    -u : 列出UID号进程详情
    -h : 显示帮助信息
    -v : 显示版本信息
```
## 其他快捷方式
1. 使用快捷键 `ctrl+r` 可以快速使用历史命令