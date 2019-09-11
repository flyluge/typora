# PLSQL入门

## HelloWorld

```plsql
-- Created on 2019/7/26 by LUGE 
declare 
  -- Local variables here
  i integer;
begin
  -- Test statements here
  i:=1;
  dbms_output.put_line('hello world!');
end;
```

## 语法

```sql
declare 
	说明部分（变量、光标、例外）
begin
	语句部分
exception
	异常部分
end; 
```

## 定义变量/常量

### 语法

```plsql
变量名 类型;
i number(11,2);
name varchar2(20);
e_ename emp.ename%type;--说明：使用emp表中ename的类型
```

### 变量类型列表

1. char
2. varchar2
3. date
4. number--子类型 int/float
5. boolean 
6. long

**引用型变量**

```plsql
变量名 表中的属性%type 
p_ename project.proname%type--使用表中属性的类型   
--测试
declare 
p_name project.proname%type;
p_zone project.belongzone%type;
begin
select proname,belongzone into p_name,p_zone from project where projectno='x001';
dbms_output.put_line(p_name||'的所在地是'||p_zone);
end;
```

**记录型变量**

```plsql
变量名 表名%rowtype
p_row emp%rowtype;--使用表的一行作为类型
--测试
declare 
p_row emp%rowtype;
begin
select * into p_row from emp where empno='7369';
dbms_output.put_line(p_row.ename||'的工作是'||p_row.job);
end;
```

### 赋值

1. 定义时赋值

   ```
   i 
   ```

   

```plsql
declare 
  myname emp.ename%type;
begin
  select emp.ename into myname from emp where emp.empno='7369';
  dbms_output.put_line(myname);
end;
```

## 运算符

```
+:加 -:减 *:乘 /:除 **:乘方
```



## if语句

### 语法

```plsql
if
	条件1
then 
	语句1;
elseif 
	条件2
then
	语句3;
else 
	语句2;
end if;
```

### 例子

```plsql
declare 
  i number(5);
begin
  i:=&num;
  if i=1 
  then dbms_output.put_line('我是谁');
  elsif i=2
  then dbms_output.put_line('我在哪');
  else 
       dbms_output.put_line('我是LUGE'); 
  end if;
end;
```



## for循环

```plsql
  for i in 1..3 loop
   dbms_output.put_line('hello world!');
  end loop;
```

## while循环

```plsql
declare 
  i number(5);
begin
  i:=1;
  while i<=5 
  loop
        dbms_output.put_line('循环输出'||i);
        i:=i+1;
  end loop;
end;
```

## 游标(cursor)

```plsql
declare
 cursor pc is select * from emp; --创建一个游标
 pemp  emp%rowtype; --定义变量 类型为emp的行
begin
 open pc; --开启游标
 loop 
      fetch pc into pemp; --将一行赋值给变量
      exit when pc%notfound; --若pc为空 退出循环
      dbms_output.put_line(pemp.ename||' '||pemp.empno); --输出查询结果
 end loop;
 close pc; --关闭游标
end;
```

## 触发器

创建一个触发器

```plsql
CREATE TRIGGER `abc` BEFORE ON `borrowinfo` FOR EACH ROW BEGIN
	DECLARE n INT;
SELECT
	IFNULL(max(id),0) INTO n
FROM
	borrowinfo;
SET NEW.useguid = CONCAT('JY',lpad(n+1,4,0));
END;
```

### 存储过程

```plsql
create procedure Demo is
begin
	dbms_output.put_line('测试');
end;
--测试2
create or replace procedure Demo6(e_ename out emp%rowtype) is
begin
  select * into e_ename from emp where empno='7369';
end Demo6;
--执行
-- Created on 2019/7/29 by LUGE 
declare 
  -- Local variables here
  e_ename emp%rowtype;
begin
  -- Test statements here
  Demo6(e_ename);
  dbms_output.put_line(e_ename.empno||e_ename.ename);
end;
```

