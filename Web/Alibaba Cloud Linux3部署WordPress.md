Alibaba Cloud Linux3部署WordPress

## 1. 安装所需环境

### 安装 Apache 

```
yum install httpd -y
```

### 启动并设置Apache开机启动

```
systemctl start httpd
systemctl enable httpd
```

### 检查 Apache 

​	在浏览器端输入`服务器公网IP/phpinfo.php`，弹出 apache 的 welcome 界面

![image-20230401215134995](https://s2.loli.net/2023/04/01/dcDTNJomIO2pxl6.png)

### 安装 PHP

```
yum install php -y
```

### 检查 PHP 

```
echo "<?php phpinfo(); ?>" > /var/www/html/phpinfo.php
```

​	执行完成后，在浏览器端输入`服务器公网IP/phpinfo.php`，弹出PHP的版本界面

![image-20230401215310371](https://s2.loli.net/2023/04/01/h13d5CwSubT7DlJ.png)

### 安装 MySQL

```
wget http://dev.mysql.com/get/mysql57-community-release-el7-10.noarch.rpm
yum -y install mysql57-community-release-el7-10.noarch.rpm
rpm --import https://repo.mysql.com/RPM-GPG-KEY-mysql-2022
yum -y install mysql-community-server
```

### 安装 php-mysql

```
yum install php-mysqlnd
```

## 2.调整 MySQL 设置

### 启动并设置 MySQL开机启动 

 (这一步可能会等待一会别以为是死机了就重启了)

```
systemctl start mysqld
```

### 查看 MySQL初始密码

```
grep "password" /var/log/mysqld.log
```

​	运行结果如下：

![image-20230401212509996](https://s2.loli.net/2023/04/01/RuY8QdbU1D5sXzl.png)

​	其中可以看到初始密码为：`2d<01uwmhUV4` （这玩意随机生成而且后面会改，不记住也没有关系）

### 登录 MySQL

输入刚刚得到的初始密码

```
mysql -uroot -p
```

### 修改 MySQL 密码

```
ALTER USER 'root'@'localhost' IDENTIFIED BY '***';
```

​	这里请将 `***`替换为自己设置的新密码。新密码设置的时候如果设置的过于简单会报错，必须同时包含大小写英文字母、数字和特殊符号中的三类字符。举个例子，如果我想设置密码为`Tempesttissimo_1540`，那我应该输入的指令就是

```
ALTER USER 'root'@'localhost' IDENTIFIED BY 'Tempesttissimo_1540';
```

### 创建用于 WordPress 的数据库

​	这里为了方便，这个数据库名称就直接设置为wordpress了

```
create database wordpress; 
```

### 退出MySQL

```
exit;
```

## 3.安装 WordPress

### 下载并解压WordPress

```
cd /var/www/html
wget https://cn.wordpress.org/latest-zh_CN.tar.gz
tar -zxvf latest-zh_CN.tar.gz
```

### 重命名路径(可选)

​	这里设置的路径将会是之后访问的网址。举个例子就是在`/var/www/html`路径下刚刚解压`latest-zh_CN.tar.gz`得到了一个名为`wordpress`的文件夹，里面包含的是未进行配置的WordPress，在配置完成后就需要以`主机ip/wordpress/index.php`来进行访问。

```
cd wordpress
mv * ../
```

​	以上的路径表示访问的网址从`主机ip/wordpress/index.php`变为了`主机ip/index.php`。后文以完成了重命名路径进行演示。

### 删除压缩包

```
rm latest-zh_CN.tar.gz
```

## 4.配置 WordPress

从这后面的部分开始将会大量使用图形交互界面

### 初始化WordPress

浏览器访问`http://公网IP/index.php`（后续以完全按照本文进行过重命名路径的话，被重定向至如下界面：

![](https://s2.loli.net/2023/04/01/mqKvcZyuOE4QI8z.png)

### 数据库链接设置

　数据库名 填写 `wordpress` （与前面在创建用于 WordPress 的数据库时的名字相同即可，完全跟着本文的步骤来的话请直接填写`wordpress`）

​	用户名 填写 `root`

​	密码 填写 前文 ‘修改 MySQL 密码’ 中使用的密码

​	其余保持不变即可

![](https://s2.loli.net/2023/04/01/gpRPjT1kJZEUdGA.png)

#### 可能的异常

可能出现如下错误界面

![](https://s2.loli.net/2023/04/01/Lc47xYVm6oTQgDM.png)

回到主机的命令行窗口，进入`/var/www/html文件夹下：

```
cd /var/www/html
```

新建`wp-config.php`文件并打开

```
vim wp-config.php
```

将浏览器中代码栏的文本全选并复制到主机的命令行窗口中（也就是复制到`wp-config.php`中）

![](https://s2.loli.net/2023/04/01/TPJBqcvKyRY5kta.png)

在`wp-config.php`最后一行补充上`?>`：

![image-20230401222656597](https://s2.loli.net/2023/04/01/KACL6F72HiMsmhG.png)

按下`Esc`键后再次输入`:`进入vim命令行模式，再输入`wq`按下回车，保存并退出vim

![](https://s2.loli.net/2023/04/01/HEpCboFYU9mx1eL.png)

重启Apache服务

```
systemctl restart httpd
```

回到浏览器即可继续后续步骤

### 设置博客信息

照着填

![](https://s2.loli.net/2023/04/01/KO24LJtq5lEVQiy.png)

### 登入WordPress仪表盘

![](https://s2.loli.net/2023/04/01/i95yedpQKHjaq3B.png)



