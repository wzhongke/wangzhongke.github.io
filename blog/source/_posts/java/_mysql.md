---
title: mysql

---

# 配置MySQL
1. 运行MySQL配置向导文件： $mysql-path/bin/MySQLInstanceConfig.exe
2. 修改MySQL的配置文件： $mysql-path/my.ini
    修改编码方式：
     ```
     [mysql] 客户端配置
    default-character-set=utf8
    [mysqld]  服务器端存储配置
    character-set-server=utf8
     ```

# 启动/关闭MySQL
1. linux 启动 `service mysql start`，关闭服务 `service mysql stop`
2. Windows 启动`net start mysql`， 关闭服务 `net stop mysql`

# 登录MySQL
MySQL登录参数

参数                | 描述
:------------------| :----------------
-D --database=name | 打开指定的数据库
--delimiter=name   | 指定分隔符
-h, --host=name    | 服务器地址或域名
-p, --password[=name] | 密码
-P， --port=3306    | 端口号
--prompt=name       | 设置提示符
-u, --user=name    | 用户名
-V, --version      | 输出版本信息

```bash
# 使用 地址+端口号 登录
mysql -p -P3306 -hlocalhost
```

退出MySQL ： `exit;`  或 `quit;` 或  `\q;`

修改MySQL的提示符：

参数         | 说明
:-----------| :----------
\h | 服务器的名称
\D | 完整的日期
\u | 当前用户
\d | 当前的数据库

```bash
mysql -uroot -p --prompt 提示符
// 连接上客户端之后
mysql> prompt 提示符
```

# MySQL简单的命令
1. 查看当前服务器版本： `SELECT VERSION();`
2. 显示当前日期： `SELECT NOW();`
3. 显示当前用户： `SELECT USER();`
4. 查看所有的数据库： `SHOW DATABASES;`
5. 查看警告信息： `SHOW WARNINGS;`
6. 查看数据库表的创建信息： `SHOW CREATE db_name;`
7. 查看当前打开的数据库： `SHOW DATABASE();`
8. 查看数据库中的表: `SHOW TABLES [FROM db_name] [LIKE 'pattern' | WHERE expr];`
9. 查看数据库表的结构: `SHOW COLUMNS FROM tb_name`
10. 查看数据表的创建语句： `SHOW CREATE TABLE tb_name;`

## 创建数据库
```sql
CREATE {DATABASE | SCHEMA} [IF NOT EXISTS] db_name [DEFAULT] CHARACTER SET [=] charset_name
```
实例：
```sql
CREATE DATABASE test;
CREATE DATABASE IF NOT EXISTS test1 CHARACTER SET gbk;
```

## 修改数据库
命令格式：
```sql
ALTER {DATABASE | SCHEMA} [db_name] [DEFAULT] CHARACTER SET [=] charset_name;
```
示例：
```sql
ALTER DATABASE test2 CHARACTER SET = utf8
```

## 删除数据库
```sql
DROP {DATABASE | SCHEMA} [IF EXISTS] db_name;
```

# MySQL 数据类型
数据类型是指列、存储过程参数、表达式和局部变量的数据特征，它决定了数据的存储格式，代表了不同的信息类型。
## 整型
可以根据下表选择需要的整型类型，既能保证能存储所需数值，又能节省空间。 `UNSIGNED` 无符号值
数据类型   | 存储范围              | 字节
:--------|:---------------      |:-------------
tinyint   | 有符号值： -128到127 (-2^7 到 2^7-1) <br> 无符号值： 0到255 (0到2^8-1)   | 1字节
samllint  | 有符号值： -32768到32767 (-2^15 到 2^15-1) <br> 无符号值： 0到65535 (0到2^16-1)   | 2字节
mediumint | 有符号值： -2^23 到 2^23-1 <br> 无符号值： 0到2^24-1   | 3 字节 (3*8)
int       | 有符号值： -2^31 到 2^31 -1 <br> 无符号值： 0 到 2^32-1 | 4 字节
bigint    | 有符号值： -2^63 到 2^63 -1 <br> 无符号值： 0 到 2^64-1 | 8 字节

## 浮点型
数据类型               |  存储范围
:----------------| :-----------
float[(M,D)]     |-3.402823466E+38 到 -1.175494351E-38、0 和 1.175494351E-38 到 3.402823466E+38 <br> M 是数字的总位数，D是小数点后边的位数。如果没有M和D，那么根据硬件允许的限制来保存值。单精度浮点数精确到大概7位小数
double[(M,D)]    | -1.7976931348623157E+308 到 -2.2250738585072014E-308、 0 和 2.2250738585072014E-308 到 1.7976931348623157E+308

## 日期类型
数据类型      | 存储需求
:------------| :-----
year        | 1
time        | 3
date        | 3
datetime    | 8
timestamp   | 4

## 字符型

数据类型                  | 存储需求
:----------------------- |:----------
char(M)                  | M 个字节，定长类型， 0 <= M <= 255
varchar(M)              | L+1个字节(变长类型)，L<=M && 0 <= M <= 65536
tinytext                | L+1个字节，L < 2^8
text                    | L+2 个字节，L < 2^16
mediumtext              | L+3 个字节，L < 2^24
longtext                | L+4 个字节，L < 2^32
enum('value1', 'value2', ...) | 1或2个字节，取决于枚举值的个数 (最大65535个值)
set('value1','value2', ...) | 1,2,3,4或8个字节，取决于set成员的数目(最多64个成员)

## 创建表
```sql
CREATE TABLE [IF NOT EXISTS] table_name (
    column_name data_type,
    ...
)
```

## 插入记录 `INSERT`
`AUTO_INCREMENT` 是自动编号，且必须与主键组合使用，默认情况下，起始值为1，每次的增量为1


```sql
INSERT [INTO] ta_name [(col_name, ...)] VALUES (val, ...) [, (s_val, ...)]
-- 如果主键相同，则替换之
REPLACE [INTO] ta_name [(col_name, ...)] VALUES (val, ...) [(s_val, ...)]
```

## 查找 `SELECT`
### 简单的查找
```sql
SELECT expr, ...    FROM tb_name;
```

