# JAVA基础

## 名词解释

1. JDK(Java Development Kit)

   ```
   	JDK中包含JRE，在JDK的安装目录下有一个名为jre的目录，里面有两个文件夹bin和lib，在这里可以认为bin里的就是jvm，lib中则是jvm工作所需要的类库，而jvm和 lib和起来就称为jre。
   	JDK是整个JAVA的核心，包括了Java运行环境JRE（Java Runtime Envirnment）、一堆Java工具（javac/java/jdb等）和Java基础的类库（即Java API 包括rt.jar）。
   ```

   ![1565073827655](C:\Users\Luge\AppData\Roaming\Typora\typora-user-images\1565073827655.png)

2. JRE(Java runtime environment)

   ```
   	JRE中包含了Java virtual machine（JVM），runtime class libraries和Java application launcher，这些是运行Java程序的必要组件。
   ```

3. JVM( java virtual machine)

   ```
   	就是我们常说的java虚拟机，它是整个java实现跨平台的最核心的部分，所有的java程序会首先被编译为.class的类文件，这种类文件可以在虚拟机上执行。
   ```

## JAVA环境配置

1. 安装JDK(默认路径)

2. 配置环境变量

   ```
   1. 右击电脑->属性->高级系统设置->环境变量->系统变量
   2. 新建JAVA_HOME 变量值为jdk的安装目录:C:\Program Files\Java\jdk1.8.0_171
   3. 设置Path 添加变量值
   	%JAVA_HOME%\bin;
   	%JAVA_HOME%\jre\bin;
   ```

## 数据类型

```properties
byte a=0;			//1字节
short b=0;			//2字节
int c=0;			//4字节
long d=0l;			//8字节
float e=0.0f;		//4字节
double f=0.0d;		//8字节
boolean g=false;	//1字节
char h='a';			//2字节
String i="字符串";
```

## 数组

### 一维数组

创建方式：

1. 动态创建

   ```java
   数据类型[] 数组名=new 数据类型[数组大小];
   int array[]=new int[10];
   ```

   

2. 静态创建

   ```java
   1. 数据类型[] 数组名={数据，数据，...}
   2. 数据类型[] 数组名=new 数据类型[]{数据，数据，...};
   int[] aarray={1,2,3};
   double[] barray=new double[]{1.0,2.0,3.0};
   ```

默认值：

```properties
int -- 0
double -- 0.0
boolean -- false
String -- null
```

### 多维数组

创建方式：

1. 动态创建

   ```java
   数据类型[][] 数组名=new 数据类型[大小][大小];
   int[][] a=new int[2][1];
   ```

2. 静态创建

   ```
   数据类型[][] 数组名={{数据，数据}，{数据，数据}，...}
   int[][] a={{1,2},{1,2}}
   ```

   

### 相关异常

1. 数组越界：ArrayIndexOutOfBoundsException
2. 空指针异常: NullPointerException

### 数组相关操作类

1. Arrays

   1. 常用方法:

      ```
      Arrays.sort(T t)  排序
      Arrays.binarySearch(T t[],Result r)  二分查找下标
      ```

      

## 方法

创建方式：

```java
访问修饰符 方法类型 返回类型 方法名（参数列表）{
	return ;
}
example:
public static void test01(int a){
    System.out.println(a);
    return;
}
```

### 方法参数传递

1. 基本数据类型传参

   ```
   值传递  形参内容修改，参数的值不变
   ```

   

2. 数组、对象传参

   ```
   地址传递 形参内容修改，参数的内容也会修改
   ```

## 类和对象

创建方式

```
修饰符 class 类名{
	属性;
	方法；
}
```

### 构造方法

在类创建时调用，初始化对象

当没有无参的构造方法时 无法通过 **new 类名();** 的方式直接创建对象

```
修饰符 类名（参数）{
	操作
}
```

### 创建对象

创建对象有四种方式

1. new

   ```java
   Student s=new Student();
   ```

2. class.newInstance

   ```java
   Class<?> class1 = User.class;
   //Class<?> class1 = Class.forName("com.lu.test02.User");
   System.out.println(class1.getName());
   Method md = class1.getMethod("getUsername");
   Object object = class1.newInstance();
   User u=(User)object; 
   u.setUsername("张三");
   String name = (String) md.invoke(u);
   ```

   

## 修饰符

### 访问修饰符

1. public		：所有包的类都可以访问
2. protected         ：同包中的类或者子类可以访问（子类在不同包中也可以访问）
3. private              ：只能在当前类中访问（子类不能访问）
4. default(缺省)    ：同包中所有的类都可以访问（子类在不同包里无法访问）

### 方法类型

1. static                 ：静态方法（静态方法只能访问静态方法，无法访问非静态方法/静态方法可以通过类名直接访问。非静态方法可以访问静态方法）
2. final                   :无法修改（final修饰的属性为常量，final修饰的类不可被继承，final修饰的方法无法被子类方法覆盖）
3. 默认（空）       ：非静态方法

## 多态

### 概述

事物存在的多种形体

### 多态的前提

1. 要有继承关系
2. 要有方法的重写
3. 要有父类引用指向子类对象

### 代码演示

```java
public class Animal {
    public int age=20;
	public void eat() {
		System.out.println("动物吃");
	}
}
public class Cat extends Animal {
    public int age=30;
	@Override
	public void eat() {
		System.out.println("猫吃");
	}
	public static void main(String[] args) {
		Animal cat=new Cat();
        System.out.println(cat.age);//结果为20
		cat.eat();//输出结果为猫吃
	}
}
```

### 多态中成员访问特点

1. 成员变量

   编译看左边,运行看左边(运行使用父类变量)  

2. 成员方法

   编译看左边,运行看右边(运行执行子类方法)

### 上/下转型

```java
Animal cat=new Cat();//向上转型
System.out.println(cat.age);//20
Cat cat2=(Cat) cat;//向下转型
System.out.println(cat.age);//30
```

## 抽象类

### 特点

1. 抽象类和抽象方法必须用abstract方法修饰
2. 抽象类不一定有抽象方法,但有抽象方法的类一定是抽象类或接口
3. 抽象类不能实例化
4. 抽象类的子类要么是抽象类,要么重写抽象类的所有方法
5. 抽象类里可以有普通方法和抽象方法
6. 抽象的类或方法不能和static,final共存

### 代码演示

```java
public abstract class Demo1 {
    int a=10;//常量
    final int b=100;//变量
    abstract void eat();
    void jump(){
        System.out.println("跳");
    }
}
public class Demo2 extends Demo1 {
    @Override
    void eat() {
        System.out.println("吃饭");
    }
    public static void main(String args[]){
        Demo2 demo2 = new Demo2();
        demo2.eat();
        demo2.jump();
    }
}
```

## 接口

### 特点

1. 接口必须用interface修饰
2. 类实现接口用implements
3. 接口可以继承接口
4. 接口不能实例化
5. 接口的成员变量必须是常量,并且为静态公共的 默认**public static final**

### 代码演示

```java
public interface Animal{
	void eat();//相当于 public abstract void eat()
	void run();
}
public class Dog implements Animal{
	public void eat(){
		System.out.println("Dog eat");
	}
	public void run(){
		System.out.println("Dog run");
	}
	public static void main(String args[]){
		Animal a=new Dog();
		a.eat();
		a.run();
	}
}
```

