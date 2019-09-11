# Mybatis

## Mybatis架构

![1564534019709](C:\Users\Luge\AppData\Roaming\Typora\typora-user-images\1564534019709.png)

## 配置文件

Mybatis的核心配置文件,一般起名为:SqlMapConfig.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
	<properties resource="jdbc.properties"></properties>
<!-- 	<properties>
		<property name="jbbc.username" value="root"/>
		<property name="jbbc.password" value="Gepoint"/>
	</properties> -->
	<!--别名  -->
	<typeAliases>
		<!--定义单个别名   不区分大小写-->
		<typeAlias type="com.lu.test.po.User" alias="userpojo"/>
		<!--包扫描配置  -->
		<!--默认是类全称  不区分大小写  -->
		<package name="com.lu.test.po"/>
	</typeAliases>
	<!-- 和spring整合后 environments配置将废除 -->
	<environments default="development">
		<environment id="development">
			<!-- 使用jdbc事务管理 -->
			<transactionManager type="JDBC" />
			<!-- 数据库连接池 -->
			<dataSource type="POOLED">
				<property name="driver" value="${jdbc.drivername}" />
				<property name="url" value="${jdbc.url}" />
				<property name="username" value="${jbbc.username}" />
				<property name="password" value="${jbbc.password}" />
			</dataSource>
		</environment>
	</environments>
	<mappers>
		<mapper resource="mybatis/user.xml" />
		<mapper resource="mybatis/UserMapper.xml" />
<!-- 		<mapper class="com.lu.dao.UserDao"/>
		<package name="com.lu.dao"/> -->
	</mappers>
</configuration>

```



### 1. 属性

1. 定义

```xml
方式一:
<properties>
    <property name="jbbc.username" value="root"/>
    <property name="jbbc.password" value="Gepoint"/>
</properties> 
方式二:(引入配置文件)
<properties resource="jdbc.properties"></properties>
```

2. 使用

```xml
<environments default="development">
    <environment id="development">
        <!-- 使用jdbc事务管理 -->
        <transactionManager type="JDBC" />
        <!-- 数据库连接池 -->
        <dataSource type="POOLED">
            <property name="driver" value="${jdbc.drivername}" />
            <property name="url" value="${jdbc.url}" />
            <property name="username" value="${jbbc.username}" />
            <property name="password" value="${jbbc.password}" />
        </dataSource>
    </environment>
</environments>
```



### 2. 别名

1. 系统定义的别名：对一些常用的Java类进行定义

![1564563891635](C:\Users\Luge\AppData\Roaming\Typora\typora-user-images\1564563891635.png)![1564563965664](C:\Users\Luge\AppData\Roaming\Typora\typora-user-images\1564563965664.png)

2. 用户自定义别名

   1. 配置单个别名

      ```xml
      <typeAliases>
          <typeAlias type="com.learn.po.Role" alias="role"/>
      </typeAliases>
      ```

      

   2.  包扫描配置别名

      ```xml
      <typeAliases>
          <!--包扫描配置  -->
          <!--默认是类全称  不区分大小写  -->
          <package name="com.lu.test.po"/>
      </typeAliases>
      ```

### 3. 映射

1. 方式一:引入映射文件

   ```xml
   <mappers>
       <mapper resource="mybatis/user.xml" />
       <mapper resource="mybatis/UserMapper.xml" />
   </mappers>
   ```

2. 方式二:引入类文件

   ```xml
   <mappers>
   	<mapper class="com.lu.dao.UserDao"/>
   </mappers>
   <!--
   要求:
   1. 接口文件与映射文件在同一目录下
   2. 接口文件与映射文件名称相同
   -->
   ```

3. 方式三:映射文件包扫描*重点

   ```xml
   <mappers>
   	<package name="com.lu.dao"/>
   </mappers>
   <!--
   要求:
   1. 接口文件与映射文件在同一目录下
   2. 接口文件与映射文件名称相同
   -->
   ```


## 映射文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--namespace:命名空间 用于隔离sql语句  -->
<mapper namespace="user">
	<!--id sql语句的唯一标识  parameterType入参的数据类型  resultType返回结果的数据类型 -->
	<select id="getUserById" parameterType="int" resultType="com.lu.test.po.User">
		<!--#{}为占位符-->
		select * from user where id=#{id};
	</select>
	<select id="getUserById2" parameterType="int" resultType="com.lu.test.po.User">
		<!--#{}为占位符-->
		select username from user where id=#{id};
	</select>
	<select id="getUserById3" parameterType="string" resultType="com.lu.test.po.User">
		<!--#{}为占位符-->
		<!--${value}为字符串拼接指令-->
		<!-- select username from user where username like #{name}; -->
		select * from user where username like '${value}';
	</select>
	<insert id="inserttest01" parameterType="com.lu.test.po.User">
		<!--
			selectKey 主键返回
			resultType 主键返回类型
			keyProperty user中的主键属性
			order 何时执行
		 -->
 		<selectKey resultType="int" keyProperty="id" order="AFTER">
			select last_insert_id();
		</selectKey>
<!-- 		<selectKey keyProperty="uuidname" order="BEFORE" resultType="string">
			select UUID();
		</selectKey> -->
		INSERT INTO 
		`mbatis`.`user`
			(`username`, `birthday`, `sex`, `address`) 
		VALUES 
			(#{username}, #{birthday}, #{sex}, #{address});
	</insert>
	<update id="updateuser01" parameterType="com.lu.test.po.User">
		update user 
		set username=#{username}
		where id=#{id};
	</update>
	<delete id="deleteuser01" parameterType="int">
		delete from user where id=#{id};
	</delete>
</mapper>

```

### 1. namespace

​	namespace:命名空间 用于隔离sql语句

### 2. 占位符与字符串拼接指令

1. #{} 			 占位符
2. ${value}                 拼接字符串指令

### 3. 查询语句

```xml
<select id="getUserById" parameterType="int" resultType="com.lu.test.po.User">
    <!--#{}为占位符-->
    select * from user where id=#{id};
</select>
```

#### 3.1 属性介绍

1. id				sql语句的唯一标识
2. parameterType        传入参数的类型
3. resultType                 返回结果的类型

### 4. 插入语句

```xml
<insert id="inserttest01" parameterType="com.lu.test.po.User">
    <!--
       selectKey 主键返回
       resultType 主键返回类型
       keyProperty user中的主键属性
       order 何时执行
   -->
    <selectKey resultType="int" keyProperty="id" order="AFTER">
        select last_insert_id();
    </selectKey>
    <!-- 		<selectKey keyProperty="uuidname" order="BEFORE" resultType="string">
   select UUID();
  </selectKey> -->
    INSERT INTO 
    `mbatis`.`user`
    	(`username`, `birthday`, `sex`, `address`) 
    VALUES 
   		(#{username}, #{birthday}, #{sex}, #{address});
</insert>
```

#### 4.1 属性介绍

1. id				sql语句的唯一标识
2. parameterType        传入参数的类型

#### 4.2 插入后返回主键(mysql)

```sql
<selectKey resultType="int" keyProperty="id" order="AFTER">
    select last_insert_id();
</selectKey>
```
1. selectKey                  主键返回
2. resultType		返回值类型
3. keyProperty             给类中赋值的属性名
4. order                         何时执行

#### 4.3 生成uuid(mysql)

```sql
<selectKey keyProperty="uuidname" order="BEFORE" resultType="string">
   select UUID();
</selectKey>
```

### 5. resultMap(手动映射)

```xml
<!--默认类的属性名与表属性名一致  单表查询可以默认不配 关联查询必须写全-->
<mapper namespace="com.lu.dao.OrderDao">
	<resultMap type="order" id="resultmap_order">
		<id property="id" column="id"/>
		<result property="userId" column="user_id"/>
		<result property="number" column="number"/>
		<result property="createtime" column="createtime"/>
		<result property="note" column="note"/>
	</resultMap>
	<select id="findById" parameterType="int" resultMap="resultmap_order">
		select * from `order` where id=#{id}; 
	</select>
</mapper>
```

### 6. 动态sql

```xml
<sql id="sql_findUserBySexAndName">
    *
</sql>
<select id="findByIds" parameterType="querypo" resultType="user">
    select 
   		 <include refid="sql_findUserBySexAndName"></include>
    from user 
    <where>
        <!-- 
        foreach:循环标签
        collection:要遍历的集合
        separator:分隔符
  	  -->
        <!-- 目的 输出id in {1,16,25};-->
        <foreach collection="ids" item="uid" open="id in (" separator="," close=")">
            #{uid}
        </foreach>
        <!-- id in {1,16,25}; -->
    </where>
</select>
<select id="findUserBySexAndName" resultType="user" parameterType="user">
    select
    <include refid="sql_findUserBySexAndName"></include>
    from user
    <!--<where> 标签自动补全where语句 并且去掉多余的and  -->
    <where>
        <if test="username!=null and username!=''">
            and username like '%${username}%'
        </if>
        <if test="sex!=null and sex!=''">
            and sex like '%${sex}%'
        </if>	
    </where>
</select>
```

1. sql+include

   作用:sql标签定义一个sql片段 ,在语句中用include标签添加

   语法:

   ```xml
   <sql id="sql_findUserBySexAndName">
       *
   </sql>
   ...
   <include refid="sql_findUserBySexAndName"></include>
   ...
   ```

2. if

   判断:判断正确则执行语句内的内容

   语法:

   ```xml
   <if test="username!=null and username!=''">
       and username like '%${username}%'
   </if>
   <if test="sex!=null and sex!=''">
       and sex like '%${sex}%'
   </if>	
   ```

3. where 

   作用:自动补上where 并且去掉语句中重复的and

   语法:

   ```xml
   <where>
       <if test="username!=null and username!=''">
           and username like '%${username}%'
       </if>
       <if test="sex!=null and sex!=''">
           and sex like '%${sex}%'
       </if>	
   </where>
   ```

4. foreach

   作用:循环

   语法:

   ```xml
   <!-- 
       foreach:循环标签
       collection:要遍历的集合
       separator:分隔符
   	item:循环内容封装
       -->
   <foreach collection="ids" item="uid" open="id in (" separator="," close=")">
       #{uid}
   </foreach>
   ```

   

## 抽取dao

### 普通方式

### 动态代理

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--动态代理的方式  -->
<!--规则 -->
<!-- 
	1. namespace必须是接口的全路径名
	2. 接口的方法名必须与sql的id一致
	3. 接口的入参必须与sql的parameterType一致
	4. 接口的返回值必须与sql的resultType一致
 -->
<mapper namespace="com.lu.dao.UserDao">
	<select id="getUserById" parameterType="int" resultType="com.lu.test.po.User">
		select * from user where id=#{id}; 
	</select>
	<insert id="insertUser" parameterType="com.lu.test.po.User">
		<selectKey resultType="int" keyProperty="id" order="AFTER">
			select last_insert_id();
		</selectKey>
		INSERT INTO  `mbatis`.`user`
			(`username`, `birthday`, `sex`, `address`) 
		VALUES 
			(#{username}, #{birthday}, #{sex}, #{address});
	</insert>
</mapper>

```

#### 规则

```properties
1. namespace必须是接口的全路径名
2. 接口的方法名必须与sql的id一致
3. 接口的入参必须与sql的parameterType一致
4. 接口的返回值必须与sql的resultType一致
```

