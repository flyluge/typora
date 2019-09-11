# Linux操作（CentOS 7）

## 防火墙

### 查看防火墙状态

```
systemctl status firewalld
```

### 关闭防火墙

```
systemctl stop firewalld
```

### 开启防火墙

```
systemctl start firewalld
```

### 开启防火墙端口

```
netstat -nupl|grep 3306
```

### 重启防火墙

```
firewall-cmd --reload
```

### 查看端口是否开放

```
firewall-cmd --query-port=3306/tcp
```

### 开放端口

```
firewall-cmd --add-port=8080/tcp
```

### 查看开放的端口

```
firewall-cmd --list-all
```

