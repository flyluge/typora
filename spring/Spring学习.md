





# Spring学习

## 1 Spring开发包

### 1.1 包介绍

![jar01](C:\Users\Luge\Desktop\Typora\spring\img\jar01.jpg)

1. docs		:Spring开发的规范和api
2. libs 		 :Spring开发的jar包
3. schema          :Spring配置文件的约束



### 1.2 Spring运行环境

![jar02](C:\Users\Luge\Desktop\Typora\spring\img\jar02.jpg)

Spring的核心jar包为Beans,Core,Context,SpEL

同时需要导入![1554381776260](C:\Users\Luge\AppData\Roaming\Typora\typora-user-images\1554381776260.png)

### 1.3 spring IOC全部包

![1554381796590](C:\Users\Luge\AppData\Roaming\Typora\typora-user-images\1554381796590.png)

## 2 IOC和DI

### 2.1 IOC(inversion of control)

控制反转： IOC是一种设计思想，在spring中，IOC思想是将类的创建权交给了spring

### 2.2 DI(Dependency Injection)

依赖注入： DI其实就是IOC的另外一种说法

## 3 工厂类

### 3.1 BeanFactory

- 老版本工厂

### 3.2 ApplicationContext

- 新版本工厂

1. 获取方式

   ```java
   ApplicationContext applicationContext=new ClassPathXMlApplicationContext("xxx.xml","xxx2.xml");
   ```

2. 主要用法

   ```java
   ApplicationContext applicationContext=new ClassPathXMlApplicationContext("xxx.xml");
   User user=(User)applicationContext.getBean("User");
   ```

- ApplicationContext有两个实现类

  1. ClassPathXmlApplicationContext :加载类路径下的xml配置文件
  2. FileSystemXmlApplicationContext ：加载系统路径下的配置文件

  

### 3.3 BeanFactory与ApplicationContext的区别

```
BeanFactory ：在使用getBean时候才会创建类的实例
Application : 在加载applicationContext.xml配置文件就会创建类的实例
```

## 4 配置文件

### 4.1 applicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
    	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    	xmlns:context="http://www.springframework.org/schema/context"
    	xmlns:aop="http://www.springframework.org/schema/aop"
    	xmlns:tx="http://www.springframework.org/schema/tx"
    	xsi:schemaLocation="http://www.springframework.org/schema/beans 
    	http://www.springframework.org/schema/beans/spring-beans.xsd
    	http://www.springframework.org/schema/context
    	http://www.springframework.org/schema/context/spring-context.xsd
    	http://www.springframework.org/schema/aop
    	http://www.springframework.org/schema/aop/spring-aop.xsd
    	http://www.springframework.org/schema/tx
    	http://www.springframework.org/schema/tx/spring-tx.xsd">
	<bean id="User" class="com.lu.testSpring.User">
    	<property name="name" value="路哥"></property>
        <property name="book" ref="book01"></property>
    </bean>
    <bean id="book01" class="com.lu.book">
    	<property name="name" value="遮天"></property>
        <property name="price" ref="520.5"></property>
    </bean>
    <!--引入其他文件-->
    <import resource="applicationContext2.xml"></import>
</beans>
```

### 4.2 bean属性

| 属性名      | 介绍                                                   |                          |
| ----------- | ------------------------------------------------------ | ------------------------ |
| id          | 使用了约束中的唯一约束，不能有特殊字符，用来标识一个类 | id="User"                |
| class       | 类的全路径                                             | class="com.lu.User"      |
| name        | 与id作用相同，可以使用特殊字符                         | name="User__"            |
| init-method | 设置初始化方法                                         | init-method="init"       |
| destory     | 设置类销毁时执行的方法                                 | destory-method="destory" |

### 4.3 属性注入

#### 4.3.1 普通方式注入

1. 普通属性注入

   ```xml
   <property name="name" value="遮天"></property>
   ```

   

2. 对象注入

   ```xml
   <property name="name" ref="遮天"></property>
   ```

   

3. 集合注入

   ```xml
   <property name="publisher">
   	<list>
   		<value>路哥出版社</value>
   		<value>路哥出版社2</value>
   	</list>
   </property>
   ```

   

4. list注入

   ```xml
   <property name="list">
   		<list>
   			<ref bean="book01"></ref>
   			<ref bean="book02"></ref>
   		</list>
   </property>
   ```

   

5. map注入

   ```xml
   <property name="students">
   	<map>
   		<entry key="stu1" value-ref="student1" />
   		<entry key="stu2" value-ref="student2" />
   		<entry key="stu3" value-ref="student3" />
   	</map>
   </property>
   
   ```

#### 4.3.2 SpEL方式注入属性

1. 第一步需要修改约束文件![1554385643613](C:\Users\Luge\AppData\Roaming\Typora\typora-user-images\1554385643613.png)

2. 属性注入时直接使用name_value形式

3. 配置代码如下

   ```xml
   <bean id="User2" class="com.lu.testSpring.User">
       <property name="name" value="#{'路哥哥呀'}"></property>
       <property name="password" value="#{'123456'}"></property>
       <property name="book" value="#{book01}"></property>
       <property name="list">
           <list>
               <ref bean="book01"></ref>
               <ref bean="book02"></ref>
           </list>
       </property>
   </bean>
   ```

   

#### 4.3.3 使用注解的方式完成属性注入

1. 第一步 在配置文件中加入如下内容

   ```xml
    <context:component-scan base-package="com.lu.test2"></context:component-scan>
   ```

2. 第二步

   - User

     ```java
     @Component("User") //<bean id="User" class="" init-method="" destory=""/>
     @Scope("prototype")
     public class User {
     	@Value("尼古拉斯。王。富贵")
     	private String name;
     	@Value("28") //<property name="" value=""/>
     	private Integer age;
     	@Autowired
     	@Qualifier(value="Book")//<property name="" ref="">
     	private Book book;
     	@PostConstruct//相当于init-method
     	public void init() {
     		System.out.println("初始化");
     	}
     	@PreDestroy//相当于destory-method
     	public void destory() {
     		System.out.println("销毁");
     	}
     	public String getName() {
     		return name;
     	}
     	public void setName(String name) {
     		this.name = name;
     	}
     	public Integer getAge() {
     		return age;
     	}
     	public void setAge(Integer age) {
     		this.age = age;
     	}
     	public Book getBook() {
     		return book;
     	}
     	public void setBook(Book book) {
     		this.book = book;
     	}
     	public void read() {
     		System.out.println("读书");
     	}
     	@Override
     	public String toString() {
     		return "User [name=" + name + ", age=" + age + ", book=" + book + "]";
     	}
     }
     ```

   - Book

     ```java
     @Component("Book")
     public class Book {
     	@Value("哈哈哈")
     	private String name;
     	private double price;
     	public String getName() {
     		return name;
     	}
     	public void setName(String name) {
     		this.name = name;
     	}
     	public double getPrice() {
     		return price;
     	}
     	public void setPrice(double price) {
     		this.price = price;
     	}
     	@Override
     	public String toString() {
     		return "Book [name=" + name + ", price=" + price + "]";
     	}
     }
     ```

     

   - 注解详解

     | 注解                                                    | 作用         | 相当于                                               |
     | ------------------------------------------------------- | ------------ | ---------------------------------------------------- |
     | @Component<br>衍生类:@Repository、@Service、@Controller | 定义Bean     | <bean id="User" class="" init-method="" destory=""/> |
     | @Value                                                  | 定义属性     | <property name="" value=""/>                         |
     | @Autowired
		<br/>	@Qualifier(value="Book")             | 定义对象属性 | <property name="" ref="">                            |
     | @PreDestroy                                             | 销毁方法     | destory-method                                       |
     | @PostConstruct                                          | 初始化方法   | init-method                                          |

   - Bean范围注解 

   1. singleton :默认单例
   2. prototype:多例
   3. request
   4. session
   5. globalsession

## 5 AOP

### 5.1 AOP简介

- AOP是Spring框架面向切面的编程思想，AOP采用一种称为“横切”的技术，将涉及多业务流程的通用功能抽取并单独封装，形成独立的切面，在合适的时机将这些切面横向切入到业务流程指定的位置中。

- Spring AOP建立在Java动态代理的基础上
- 包
  - ![1554822790894](C:\Users\Luge\AppData\Roaming\Typora\typora-user-images\1554822790894.png)

### 5.2 AOP相关概念

1. JoinPoint 连接点，需要被增强的方法
2. PointCut 切入点， 正真被增强的方法
3. Advice 通知、增强  增强的方法
4. Introduction 引介：类层面的增强
5. Target 目标类、被增强的对象
6. Weaving 织入：将通知应用到目标的过程
7. Proxy 代理对象
8. Aspect 切面、方面

### 5.3 底层实现

- Loginimpl

```java
public class Loginimpl implements Login{
	public void login() {
		System.out.println("在这里实现登陆");
	}
}
```

- 代理类

```java
public class LoginProxy implements InvocationHandler {
	private Object obj;
	LoginProxy(Object object){
		obj=object;
	}
	public static Object bind(Object obj) {
		Class<? extends Object> clazz=obj.getClass();
		return Proxy.newProxyInstance(clazz.getClassLoader(), clazz.getInterfaces(), new LoginProxy(obj));
	}
	@Override
	public Object invoke(Object proxy, Method method, Object[] arg2) throws Throwable {
		// TODO Auto-generated method stub
		before();
		Object object=method.invoke(obj, arg2);
		after();
		return object;
	}
	public void logging(String msg) {
		Logger logger=Logger.getLogger(obj.getClass().getName());
		logger.info(msg);
	}
	public void before() {
		logging("输出前置信息");
	}
	public void after() {
		logging("输出后置信息");
	}
}
```

- 使用代理类代理login对象并调用增强方法

```java
    public void test02(){
        Login login=(Login)LoginProxy.bind(new Loginimpl());
        login.login();
    }
```

### 5.4 AOP通知类型

| 通知类型 | 接口                  | 已实现接口           |
| -------- | --------------------- | -------------------- |
| 前置通知 | BeforeAdvice          | MethodBeforeAdvice   |
| 后置通知 | AfterReturnningAcvice | AfterReturningAdvice |
| 环绕通知 | MethodTnterceptor     | MethodInterceptor    |
| 抛出通知 | ThrowAdvice           | ThrowAdvice          |

### 5.5 AOP使用方式

#### 5.5.1 ProxyFactory

- 需要增强的类 

  ```java
  public class UserLogin {
  	@Test
  	public void login() {
  		System.out.println("用户登陆操作=================");
  	}
  }
  ```

- 增强类(实现通知接口)

  ```java
  public class UserLoginProxy implements MethodBeforeAdvice,AfterReturningAdvice,MethodInterceptor{
  	@Override
  	public void before(Method arg0, Object[] arg1, Object arg2) throws Throwable {
  		// TODO Auto-generated method stub
  		System.out.println("登陆之前执行的操作++++++++++++++");
  	}
  	@Override
  	public void afterReturning(Object arg0, Method arg1, Object[] arg2, Object arg3) throws Throwable {
  		// TODO Auto-generated method stub
  		System.out.println("登陆之后执行的操作+++++++++++++");
  	}
  	@Override
  	public Object invoke(MethodInvocation arg0) throws Throwable {
  		// TODO Auto-generated method stub
  		System.out.println("1111111111");
  		arg0.proceed();
  		System.out.println("2222222222");
  		return null;
  	}
  }
  ```

- 使用ProxyFactory获取代理的增强类

  ```java
  	@Test
  	public void test01() {
  		UserLogin userLogin=new UserLogin();
  		BeforeAdvice beforeAdvice=new UserLoginProxy();
  		ProxyFactory pf=new ProxyFactory();
  		pf.setTarget(userLogin);
  		pf.addAdvice(beforeAdvice);
  		UserLogin userLogin2=(UserLogin) pf.getProxy();
  		userLogin2.login();
  	}
  ```

  

#### 5.5.2 XML

- xml文件

  ```xml
  <!--配置接入点-->
  <bean id="Login" class="com.lu.aoptest.Loginimpl"/>
  <bean id="UserLoginProxy2" class="com.lu.test.UserLoginProxy2"/>
  <!--aop-->
  <aop:config>
      <!--配置切入点-->
      <aop:pointcut expression="execution(* com.lu.test.UserLogin.login(..))" id="pointcut1"/>
      <!--配置切面-->
      <aop:aspect ref="UserLoginProxy2">
          <aop:before method="before" pointcut-ref="pointcut1"/>
          <aop:after-returning method="after" pointcut-ref="pointcut1"/>
          <aop:around method="interceptor" pointcut-ref="pointcut1"/>
      </aop:aspect>
  </aop:config>
  ```

- 切面类（通知类）

  ```java
  public class UserLoginProxy2 {
  	public void before() {
  		System.out.println("前置通知");
  	}
  	public void after() {
  		System.out.println("后置通知");
  	}
  	//环绕通知
  	public void interceptor(ProceedingJoinPoint joinPoint) throws Throwable {
  		System.out.println("前");
  		joinPoint.proceed();
  		System.out.println("后");
  	}
  }
  ```

- 使用方式

  ```java
  @Test
  public void test02() {
      ApplicationContext applicationContext=new 		      ClassPathXmlApplicationContext("applicationContext.xml");
      UserLogin userLogin=(UserLogin) applicationContext.getBean("UserLogin");
      userLogin.login();
  }
  ```

#### 5.5.3 注解的方式

1. xml的注解

## 6 Spring 模板

### 6.1 DataSource

1. JNDI（在支持JNDI的环境中，如tomcat、weblogic）

2. DriverManagerDataSource（Spring提供的轻量级的接口）

   ```xml
   <bean id="datasource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
       <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"></property>
       <property name="url" value="jdbc:mysql://localhost:3306/school?serverTimezone=UTC&amp;useUnicode=true&amp;characterEncoding=utf8&amp;useSSL=false"></property>
       <property name="username" value="root"></property>
       <property name="password" value="123456"></property>
   </bean>
   ```

   ```java
   ApplicationContext application=new ClassPathXmlApplicationContext("applicationContext.xml");
   		DriverManagerDataSource dataSource=(DriverManagerDataSource) application.getBean("datasource");
   		JdbcTemplate jdbcTemplate=new JdbcTemplate(dataSource);
   		jdbcTemplate.update("update student set stuname=? where stuid=?","亚索","003");
   ```

   对应java

   ```java
   DriverManagerDataSource dataSource=new DriverManagerDataSource();
   		dataSource.setDriverClassName("com.mysql.cj.jdbc.Driver");
   		dataSource.setUrl("jdbc:mysql://localhost:3306/school?serverTimezone=UTC&useUnicode=true&characterEncoding=utf8&useSSL=false");
   		dataSource.setUsername("root");
   		dataSource.setPassword("123456");
   		JdbcTemplate jdbctemplate=new JdbcTemplate(dataSource);
   		jdbctemplate.update("update student set stuname=? where stuid=?","盖伦","001");
   ```

   

3. 数据库连接池

- C3P0

  ```xml
  <bean id="datasource2" class="com.mchange.v2.c3p0.ComboPooledDataSource">
      <property name="driverClass" value="com.mysql.cj.jdbc.Driver"></property>
      <property name="jdbcUrl" value="jdbc:mysql://localhost:3306/school?serverTimezone=UTC&amp;useUnicode=true&amp;characterEncoding=utf8&amp;useSSL=false"></property>
      <property name="user" value="root"></property>
      <property name="password" value="123456"></property>
  </bean>
  ```

  ```java
  ApplicationContext application=new ClassPathXmlApplicationContext("applicationContext.xml");
  		ComboPooledDataSource dataSource=(ComboPooledDataSource) application.getBean("datasource2");
  		JdbcTemplate jdbcTemplate=new JdbcTemplate(dataSource);
  		jdbcTemplate.update("update student set stuname=? where stuid=?","加里奥","003");
  ```

  对应的java

  ```java
  ComboPooledDataSource dataSource=new ComboPooledDataSource();
  		try {
  			dataSource.setDriverClass(driver);
  			dataSource.setJdbcUrl(url);
  			dataSource.setUser(username);
  			dataSource.setPassword(password);
  		} catch (PropertyVetoException e) {
  			// TODO Auto-generated catch block
  			e.printStackTrace();
  		}
  		JdbcTemplate jdbcTemplate=new JdbcTemplate(dataSource);
  		jdbcTemplate.update("update student set stuname=? where stuid=?","提莫","004");
  ```

- DBCP

  ```xml
  <bean id="datasource3" class="org.apache.commons.dbcp2.BasicDataSource">
      <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"></property>
      <property name="url" value="jdbc:mysql://localhost:3306/school?serverTimezone=UTC&amp;useUnicode=true&amp;characterEncoding=utf8&amp;useSSL=false"></property>
      <property name="username" value="root"></property>
      <property name="password" value="123456"></property>
  </bean>
  ```

  ```java
  		ApplicationContext application=new ClassPathXmlApplicationContext("applicationContext.xml");
  		BasicDataSource dataSource=(BasicDataSource) application.getBean("datasource3");
  		JdbcTemplate jdbcTemplate=new JdbcTemplate(dataSource);
  		jdbcTemplate.update("update student set stuname=? where stuid=?","泰达米尔","003");
  ```

  对应的java

  ```java
  		BasicDataSource dataSource=new BasicDataSource();
  		dataSource.setDriverClassName(driver);
  		dataSource.setUrl(url);
  		dataSource.setUsername(username);
  		dataSource.setPassword(password);
  		JdbcTemplate jdbcTemplate=new JdbcTemplate(dataSource);
  		jdbcTemplate.update("update student set stuname=? where stuid=?","卢锡安","003");
  ```

### 6.2 JdbcTemplate的使用

1. CRUD

   - 插入

     ```java
     JdbcTemplate jdbcTemplate=new JdbcTemplate(dataSource);
     jdbcTemplate.update("insert into user values(?,?,?,?)","001","盖伦","30","坦克");
     ```

     

   - 更新

     ```java
     JdbcTemplate jdbcTemplate=new JdbcTemplate(dataSource);
     jdbcTemplate.update("update student set stuname=? where stuid=?","卢锡安","003");
     ```

     

   - 删除

     ```java
     JdbcTemplate jdbcTemplate=new JdbcTemplate(dataSource);
     jdbcTemplate.update("delete from user where userid=?",003");
     ```

2. 查询

   - 查询单条信息（jdbcTemplate.queryForObject）

     ```java
     //查询数量
     Long count=jdbcTemplate.queryForObject("select count(*) from student",Long.class);
     //查询姓名
     String name=jdbcTemplate.queryForObject("select stuname from student where stuid=?",String.class,"003");
     ```

     

   - 查询结果封装到对象

     ```java
     Student student=jdbcTemplate.queryForObject("select * from student where stuid=?",new RowMapper<Student>() {
     			@Override
     			public Student mapRow(ResultSet rs, int rowNum) throws SQLException {
     				// TODO Auto-generated method stub
     				Student s=new Student();
     				try {
     					s.setStuID(rs.getString("stuid"));
     					s.setStuName(rs.getString("stuName"));
     					DateFormat dateFormat = new SimpleDateFormat("yyyy-MM-dd");
     					s.setBirthday(dateFormat.parse(rs.getString("birthday")));
     					s.setMessage(rs.getString("message"));
     					s.setSex(rs.getString("sex"));
     				} catch (ParseException e) {
     					e.printStackTrace();
     				}
     				return s;
     			}
     		},"001");
     ```

     

   - 查询对象列表

     ```java
     		List<Student> ss = jdbcTemplate.query("select * from student",new RowMapper<Student>() {
     			@Override
     			public Student mapRow(ResultSet rs, int rowNum) throws SQLException {
     				// TODO Auto-generated method stub
     				Student s=new Student();
     				try {
     					s.setStuID(rs.getString("stuid"));
     					s.setStuName(rs.getString("stuName"));
     					DateFormat dateFormat = new SimpleDateFormat("yyyy-MM-dd");
     					s.setBirthday(dateFormat.parse(rs.getString("birthday")));
     					s.setMessage(rs.getString("message"));
     					s.setSex(rs.getString("sex"));
     				} catch (ParseException e) {
     					e.printStackTrace();
     				}
     				return s;
     			}
     		} );
     		for(Student sts:ss) {
     			System.out.println(ss);
     		}
     ```

     

## 7 Spring事务管理

### 7.1 Spring事务管理

#### 7.1.1 PaltformTransactionManager:平台事务管理器

- 平台事务管理器，接口，是Spring用于管理事务的真正对象
  - DataSourceTransactionManager ：底层使用JDBC事务管理
  - HibernateTransactionManager    ：底层使用hibernate事务管理

#### 7.1.2 TransactionDefinition: 事务定义信息

- 事务定义： 隔离级别、超时信息、传播行为、是否只读
- Spring事务的传播行为
  - 保证多个操作在一个事务里
    - `PROPAGATION_REQUIRED` :**默认值** 一个事务外层还有一个事务，则此事务使用外层的事务。若外层无事务，创建一个新的事务
    - `PROPAGATION_NOT_SUPPORTED`：一个事务外层还有一个事务，则此事务使用外层的事务。若外层无事务，不使用事务
    - `PROPAGATION_MANDATORY` ：一个事务外层还有一个事务，则此事务使用外层的事务。若外层无事务，抛出异常
  - 保证多个操作不在同一个事务里
    - `PROPAGATION_REQUIRES_NEW` ： 一个事务外层还有一个事务，将外层事务挂起，自身重新创建一个新的事务
    - `PROPAGATION_NOT_SUPPORTED` ： 一个事务外层还有一个事务，将外层事务挂起，自身不适用事务
    - `PROPAGATION_NEVER` ： 一个事务外层还有一个事务，抛出异常
  - 嵌套式事务
    - `PROPAGATION_NESTED`： 一个事务外层还有一个事务，按照外层的事务执行，执行完后，设置一个保存点，执行内层操作，如果没有异常，执行通过。有异常则可以选择回滚到初始位置或回滚到保存点

#### 7.1.3 TransactionStatus: 事务的状态

- 事务状态：用于记录事务管理过程中事务状态的对象

## 7.2 事务模板 transactionTemplate

### 7.2.1 编程式事务管理

1. 第一步 配置平台事务管理器

   ```xml
   <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
   <property name="dataSource" ref="dataSource"></property>
   </bean>
   ```

2. 第二步 配置transactionTemplate模板

   ```xml
   <bean id="transactionTemplate" class="org.springframework.transaction.support.TransactionTemplate">
   <property name="transactionManager" ref="transactionManager"></property>
   </bean>
   ```

3. 获取模板，使用事务

   ```java
   transactionTemplate.execute(new TransactionCallbackWithoutResult(){
       //需要使用事务的操作
   });
   ```

   

### 7.2.2 声明式事务管理

1. 第一步 配置平台管理器

   ```xml
   <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
   		<property name="dataSource" ref="dataSource"></property>
   	</bean>
   ```

2. 第二步 配置事务的增强

   ```xml
   <tx:advice id="txAdvice" transaction-manager="transactionManager">
       <tx:attributes>
           <tx:method name="*" propagation="REQUIRED"/>
       </tx:attributes>
   </tx:advice>
   ```

3. 第三步 配置切入点和切面

   ```xml
   <aop:config>
       <aop:pointcut expression="execution(* com.lu.service.impl.StudentServiceimpl.*(..))" id="pointcut1"/>
       <aop:advisor advice-ref="txAdvice" pointcut-ref="pointcut1"/>
   </aop:config>
   ```

### 7.2.3 注解的方式配置事务

1. 第一步 配置事务平台管理器

   ```xml
   <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
   		<property name="dataSource" ref="dataSource"></property>
   	</bean>
   ```

2. 第二步 开启注解事务

   ```xml
   	<tx:annotation-driven transaction-manager="transactionManager"/>
   ```

3. 在业务层添加注解

   ```java
   @Transactional
   public class StudentServiceimpl implements StudentService{
   ```

   



