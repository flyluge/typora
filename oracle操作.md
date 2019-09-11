# oracle入门

## 介绍

### 概念

#### 数据库（database）

Oracle数据库组成：数据文件(dbf或ora)、控制文件、联机文件、参数文件。

#### 实例

一个数据库可以有n个实例，一个实例后有一系列的进程和内存结构

#### 数据文件(dbf)

数据文件是数据库的物理存储单位。一个数据文件只能属于一个表空间，而一个表空间可以有多个数据文件。

不能之间输出数据文件，只能先删除表空间然后删除数据文件。

#### 表空间

表空间是对物理数据的逻辑映射。一个数据库可以在逻辑上划分为多个表空间。

#### 用户

用户是在实例下建立的。不同的实例中可以有相同的用户名。表的数据由用户放到表空间中，而表空间会把数据随机放到一个或多个数据文件中。

![1563781791904](C:\Users\Luge\AppData\Roaming\Typora\typora-user-images\1563781791904.png)

### 初始用户

1. scott![1563782371797](C:\Users\Luge\AppData\Roaming\Typora\typora-user-images\1563782371797.png)

2. hr

## 操作

### sql

结构化查询语言

1. DML(数据库操作语言)  insert、update、delete
2. DDL(数据库定义语言) create、drop
3. DCL(数据库控制语言) grant、revoke
4. DQL
5. TPL

### 创建表空间

```sql
create tablespace lugetest01
datafile 'd:\lugetest01.dbf'
size 10m
autoextend on
next 5m
```

### 创建用户

```sql
create user lugege
identified by '123456'
default tablespace lugetest01
```

### 创建表

```sql
1. create table name(
	age number(3)
	)
2. create table name2 as(select * from emp) ----不会复制列的约束
```

### 为表添加属性

```sql
alter table tablename add columnname varchar2(20) null;
```



### 删除表内容

```
delete from tablename where 条件
```

### delete与truncate的区别

truncate用于删除表中所有的行，与**delete from 表名**作用一样，但是truncate更快，使用的系统资源和事务日志资源更少。并且truncate可以重置具有自增长数据的值，truncate不会开启事务。

```sql
truncate table tabelname
```



### 查询

```sql
查询所有：select * from 表名
查询选定列：select username,password from student
取别名：select username 用户名 from student
去重复：select distinct */列名 from 表名
排序：select * from student order by bir asc/desc
```

### 进入

```
sqlplus
```

### 登录

``` 
scott/tiger@orcl as normal
sys/change_on_install@orcl as sysdba
```

1. normal 、sysdba、 sysoper的区别

   ```
   normal:普通用户
   sysdba:拥有系统最高权限的用户
   sysoper:主要用于关闭、启动数据库
   ```

   

###  创建表空间

```
create tablespace xxx datafile 'D:\xx.dbf' size 100m;
create tablespace school datafile 'd:\school.dbf' size 10m;
```

### 删除表空间

```
drop tablespace 表空间名 including contents and datafiles;
drop tablespace school incudinhg contents and datafiles;
```

### 创建用户

```
create user 用户名 identified by 123456 default tablespace 表空间名
create user lugege identified by 123456 default tablespace school
```

### 切换用户

```
conn sys/change_on_install@orcl as sysdba;（该用户为管理员）
conn scott/tiger@orcl
```

### 授权用户

```
grant connect,resource to 用户名;
grant insert to lugege;
```

### 取消用户权限

```
revoke connect on school from lugege
```

### 删除用户

```
drop user 用户名 cascade;
drop user lugege cascade;
```

### 分页查询

```
select * from (select rownum num from school) where num>2 and num<6
```

### 字符串拼接

```
select name ||'的职业是'||job from student
```

### 时间操作

1. 将字符串转换为日期

   ```sql
   to_date('2016-8-15','YYYY-MM-dd');
   ```

2. 计算日期相差天数

   ```sql
   日期1-日期2
   select to_date('2016-8-15','YYYY-MM-dd')-sysdate from dual;
   ```

3. 计算相差小时数

   ```sql
   select (to_date('2016-8-15','YYYY-MM-DD')-to_date('2016-7-15','YYYY-MM-DD'))*24 from dual;
   ```

4. 计算日期相差月数

   ```sql
   months_between(日期1,日期2);
   select months_between(to_date('2016-8-15','YYYY-MM-DD'),to_date('2016-7-15','YYYY-MM-DD')) from dual;
   ```

5. 相差年数

   ```sql
   select months_between(to_date('2018-8-16','YYYY-MM-DD'),to_date('2016-7-15','YYYY-MM-DD'))/12 from dual;
   ```

6. 格式化输出日期

   ```sql
   to_char(sysdate,'YYYY-MM-DD');
   to_char(sysdate,'YYYY"年"MM"月"DD"日"')
   ```

   

### 小数处理

1. 四舍五入

   ```sql
   round(数字,保留小数的位数)
   select round(months_between(to_date('2018-8-16','YYYY-MM-DD'),to_date('2016-7-15','YYYY-MM-DD'))/12,3) from dual;
   select round(3.14159,4) from dual; --3.1416
   ```

2. 直接截取

   ```sql
   trunc(数字,保留小数位数)
   select trunc(3.14159,4) from dual; --3.1415
   ```

3. 向上取整

   ```sql
   ceil(数字)
   select ceil(3.14) from dual; --4
   ```

4. 向下取整

   ```sql
   floor(数字)
   select floor(3.14) from dual; --3
   ```

   

dba

resource

connect

### 权限

#### 权限列表

```properties
ALTER SESSION --修改会话
CREATE CLUSTER --建立聚簇
CREATE DATABASE LINK --建立数据库链接
CREATE SEQUENCE --建立序列
CREATE SESSION --建立会话
CREATE SYNONYM --建立同义词
CREATE VIEW --建立视图
RESOURCE角色： --是授予开发人员的，能在自己的方案中创建表、序列、视图等。
CREATE CLUSTER --建立聚簇
CREATE PROCEDURE --建立过程
CREATE SEQUENCE --建立序列
CREATE TABLE --建表
CREATE TRIGGER --建立触发器
CREATE TYPE --建立类型

grant create session to zhangsan;//授予zhangsan用户创建session的权限，即登陆权限，允许用户登录数据库
grant unlimited tablespace to zhangsan;//授予zhangsan用户使用表空间的权限
grant create table to zhangsan;//授予创建表的权限
grante drop table to zhangsan;//授予删除表的权限
grant insert table to zhangsan;//插入表的权限
grant update table to zhangsan;//修改表的权限

grant select on tablename to zhangsan;//授予zhangsan用户查看指定表的权限
grant drop on tablename to zhangsan;//授予删除表的权限
grant insert on tablename to zhangsan;//授予插入的权限
grant update on tablename to zhangsan;//授予修改表的权限
grant insert(id) on tablename to zhangsan;
grant update(id) on tablename to zhangsan;//授予对指定表特定字段的插入和修改权限，注意，只能是insert和update
grant alert all table to zhangsan;//授予zhangsan用户alert任意表的权限
```

#### 操作

1. 查看当前用户

   ```sql
   select * from user_users;
   ```

2. 查看所有用户

   ```sql
   select * from all_users;
   select * from dba_users;//详细信息
   ```

3. 查看当前用户具有的所有角色

   ```sql
   select * from session_roles;//激活的角色
   select * from user_role_privs//被授予的角色
   ```

4. 获取所有用户及其角色

   ```sql
   select * from dba_role_privs;
   ```

5. 查看某个用户具有的角色

   ```sql
   select * from dba_role_privs where grantee='SCOTT';
   ```

6. 查看某个角色具有的权限

   ```sql
   select * from dba_sys_privs where grantee='CONNECT';
   ```

7. 查看所有的角色

   ```sql
   select * from dba_roles;
   ```

8. 显示当前数据库的全称

   ```sql
   select * from global_name;
   ```

   

## 数据类型

1. varchar(长度)  可变字符类型 目前支持

   ```sql
   varchar(20)
   ```

   

2. varchar2(长度) 可变字符类型

   ```sql
   varchar2(20)
   ```

3. char(长度) 固定长度类型

   ```sql
   char(15)
   ```

4. number(总长度,小数长度) 

   ```sql
   number(5,2)  --- 5.21
   ```

5. date 年月日时分秒

6. timestamp 时间戳 比date更加精确



## 约束

1. 主键约束

   ```sql
   id varchar2(20) primary key
   ```

2. 外键约束

   ```sql
   foreign key(cno) references category(cno)
   ```

3. 非空约束

   ```sql
   stuno varchar2(11) not null
   ```

4. 检查约束(在mysql中被忽略，无意义)

   ```sql
   sex varchar2(4) check(sex in ('男','女','人妖'))
   ```

5. 唯一约束

   ```sql
   stuno varchar2(11) unique
   ```

## 事务

### 隔离级别

1. mysql（默认：读已提交）

   ```properties
   1.读未提交(read uncommited)
   2.读已提交(read commited)
   3.可重复读(repeatableread)
   4.串行化(serializable)（一个事务执行完才能执行下一个事务）
   ```

2. oracle(默认：只读)

   ```properties
   1.读已提交(read commited)
   2.串行化(serializable)
   3.只读(read only)
   ```

### 保存点

```sql
--测试保存点
declare
begin
  insert into student(id) values(1);
  insert into student(id) values(2);
  insert into student(id) values(3);
  savepoint ceshi01; --保存点
  insert into student(id) values(3);
  insert into student(id) values(5);
  commit;
exception
  when others then
  rollback to  ceshi01;--如果出现异常 回滚到保存点
  commit;
end;
```



## 视图

### 概念

1. 视图是对查询结果的封装
   1. 视图来源于他本身查询的那张表，视图本身不存储任何数据
2. 能够封装复杂的查询结果
3. 屏蔽表中的细节

### 语法

```sql
create [or replace] view 视图名称 as 查询语句 [with read only]
```

### 创建视图

```sql
create view view1 as (select ename from emp);
create view view1 as (select ename from emp) with read only;--创建只读视图
```

### 通过视图查询数据

```sql
select * from view1;
```

### 通过视图修改数据

```
update view1 set ename='zhangsan' where ename='SMITH';
```

### 删除视图

```sql
drop view view1;
```



## 序列

### 名词解释

1. 序列

   *序列*是被排成一列的对象(或事件);这样每个元素不是在其他元素之前,就是在其他元素之后

2. 序列化

   *序列化* (Serialization)是将对象的状态信息转换为可以存储或传输的形式的过程。

   ### oracle序列

   #### 语法

   ```sql
   create sequence test01
   start with 0    --开始位置
   increment by 1  --每次增长大小
   maxvalue 10     --最大值
   minvalue 0      --最小值
   cycle           --循环
   cache 3         --缓存数量
   ```

   ### 主要写法

   ```sql
   create sequence test01;
   ```

   ### 操作

   1. 查看序列

      ```sql
      select test01.nextval from dual;--查看下一个
      select test01.currval from dual;--查看当前
      ```

   2. 删除序列

      ```
      drop sequence test01;
      ```

      

## 索引

### 概念

​	像字典目录一样，创建索引可以加快查询数据的速度。

### 语法

```sql
create [unique/CLUSTERED/NONCLUSTERED] index indexname
on {tablename/viewname}
[with [index_property[,...n]]]

create index testindex01
on wubaiwan(name)   --创建单个索引

create index testindex02
on wubaiwan(name,address) --创建复合索引
```

**说明**

```properties
UNIQUE: 建立唯一索引
CLUSTERED: 建立聚集索引
NONCLUSTERED: 建立非聚集索引
Index_property: 索引属性
UNIQUE索引既可以采用聚集索引结构，也可以采用非聚集索引的结构，如果不指明采用的索引结构，则SQL Server系统默认为采用非聚集索引结构
```

