# Git学习

## 安装git

### 配置全局信息

1. 配置全局用户

   ```haskell
   $ git config --global user.name 'flyluge'
   ```

2. 创建全局邮箱

   ```haskell
   $ git config --global user.email '1378413901@qq.com'
   ```

3. 检查配置

   ```haskell
   $ git congig --list
   ```

   

## 创建版本库(reposity)

```haskell
$ mkdir learngit    创建learngit文件夹
$ cd learngit		进入文件夹
$ pwd				显示当前目录全路径
```

## 创建版本库

```haskell
$ git init    		使当前文件夹变为Git可以管理的仓库
```

### 创建文件readme.txt

### 将文件添加到暂存区

```haskell
$ git add readme.txt
```

### 将暂存区文件提交到仓库(分支)

```haskell
$ git commit -m "提交的信息" 
```

### 修改文件,查看状态

```haskell
$ git status    	显示文件状态	
```

### 查看文件变化

```haskell
$ git status
```

### 再次将文件添加到仓库并提交

```JAVA
$ git add readme.txt
$ git commit -m "修改1.0"
```

## 时光穿梭机

### 创建密钥对

```haskell
$ ssh-keygen -t rsa -C "1378413901@qq.com"
```

### 查看历史版本

```haskell
$ git log
```

### 版本回退

```JAVA
$ git reset --hard b2225571b2d8179658a98263c13c1b644a6bea9a //回退到指定版本
$ git reset --hard head^ //回退到上个版本
$ git reset --hard head^^ //回退到上上版本

/*反馈信息:
HEAD is now at b222557 再次修改
head代表当前版本*/
```

### 从暂存区撤回

```haxe
git reset head readme.txt    //将add过的readme.txt从暂存区撤回
```

### 工作区和暂存区

在工作区中的.git文件中存放git的版本库,版本库中存放一个重要的内容stage(index)暂存区,还有git创建的第一个分支master和一个执行master的指针head

* git init指令结束Git会自动创建一个分支master
* git add指令将文件提交到暂存区
* git commit将暂存区的文件提交到分支![git暂存区](C:\Users\Luge\Desktop\Typora\git学习\tuku\git暂存区.png)

### 删除文件

1. 方式一

   ```haskell
   $ git rm delete.txt
   $ git commit -m "删除文件"
   ```

   

## 远程仓库

### 将本地仓库推送到远程

1. 已有本地仓库

2. 在github建立一个远程仓库

3. 关联远程仓库

   ```haskell
   $ git remote add origin "git@server-name:path/reponame.git"
   ```

4. 第一次推送本地仓库到远程

   ```haskell
   $ git push -u origin master
   ```

5. 每次本地提交后(commit),可以推送

   ```haskell
   $ git push origin master
   ```

### 将远程仓库克隆到本地

1. 在github创建一个远程仓库

2. 没有本地仓库

3. 从远程仓库克隆至本地

   ```haskell
   $ git clone https://github.com/flyluge/learngit.git
   ```

4. 提交

   ```haskell
   $ git push origin master
   ```

## 分支

**特点:**

1. 分支与分支之间不可见
2. 分支与分支之间互不影响

**操作:**

1. 创建分支

   ```haskell
   $ git branch dev
   ```

2. 查看分支(当前分支会用*标识)

   ```haskell
   $ git branch
   ```

3. 切换到分支

   ```haskell
   $ git checkout dec
   ```

4. 创建 测试.txt文件

5. 切换分支并提交

   ```haskell
   $ git checkout dev
   $ git add "测试.txt"
   $ git commit -m "测试分支"
   ```

6. 切换回主分支

   ```haskell
   $ git checkout master
   ```

7. 文件夹中的 测试.txt不见了

8. 将指定分支合并到当前分支

   ```haskell
   $ git merge dev
   ```

9. 分支合并后就可以放心的删除分支了

   ```haskell
   $ git branch -d dev
   ```

### 解决冲突

1. 新建一个分支featrue1

   ```
   $ git branch feature1
   ```

2. 切换到feature1分支

   ```haskell
   $ git checkout feature1
   ```

3. 修改 分支测试.txt

4. 提交

   ```haskell
   $ git add 分支测试.txt
   $ git commit -m "featrue1的提交"
   ```

5. 切换到主分支

   ```haskell
   $ git checkout master
   ```

6. 修改 分支测试.txt

7. 提交

   ```haskell
   $ git add 分支测试.txt
   $ git commit -m "master的提交"
   ```

8. 合并分支

   ```haskell
   $ git merge feature1
   ```

9. 提示有冲突

10. 手动修改冲突文件并提交

    ```haskell
    $ git add 分支测试.txt
    $ git commit -m "合并冲突"
    ```

11. 删除分支

    ```haskell
    $ git branch -d feature1
    ```

    



