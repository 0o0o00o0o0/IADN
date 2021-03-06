# MySQL入门

## 前述
+ 本文所述安装环境为ubuntu16.04LTS 
+ 所安装的MySQL版本为5.7

## 安装MySQL
```bash
apt-cache search mysql-server
apt install mysql-server-5.7
```

## 启动/停止/重启/查看进程
```bash
ps -ef|grep mysqld
sudo service mysql start/stop/restart
```
## docker下安装MySQL
```bash
docker pull mysql
# -p 3306:3306：将容器的3306端口映射到主机的3306端口
# -v /home/docker/mysql/conf/my.cnf:/etc/mysql/my.cnf 将主机/home/docker/mysql/conf/my.cnf挂载到容器的/etc/mysql/my.cnf
# -v /home/docker/mysql/logs:/logs 将主机/home/docker/mysql/logs挂载到容器的/logs
# -v /home/docker/mysql/data:/mysql_data 将主机/home/docker/mysql/data挂载到容器的/mysql_data
# -e MYSQL_ROOT_PASSWORD=123456 初始化root用户的密码
docker run -p 3306:3306 --name mysql -v /home/docker/mysql/conf/my.cnf:/etc/mysql/my.cnf -v /home/docker/mysql/logs:/logs -v /home/docker/mysql/data:/mysql_data -e MYSQL_ROOT_PASSWORD=123456 -d mysql
docker exec -it mysql bash
mysql -uroot -p
```

## 数据库连接方式

### 应用程序驱动
+ 应用程序使用驱动（connector/driver）客户端连接MySQL
+ MySQL驱动程序涵盖各种主流语言

### MySQL命令行
+ 验证命令行工具（mysql-client）是否安装成功
```bash
mysql -V
```
+ 注意事项
    + socket权限要求为：777，不要更改权限
    + 不要将密码直接输入在命令行里，存在安全风险
+ 常用命令
```bash
mysql> status # 查看版本等信息
mysql> show variables like 'port';  #查看端口号
mysql> show processlist;
mysql> quit #退出mysql命令行
```
+ Socket连接（本地连接）
```bash
mysql -S/var/run/mysqld/mysqld.sock -uroot -p #-u用户名 -p密码
mysql -u root -p # mac下连接
```
+ TCP/IP连接（远程连接）
```bash
 mysql -h127.0.0.1 -P3306 -uroot -p #-h ip地址 -P端口号 -u用户名 -p密码
```
### Gui工具
+ 常用工具有Navicat/MySQLWokBench

## SQL语言入门

### 关系型数据库的特点
+ 数据存放在表中
+ 表的每一行被称为记录
+ 表中的所有记录都有相同的字段（列）

### 创建与查看数据库
```bash
mysql> CREATE DATABASE abc;  #创建数据库abc
mysql> show databases; 
```

### SQL是什么
+ SQL是一种特殊目的的编程语言，用于关系型数据库中的标准数据存取操作
+ SQL是与数据库进行沟通的钥匙

### SQL用处
+ 创建表及表中字段
+ 对表进行增删查改操作
+ 用SQL操作数据库的一切

### SQL语句的分类

#### DDL（数据定义）
+ CREATE/DROP/ALTER TABLE #创建／删除／修改表
+ 查看当前数据库有哪些表
```bash
mysql> use test;  # 选择使用test这个数据库
mysql> show tables; 
```
+ CREATE TABLE
```bash
mysql> create table stu(   #stu 表名
        -> id int(10),  # id-字段；int-类型；10-这里指字符最大长度
        -> name varchar(20),
        -> age int(10),
        -> primary key(id)); # 通过primary key关键字定义字段的主键（每张表都需要包含一个主键，主键唯一标识一条记录，是唯一字段，不可为空，不可重复）
mysql> show create table stu; # 查看创建好的表
```
+ DROP TABLE
```bash
drop table stu; # 删除表
```
+ ALTER TABLE
```bash
mysql> alter table stu add column gender varchar(20);  # 新增字段gender
mysql> alter table stu modify column gender varchar(10);  # 修改字段gender
mysql> alter table stu drop column gender;  # 删除字段gender
mysql> alter table stu change notrName noteName varchar(100); #将字段notrName更改为noteName
```

#### DML（数据操作）
+ 向表中插入／修改／删除记录（select--根据条件从表中查询记录）
+ SELECT FROM TABLE
```bash
mysql> selece * from stu;  # 查询表所有字段
mysql> select name,age from stu; # 查询表指定字段
mysql> select name,age from stu where id=1; # 查询表id=1的指定字段
```
+ INSERT INTO TABLE
```bash
mysql> insert into stu(id,name) values(1,'John');
mysql> insert into stu values(2,'Lucy',20); # 插入完整字段
```
+ UPDATE TABLE SET
```bash
update stu set age=29 where id=1; # 将id=1的所在的记录的字段age改为29
```
+ DELETE FROM TABLE
```bash
delete from stu where id=1; # 将id=1的所在的记录删除
```

#### DCL（权限管理）
+ 控制数据库的访问权限等设置
+ GRANT
+ REVOKE

#### TCL（事务控制）
+ 控制事务进展
+ COMMIT
+ ROLLBACK

## Tips
+ 解决中文乱码问题（创建数据库时编码设置为utf-8）
```bash
CREATE DATABASE `ianote_schema` CHARACTER SET 'utf8' COLLATE 'utf8_general_ci';
```
