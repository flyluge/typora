# SSH整合

## 1 环境配置

1. 配置Spring核心监听器

   ```xml
   <!--配置spring的核心监听器  -->
   <listener>
   	<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
   </listener>
   ```

2. 配置Spring默认的配置文件

   ```xml
   <!--配置spring的配置文件的路径  -->
   <context-param>
       <param-name>contextConfigLocation</param-name>
       <param-value>classpath:applicationContext.xml</param-value>
   </context-param>
   ```

   

## 2 Spring与Struts2整合

1. ### 导入![1555505969273](C:\Users\Luge\AppData\Roaming\Typora\typora-user-images\1555505969273.png)

2. 配置struts2的配置文件struts2.xml

   ```xml
   	<constant name="struts.devMode" value="true"></constant>
   	<package name="struts01" extends="struts-default" namespace="/">
   		<action name="StudentAction_*" class="StudentAction<!--此处为spring中配置的bean-->" method="{1}">
   			<result name="execute">index.jsp</result>
   			<result name="success">success.jsp</result>
   		</action>
   	</package>
   ```

3. 将Action类交给Spring,并注入属性

   ```xml
       <bean id="studentDao" class="com.lu.dao.impl.StudentDaoimpl"></bean>
       <bean id="studentService" class="com.lu.service.impl.StudentServiceimpl">
       	<property name="studentDao" ref="studentDao"></property>
       </bean>
   	<bean id="StudentAction" class="com.lu.web.action.StudentAction" scope="prototype">
   		<property name="studentService" ref="studentService"></property>
   	</bean>
   ```

## 3 Spring与Hibernate整合

### 3.1 配置文件

#### 3.1 .1 有hibernate.cfg.xml的方式

1. 第一步

- 配置sessionFactory

- 在applicationContext.xml文件中引入hibernate.cfg.xml

  ```xml
  <bean id="sessionFactory" class="org.springframework.orm.hibernate5.LocalSessionFactoryBean">
  		引入配置文件 
  		<property name="configLocation" value="classpath:hibernate.cfg.xml"></property>		
  </bean>
  ```

#### 3.1.2 没有hibernate.cfg.xml的方式

1. 第一步

- 配置dataSource

  ```xml
  <!--配置C3p0连接池  -->
  <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
  <property name="driverClass" value="com.mysql.cj.jdbc.Driver"></property>
  <property name="jdbcUrl" value="jdbc:mysql://localhost:3306/school?serverTimezone=UTC&amp;useUnicode=true&amp;characterEncoding=utf8&amp;useSSL=false"></property>
  <property name="user" value="root"></property>
  <property name="password" value="123456"></property>
  </bean>
  ```

- 配置sessionFactory

- 在sessionFactory bean中注入dataSource

- 在sessionFactory bean中注入sessionFactory属性（方言、显示sql...）

- 在sessionFactory bean中引入映射文件

  ```xml
  <!--spring === Hibernate  -->
  <bean id="sessionFactory" class="org.springframework.orm.hibernate5.LocalSessionFactoryBean">
      <!--注入连接池  -->
      <property name="dataSource" ref="dataSource"></property>
      <!--配置sessionFactory的属性  -->
      <property name="hibernateProperties">
          <props>
              <!--sql方言  -->
              <prop key="hibernate.dialect">org.hibernate.dialect.MySQLDialect</prop>
              <!--显示sql语句  -->
              <prop key="show_sql">true</prop>
              <!--格式化sql语句  -->
              <prop key="format_sql">true</prop>
          </props>
      </property>
      <property name="mappingResources">
          <list>
              <value>com/lu/domain/StudentVo.hbm.xml</value>
          </list>
      </property>		
  </bean>
  ```

  

