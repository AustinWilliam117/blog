---
title: PostgreSQL使用
date: 2021-01-16 19:17:55
tags: ["PostgreSQL"]
categories: ["SQL"]
---

## 安装PostgreSQL

> sudo pacman -S postgresql

在PostgreSQL可以正确使用之前，数据库集群必须被初始化:

```
# sudo su - postgres -c "initdb --locale en_US.UTF-8 -E UTF8 -D '/var/lib/postgres/data'"
```

启动PostgreSQL，(可选)，添加 PostgreSQL 到daemons列表里作为守护进程同时启动：

```
# systemctl enable postgresql.service
# systemctl start postgresql.service
```

<!--more-->

**注意：** 在本篇文章中需要以postgres用户运行的命令以`[postgres]$`作为前置符号。你可以以root用户执行`su - postgres`登陆postgres用户。如果你使用[sudo](https://wiki.archlinux.org/index.php/Sudo)，可以以普通用户执行`sudo -i -u postgres`。

## 创建第一个数据库/用户

如果创建一个与你的 Arch 用户 ($USER) 同名的数据库用户，并允许其访问 PostgreSQL 数据库的 shell，那么在使用PostgreSQL 数据库 shell 的时候无需指定用户登录（这样做会比较方便）。

以 postgres 用户身份, 添加一个新的数据库用户使用 [createuser](http://www.postgresql.org/docs/9.0/static/app-createuser.html) 命令

```
[postgres]$ createuser --interactive
输入要增加的角色名称: 我登录 Arch 的用户名
```

以具备读写权限的用户身份，创建一个新的数据库,使用[createdb](http://www.postgresql.org/docs/9.0/static/app-createdb.html) 命令。

从你的 shell (**不是** 以 postrgres 用户的身份)

```
$ createdb myDatabaseName
```



## PostgreSQL 修改密码

### 1. 修改PostgreSQL数据库默认用户postgres的密码

PostgreSQL数据库创建一个postgres用户作为数据库的管理员，密码随机，所以需要修改密码，方式如下：

步骤一：登录PostgreSQL

```
sudo` `-u postgres psql
```

步骤二：修改登录PostgreSQL密码

```
ALTER USER postgres WITH PASSWORD ``'postgres'``;
```

**注：**

- 密码postgres要用引号引起来
- 命令最后有分号

步骤三：退出PostgreSQL客户端

```
\q
```

### 2. 修改linux系统postgres用户的密码

PostgreSQL会创建一个默认的linux用户postgres，修改该用户密码的方法如下：

步骤一：删除用户postgres的密码

```
sudo` `passwd` `-d postgres
```

步骤二：设置用户postgres的密码

```
sudo` `-u postgres ``passwd
```

系统提示输入新的密码

```
Enter new UNIX password:``Retype new UNIX password:``passwd``: password updated successfully
```



## PostgreSQL命令

查看数据库

```sql
psql -l
```

查看数据库版本信息

```sql
psql --version
```

建立/删除数据库

```sql
createdb database
dropdb database
```

进入数据库

```sql
psql database
```

建一个叫cats的表：

```sql
test=# create table posts(name varchar(255), content text);
CREATE TABLE
```

预览表：

```
\dt
```

查看表的详细信息：

```shell
\d tablename
```

 查询：

```sql
komablog=# select * from posts;
 title | content 
-------+---------
(0 行记录)
```

插入数据：

```sql
komablog=# insert into posts values('leopard', 90);
INSERT 0 1
```

导入sql语句：

```shell
$ vim db.sql
...
create table posts (title varchar(255), content text);
...
$ psql komablog
> \i db.sql	#导入sql语句
> \dt
```

退出，帮助分别是：\h \q

## PostgreSQL插件——pgcli

### Quick Start

If you already know how to install python packages, then you can simply do:

```shell
$ pip install -U pgcli

or

$ sudo apt-get install pgcli # Only on Debian based Linux (e.g. Ubuntu, Mint, etc)
$ brew install pgcli  # Only on macOS
```

### Usage

```shell
$ pgcli [database_name]

or

$ pgcli postgresql://[user[:password]@][netloc][:port][/dbname][?extra=value[&other=other-value]]
```

Examples:

```shell
$ pgcli local_database

$ pgcli postgres://amjith:pa$$w0rd@example.com:5432/app_db?sslmode=verify-ca&sslrootcert=/myrootcert
```

For more details:

```shell
$ pgcli --help

Usage: pgcli [OPTIONS] [DBNAME] [USERNAME]

Options:
  -h, --host TEXT         Host address of the postgres database.
  -p, --port INTEGER      Port number at which the postgres instance is
                          listening.
  -U, --username TEXT     Username to connect to the postgres database.
  -u, --user TEXT         Username to connect to the postgres database.
  -W, --password          Force password prompt.
  -w, --no-password       Never prompt for password.
  --single-connection     Do not use a separate connection for completions.
  -v, --version           Version of pgcli.
  -d, --dbname TEXT       database name to connect to.
  --pgclirc PATH          Location of pgclirc file.
  -D, --dsn TEXT          Use DSN configured into the [alias_dsn] section of
                          pgclirc file.
  --list-dsn              list of DSN configured into the [alias_dsn] section
                          of pgclirc file.
  --row-limit INTEGER     Set threshold for row limit prompt. Use 0 to disable
                          prompt.
  --less-chatty           Skip intro on startup and goodbye on exit.
  --prompt TEXT           Prompt format (Default: "\u@\h:\d> ").
  --prompt-dsn TEXT       Prompt format for connections using DSN aliases
                          (Default: "\u@\h:\d> ").
  -l, --list              list available databases, then exit.
  --auto-vertical-output  Automatically switch to vertical output mode if the
                          result is wider than the terminal width.
  --warn / --no-warn      Warn before running a destructive query.
  --help                  Show this message and exit.
```

[官方文档](https://github.com/dbcli/pgcli)


## 基础数据类型（常用）

* 数值型

  | 名字    | 长度   | 描述       | 范围                       |
  | ------- | ------ | ---------- | -------------------------- |
  | integer | 4 字节 | 常用的整数 | -2147483648 到 +2147483647 |
  | real    | 4 字节 | 浮点型     | 6 位十进制数字精度         |
  | serial  | 4 字节 | 序列型     | 1 到 2147483647            |

* 文字型

  | 名字    | 描述             |
  | ------- | ---------------- |
  | char    | 定长,不足补空白  |
  | varchar | 变长，有长度限制 |
  | text    | 变长，无长度限制 |

* 布尔型

  | 名字    | 存储格式 | 描述       |
  | ------- | -------- | ---------- |
  | boolean | 1 字节   | true/false |

* 日期型

  | 名字      | 长度   | 描述         |
  | --------- | ------ | ------------ |
  | date      | 4 字节 | 年月日       |
  | time      | 8 字节 | 时分秒       |
  | timestamp | 8 字节 | 年月日时分秒 |

* 特色型

  | 名字               | 描述                                                         |
  | ------------------ | ------------------------------------------------------------ |
  | Array              | 数组类型可以是任何基本类型或用户定义类型，枚举类型或复合类型。 |
  | 网络地址型（inet） |                                                              |
  | JSON型             | 用来存储 JSON数据                                            |
  | XML型              | 以存储由XML标准定义的格式良好的"文档"                        |

[PostgreSQL (简体中文)](https://wiki.archlinux.org/index.php/PostgreSQL_%28%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87%29)

