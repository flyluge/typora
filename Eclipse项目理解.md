# Eclipse项目理解

## Eclipse创建项目后产生的文件夹![2019-07-31_100736](C:\Users\Luge\Desktop\Typora\java项目构造理解\img\2019-07-31_100736.png)

1. src :项目源码目录

2. bin :项目编译后的.class目录

## Eclipse编译

当点击Eclipse运行时候java jdk会默认执行编译，并将编译后的java文件，生成class文件放到项目目录下的bin文件夹里，以.class命名结尾。

## 重点

1. bin目录是整个项目的输出目录,编译后的class文件和项目的配置文件最终都会输出在bin目录中
2. 项目最后的结果是jar文件，jar文件里面也只有class文件夹，并不会有src文件夹，而是将src下的所有包名转换为文件夹保存在bin目录下，而其他Test根目录下的比如自己创建的config文件夹并不会在jar包的bin目录下存在，但是会将所有的非src文件夹下的其他文件夹所有东西都保存到bin目录下
3. .java编译器（jdk）能进行编译项目和组织项目的一切前提是：classpath。java.exe虚拟机有个cp参数，eclipse生成的java工程，也会有一个classpath参数，最终eclipse会将自己的classpath参数传给java.exe的参数cp,用于java虚拟机运行操控。比如，**你在项目Test下创建的文件夹config,是不会被读取到的，因为eclipse默认的classpath只包括src目录**，bin目录jdk目录，和依赖的jar包目录。这也就是为什么我们引进jar包时，一定要**add to build path**，包括创建文件夹时，也要**add to source**。这一切都是为了**添加进claspath路径里面**
4. jvm最后会根据classpath下的路径，将全部输出，输出到bin目录下。包括引进的jar包等等
5. 所以classpath，是虚拟机编译项目的基础，是虚拟机编译组织项目的基础
6. classpath是虚拟机编译组织项目的基础。而项目根目录是创建文件，引进路径的基础
7. buildpath就是classpath。是jvm编译组织生成项目的根本。只有添加进buildpath(classpath)，才能被jvm读取到，也就是才能被代码读取到
8. 每个项目都有一个默认的根路径。Eclipse下默认根目录是Test下，直接就是工程目录下。而生成的Jar包，默认根目录是bin下

## 项目代码里面获取项目或者文件或者类的绝对路径

因为有了classpath的存在，所以我们在读取配置文件或者涉及文件路径操作的时候，在代码里只需要写相对 相对路径就可以，相对路径就是参照classpath的路径，也就是参照最终的bin文件夹路径。如果想获取绝对路径，可以通过类的加载器，随时获取所在类的绝对路径，class.getclassload().getResource("");

## Eclipse怎么调用本地jdk的及本地jdk的虚拟机

是依靠你本地配置的JAVA_HOME环境变量，Eclipse会自动读取这个环境变量地址。进而编译运行项目的。进而也就是把Eclipse自己的classpath传递给jvm的cp参数的

## 工程文件下的.class文件

1. 项目的配置文件

2. 内容

```xml
<?xml version="1.0" encoding="UTF-8"?>
<classpath>
	<!--JDK路径-->
	<classpathentry kind="con" path="org.eclipse.jdt.launching.JRE_CONTAINER/org.eclipse.jdt.internal.debug.ui.launcher.StandardVMType/JavaSE-1.8"/>
	<!--classpath-->
	<classpathentry kind="src" path="src"/>
	<classpathentry kind="src" path="config"/>	
	<classpathentry kind="con" path="org.eclipse.jdt.junit.JUNIT_CONTAINER/4"/>
	<!--Jar包路径-->
	<classpathentry kind="lib" path="lib/mysql-connector-java-5.1.6.jar"/>
	<classpathentry kind="lib" path="lib/asm-3.3.1.jar"/>
	<classpathentry kind="lib" path="lib/cglib-2.2.2.jar"/>
	<classpathentry kind="lib" path="lib/commons-logging-1.1.1.jar"/>
	<classpathentry kind="lib" path="lib/javassist-3.17.1-GA.jar"/>
	<classpathentry kind="lib" path="lib/log4j-1.2.17.jar"/>
	<classpathentry kind="lib" path="lib/log4j-api-2.0-rc1.jar"/>
	<classpathentry kind="lib" path="lib/log4j-core-2.0-rc1.jar"/>
	<classpathentry kind="lib" path="lib/mybatis-3.2.7.jar"/>
	<classpathentry kind="lib" path="lib/slf4j-api-1.7.5.jar"/>
	<classpathentry kind="lib" path="lib/slf4j-log4j12-1.7.5.jar"/>
	<!--输出目录-->
	<classpathentry kind="output" path="bin"/>
</classpath>
```