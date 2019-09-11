# Linux下配置MySQL8

## 1. 安装MySQL8

使用![1556544458235](C:\Users\Luge\AppData\Roaming\Typora\typora-user-images\1556544458235.png)远程访问CentOS7

### 1.1 第一步 下载并上传安装包

1.官网下载压缩包

![1556544018265](C:\Users\Luge\AppData\Roaming\Typora\typora-user-images\1556544018265.png)

2.使用![1556544053728](C:\Users\Luge\AppData\Roaming\Typora\typora-user-images\1556544053728.png)将压缩包上传到Linux操作系统的data文件夹下。

![1556544396250](C:\Users\Luge\AppData\Roaming\Typora\typora-user-images\1556544396250.png)

3.解压

```linux
tar -zxvf mysql-8.0.15-linux-glibc2.12-x86_64.tar.xz
```

4.重命名

```linux
mv mysql-8.0.15-linux-glibc2.12-x86_64 mysql
```



### 1.2 第二步 卸载系统自带的Mariadb数据库

1.检测是否安装了Mariadb数据库，若存在则删除。

![1556544582562](C:\Users\Luge\AppData\Roaming\Typora\typora-user-images\1556544582562.png)

```
1. rpm -qa|grep mariadb
2. rpm -e --nodeps mariadb-libs-5.5.56-2.el7.x86_64
```

2.创建mysql用户组和mysql用户

![1556544734362](C:\Users\Luge\AppData\Roaming\Typora\typora-user-images\1556544734362.png)

```
1. groupadd mysql
2. useradd -g mysql mysql
```

### 1.3

## Linux 下的MySQL操作

#### 关闭/启动服务

```
service mysqld stop
service mysqld start
```

#### 登陆

```
mysql -u root -p 
```

