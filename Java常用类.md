# Java常用类

## 计算机存储形式

### 原码反码补码知识

1. 字节

   ```http
   国际单位制:
       1KB=1024B；1MB=1024KB=1024×1024B。
       1B（byte，字节）= 8 bit（b）；
       1KB（Kilobyte，千字节）=1024B= 2^10 B；
       1MB（Megabyte，兆字节，百万字节，简称“兆”）=1024KB= 2^20 B；
       1GB（Gigabyte，吉字节，十亿字节，又称“千兆”）=1024MB= 2^30 B；
       1TB（Terabyte，万亿字节，太字节）=1024GB= 2^40 B；
       1PB（Petabyte，千万亿字节，拍字节）=1024TB= 2^50 B；
   ```

   通常一个字节由8个二进制数组成

2. 源码

   ```http
   正数:
   	数字的八位二进制表示
   负数:
   	首位为1
   ```

3. 反码

   ```http
   正数:
   	与原码相同
   负数:
   	首位不变 其他位取反
   ```

4. 补码

   ```http
   正数:
   	与原码相同
   负数:
   	补码+1
   ```

### 计算机存储

```http
计算机中数据是以补码的形式存储的;
原因:
	若以原码形式存储,正数+负数会出现错误结果
```

## 正确获取文件地址(打成jar包依然能运行)

```java
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;

public class TestPath2 {
	public static void test01(){
		try {
			InputStream fis=TestPath2.class.getResourceAsStream("/com/lu/test02/我是一个中文路径.txt");
			InputStreamReader isr=new InputStreamReader(fis, "utf-8");
			int a;
			while((a=isr.read())!=-1) {
				System.out.println((char)a);
			}
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	}

	public static void main(String[] args) {
		System.out.println(TestPath2.class.getResource("/aaa.txt"));
		test01();
	}
}

```





## String

### 格式化输出

```java
String.format(String fmt,Object obj);
//保留两位小数输出字符串
double a=3.141592653;
String f1=String.format("%.4f",a);
System.out.println(f1);
```



## StringBuilder

java.lang.StringBuilder

线程不安全的可变字符序列。

### StringBuilder与StringBuffer的区别

```
前者线程不安全速度快 后者线程安全速度慢
```



## StringBuffer

java.lang.StringBuffer

线程安全的可变字符序列。

### 创建

```java
1.StringBuffer sb=new StringBuffer("测试01");
2.StringBuffer sb=new StringBuffer();
```

### 方法

1. length()

   ```java
   //返回字符串长度
   ```

2. capacity()

   ```java
   //返回缓冲区长度(字符串长度length+缓冲区初始长度16)
   ```

3. append()

   ```java
   //在字符串后面添加数据
   sb.append("测试02");
   ```

4. insert(int start)

   ```java
   //在字符串下标为start后面添加数据
   sb2.insert(1, "插入的内容");
   ```

5. delete(int start,int end)/delete(int index)

   ```java
   //1.删除start到end之间的数据 去头不去尾
   //2.删除index下标对应的数据
   sb2.delete(0, 1);
   ```

6. replace(int start,int end,String t)

   ```java
   //替换start到end之间的字符串为t
   sb2.replace(0, 3, "啦啦啦");
   ```

7. substring(int start,int end)/substring(int index)

   ```java
   //截取字符串//截头不截尾 返回String类型的字符串
   String a=sb2.substring(3,4);
   ```

8. reverse()

   ```java
   //将StringBuffer字符串反转
   ```

   

## Arrays

java.utils.Arrays

### 方法

1. sort

   ```java
   //正序排序
   int a[]={3,1,2};
   Arrays.sort(a);
   ```

2. equals(int a[],int b[]);

   ```java
   //比较两个数组的值是否相同
   int a[]={1,2,3};
   int b[]={1,2,3};
   boolean c=Arrays.equals(a,b);
   System.out.println(c);//true
   ```

3. binarySearch(int[] a,int b)

   ```java
   //二分查找
   int d[]={9,10,2,6,11,2};
   Arrays.sort(d);
   for(int i:d){
   System.out.println(i);
   }
   int index=Arrays.binarySearch(d,10);
   System.out.println(index);
   ```

## Java序列化与反序列化

### Serializable

java.io.Serlizable

public interface **Serializable**

```
类通过实现 java.io.Serializable 接口以启用其序列化功能。未实现此接口的类将无法使其任何状态序列化或反序列化。可序列化类的所有子类型本身都是可序列化的。序列化接口没有方法或字段，仅用于标识可序列化的语义。
```

### 序列化

```java
	//序列化
	public static void serializableUser(Object obj){
		try{
			File file=new File("mapuser.txt");
			FileOutputStream fos=new FileOutputStream(file);
			ObjectOutputStream oos=new ObjectOutputStream(fos);
			oos.writeObject(obj);
			System.out.println("序列化成功");
			oos.close();
			fos.close();
		}catch(Exception e){
			e.printStackTrace();
		}
	}
```

### 反序列化

```java
public static Object deserializableUser(String filename){
		try{
			File file=new File(filename);
			if(file.exists()){
				FileInputStream fis=new FileInputStream(file);
				ObjectInputStream ois=new ObjectInputStream(fis);
				Object obj=ois.readObject();
				System.out.println("反序列化成功");
				ois.close();
				fis.close();
				return obj;
			}else{
				System.out.println("文件不存在");
				return null;
			}
		}catch(Exception e){
			e.printStackTrace();
		}
		return null;
	}
```



## Collection

### 体系图

![1565249795793](C:\Users\Luge\AppData\Roaming\Typora\typora-user-images\1565249795793.png)

![Collection](C:\Users\Luge\Desktop\Typora\tuku\Collection.png)

## JAVA IO

![1565575437072](C:\Users\Luge\AppData\Roaming\Typora\typora-user-images\1565575437072.png)



## 泰勒公式输出月历

```java
package test;
/**
 * @author Luge
 *    打印日期
 *    计算星期公式(泰勒公式):
 *	w=y+[y/4]+[c/4]-2c+[26(m+1)/10]+d-1 
 *
 *  公式中的符号含义如下，w：星期；c：世纪-1；y：年（两位数）；
 * m：月（m大于等于3，小于等于14，即在蔡勒公式中，某年的1、2月要看
 *  作上一年的13、14月来计算，比如2003年1月1日要看作2002年的13月1日来计算）；
 *  d：日；[ ]代表取整，即只要整数部分。(C是世纪数减一，y是年份后两位，M是月份，
 *  d是日数。1月和2月要按上一年的13月和 14月来算，这时C和y均按上一年取值。)
 *
 *	算出来的W除以7，余数是几就是星期几。如果余数是0，则为星期日。 
 */
public class Calendar {
	
	/**
	 * 每月的天数
	 */
	private static int[] mdays= {0,31,28,31,30,31,30,31,31,30,31,30,31};
	/**
	 * @param y:年,m:月,d:日
 	 *      获取某个日期为星期几
	 * @return 返回值为0 - 7分别对应周日至周一
	 *   以下方法只适合于1582年10月15日之后的情形
	 */
	public static int getWeekDay(int y,int m,int d) {
		int w;
		int c=y/100;
		y=y%100;
		if(m<3) {
			m+=12;
			y--;
		}
		w=y+(y/4)+(c/4)-2*c+(26*(m+1)/10)+d-1;
		return w%7;
	}
	/**
	 * 根据年月打印出月历
	 * @param y 年
	 * @param m 月
	 */
	public static void showMonthDays(int y,int m) {
		//获取第一天星期几
		int w=getWeekDay(y,m,1);
		//判断闰年 修改二月天数
		if(isRunNian(y)) {
			mdays[2]=29;
		}else {
			mdays[2]=28;
		}
		//循环输出月历
		System.err.println("日\t一\t二\t三\t四\t五\t六");
		for(int i=0;i<w;i++) {
			System.out.print("\t");
		}
		for(int i=1;i<=mdays[m];i++) {
			System.out.print(i+"\t");
			if((i+w)%7==0) {
				System.out.println();
			}
		}
	}
	/**
	 * 判断一年是否为闰年
	 * @param y 年
	 * @return true:是闰年,false:不是闰年
	 */
	public static boolean isRunNian(int y) {
		if((y%4==0&&y%100!=0)||y%400==0) {
			return true;
		}
		return false;
	}
	public static void main(String[] args) {
		showMonthDays(2019,7);
	}
}

```

## 线程

![1566015722001](C:\Users\Luge\AppData\Roaming\Typora\typora-user-images\1566015722001.png)

### 创建线程的两种方式

1. 继承Thread类

   ```java
   class T1 extends Thread{
       public void run(){
    		   	   
       }
   }
   T1 t=new T1();
   t.start();
   ```

2. 实现Runnable接口

   ```java
   class T2 implements Runnable{
       public void run(){
           
       }	
   }
   Thread t2=new Thread(new T2());
   t2.start();
   ```

### 线程休眠(sleep())

```java
Thread.sleep(1000);//休眠1s
```

### 守护线程(Daemon())

如果一个线程作为守护线程,当其他非守护线程全部停止后,守护线程也会停止

在Java中有两类线程：User Thread(用户线程)、Daemon Thread(守护线程) 

用个比较通俗的比如，任何一个守护线程都是整个JVM中所有非守护线程的保姆：

只要当前JVM实例中尚存在任何一个非守护线程没有结束，守护线程就全部工作；只有当最后一个非守护线程结束时，守护线程随着JVM一同结束工作。
Daemon的作用是为其他线程的运行提供便利服务，守护线程最典型的应用就是 GC (垃圾回收器)，它就是一个很称职的守护者。

User和Daemon两者几乎没有区别，唯一的不同之处就在于虚拟机的离开：如果 User  Thread已经全部退出运行了，只剩下Daemon Thread存在了，虚拟机也就退出了。  因为没有了被守护者，Daemon也就没有工作可做了，也就没有继续运行程序的必要了。

```java
Thread t=new Thread(new T2());
t.setDaemon(true);//设置为守护线程
```

### 插队(插入线程)(join)

执行当前插入的线程

1. x.join()

```java
//当x线程执行完后再执行被插入的线程
public void run() {
    for(int i=0;i<5;i++) {
        if(i==2) {
            try {
                t1.join();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        try {
            Thread.sleep(200);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println(Thread.currentThread().getName());
    }
}
```

2. x.join(long mills)

```java
//让x线程执行相应的时间后再执行被插入线程
public void run() {
    for(int i=0;i<5;i++) {
        if(i==2) {
            try {
                t1.join(400);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        try {
            Thread.sleep(200);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println(Thread.currentThread().getName());
    }
}
```

### 礼让线程(yield())

让出cpu,执行其他线程

```java
public void run() {
    for(int i=0;i<5;i++) {
        if(i==2) {
            try {
                Thread.yield();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        try {
            Thread.sleep(200);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println(Thread.currentThread().getName());
    }
}
```



### 设置优先级(setPriority(int newPriority))

```java
Thread t=new Thread(new Runnable....);
t.setPriority(9);//优先级应该在1-10之间
```

### 同步代码块(synchronized)

```java
public void run(){
    synchronized(this){
    for(int i=0;i<5;i++){
  	  System.out.println(Thread.currentThread().getName()+"正在过山洞 请等待"+(5-i)+"s");
    try{
  	  Thread.sleep(1000);
    }catch(Exception e){
   	 e.printStackTrace();
    }
    }
    	System.out.println(Thread.currentThread().getName()+"过山洞完成");
    }
}
```

### ReentrantLock(互斥锁)

1. lock()
2. unlock()

一个可重入的互斥锁 [`Lock`](../../../../java/util/concurrent/locks/Lock.html)，它具有与使用 
`synchronized` 方法和语句所访问的隐式监视器锁相同的一些基本行为和语义，但功能更强大。 

### Condition(监视器)

Condition 将 Object 监视器方法（wait、notify 和 notifyAll）分解成截然不同的对象，以便通过将这些对象与任意 Lock 实现组合使用，为每个对象提供多个等待 set（wait-set）。其中，Lock 替代了 synchronized 方法和语句的使用，Condition 替代了 Object 监视器方法的使用。 

1. await()

   ```
   暂停当前线程
   ```

2. signal()

   ```
   唤醒一个随机的被暂停的线程
   ```

3. signalAll()

   ```
   唤醒所有被暂停的线程
   ```

```java
import java.util.concurrent.locks.Condition;
import java.util.concurrent.locks.ReentrantLock;
/**
 * ChefsAndDiners为厨师与食客类 类中包含了厨师做饭与食客吃饭两个线程
 * 
 * @author Luge
 *
 */
public class ChefsAndDiners {
	/**
	 * isready true:厨师做好了饭,false:厨师未做好饭
	 */
	boolean isready = true;
	/**
	 * iseated true:食客吃完了饭,false:食客未吃完饭
	 */
	boolean iseated = false;
	/**
	 * 创建一个互斥锁
	 */
	ReentrantLock r=new ReentrantLock();
	Condition c1 = r.newCondition();
	Condition c2 = r.newCondition();
	/**
	 * 厨师做饭
	 */
	public void cook() {
		/**
		 * 创建一个监视器
		 */
		try {
			while (true) {
				r.lock();
					while (!iseated) {
						c1.await();
					}
					System.out.println("厨师正在做饭,需要3s");
					Thread.sleep(3000);
					System.out.println("厨师把食物做好了");
					isready = true;
					iseated = false;
					c2.signal();
				r.unlock();
			}
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
	/**
	 * 食客吃饭
	 */
	public void eat() {
		/**
		 * 创建一个监视器
		 */
		try {
			while (true) {
				r.lock();
					while (!isready) {
						c2.await();
					}
					System.out.println("食客们正在进食,需要2s");
					Thread.sleep(2000);
					System.out.println("食客们把食物吃了");
					isready = false;
					iseated = true;
					c1.signal();
				r.unlock();
			}
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}

```



### 线程等待唤醒机制

#### 注意:

* 在同步代码块中传入的什么对象 ,就必须用该对象调用线程等待唤醒方法

1. wait

   ```
   暂停当前线程
   ```

2. notify

   ```
   随机唤醒单个等待中的线程
   ```

3. notifyAll

   ```
   唤醒所有等待中的线程
   ```



## Runtime(系统运行时)

每个 Java 应用程序都有一个 `Runtime` 类实例，使应用程序能够与其运行的环境相连接。可以通过 
`getRuntime` 方法获取当前运行时。 

```java
//执行操作系统命令
//exec
public static void main(String s[]){
    Runtime r=Runtime.getRuntime();
    System.out.println(r.freeMemory()/1024/1024);
    try{
        //r.exec("shutdown -s -t 300"); //五分钟后关机
        r.exec("shutdown -a");//取消定时关机操作
    }catch(Exception e){
        e.printStackTrace();
    }
}
```

## Timer(计时器)

在指定时间执行定义的事件(TimerTask)

```java
import java.util.Timer;
import java.util.TimerTask;
import java.util.Date;
public class TestTimer{
	public static void main(String[] args){
		Timer t=new Timer();
		Date d=new Date(119,7,16,16,9,50);
		t.schedule(new MyTask(),d,3000);
		System.out.println(d);
		while(true){
			try{
				Thread.sleep(1000);
			}catch(Exception e){
				e.printStackTrace();
			}
			System.out.println(new Date());
		}
	}
}
class MyTask extends TimerTask{
	public void run(){
		System.out.println("执行这个任务");
	}
}
```

## UDP发送与接收数据

* udp:Internet 协议集支持一个无连接的传输协议，该协议称为用户数据报协议（UDP，User Datagram Protocol）

java中利用UDP发送与接收数据

1. 发送

   ```java
       public static void main(String[] args){
           try {
               Scanner in=new Scanner(System.in);
               DatagramSocket ds=new DatagramSocket();
               while(true){
                   String data=in.nextLine();
                   if(data.equals("exit")){
                       break;
                   }
                   DatagramPacket dp=new DatagramPacket(data.getBytes(),data.getBytes().length, InetAddress.getByName("192.168.43.1"),6666);
                   ds.send(dp);//将数据发送出去
               }
               ds.close();
           } catch (Exception e) {
               e.printStackTrace();
           }
       }
   ```

2. 接收

   ```java
       public static void main(String[] args){
           try {
               DatagramSocket ds=new DatagramSocket(6666) ;
               DatagramPacket dp=new DatagramPacket(new byte[1024],1024);
               while(true){
                   ds.receive(dp);
                   byte[] arr=dp.getData();
                   int l=dp.getLength();
                   String s=new String(arr,0,l);
                   String ip=dp.getAddress().getHostAddress();//获取ip
                   int port=dp.getPort();//获取端口号
                   System.out.println(ip+":"+port+":"+s);
               }
           } catch (Exception e) {
               e.printStackTrace();
           }
       }
   ```

   