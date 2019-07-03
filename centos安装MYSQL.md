# centos安装MYSQL.md

## 安装前准备
到官网下载的mysql-8.0.16-Linux的glibc2.12-x86_64.tar.xd
通过XSHELL或者采用xFTP，PSPC等工具把安装包上传到CentOS的服务器

## 解压缩包

xd –d mysql-8.0.16-linux-glibc2.12-x86_64.tar.xd

tar -xvf mysql-8.0.16-linux-glibc2.12-x86_64.tar

### 给包重命名为mysql,并安装到/usr/local/目录下

mv mysql-8.0.16-linux-glibc2.12-x86_64 /usr/local/mysql

### 查看mysql目录下的文件

https://github.com/konpty/region.github.io/blob/master/1.png

---
### 检查mysql组和用户是否存在，创建mysql用户组

groupadd mysql

useradd -g mysql mysql

### 创建mysql用户组

cat /etc/group | grep mysql 

cat /etc/passwd | grep mysql

### 修改用户mysql的密码为A2019a（自己设定）

passwd mysql

https://github.com/konpty/region.github.io/blob/master/2.png 

### 更改所属的组和用户

chown -R mysql 

chgrp -R mysql mysql

### 进入mysql目录

cd /usr/local/mysql

### 创建data目录，创建后会默认设定一个随机mysql登陆密码SnQytTb>%1;6（每次执行都会不一样）

su mysql

cd /usr/local/mysql/bin

./mysqld –initialize

https://github.com/konpty/region.github.io/blob/master/3.png

---
### 新版的数据库是没有my.cnf需要创建my.cnf

### 在/etc/下创建创建my.cnf

cd /etc/

touch my.cnf

vim my.cnf

cat my.cnf

### 在配置文件my.cnf添加如下配置:

*[mysql]*

*设置mysql客户端默认字符集 *

default-character-set=utf8 

[mysqld]

skip-name-resolve

设置3306端口

port = 3306

设置mysql的安装目录

basedir=/usr/local/mysql

设置mysql数据库的数据的存放目录

datadir=/usr/local/mysql/data

允许最大连接数

max_connections=200

服务端使用的字符集默认为8比特编码的latin1字符集

character-set-server=utf8

创建新表时将使用的默认存储引擎

default-storage-engine=INNODB 

lower_case_table_names=1

max_allowed_packet=16M

https://github.com/konpty/region.github.io/blob/master/4.png

---
### 修改config配置，修改SELINUX=disabled

vi /etc/selinux/config

https://github.com/konpty/region.github.io/blob/master/5.png

### 修改mysql目录权限

chown -R mysql:mysql /usr/local/mysql

### 创建软连接(实现可直接命令行执行mysql)

ln -s /usr/local/mysql/bin/mysql /usr/bin

### mysqld配置,拷贝启动文件到/etc/init.d/下并重命令为mysqld

cp /usr/local/mysql/support-files/mysql.server  /etc/init.d/mysqld

### 增加执行权限

chmod 755 /etc/init.d/mysqld

### 检查自启动项列表中没有mysqld

chkconfig --list mysqld

### 如果没有就添加mysqld

chkconfig --add mysqld

### 设置开机启动

chkconfig mysqld on

---
### 启动测试

service mysqld start

https://github.com/konpty/region.github.io/blob/master/6.png

说明我们的配置文件成功，mysql彻底安装完成。

安装完数据库后，我们需要重置mysql连接密码，用上面随机生成的密码登陆mysql。

mysql -u root -p (一路直接回车)

https://github.com/konpty/region.github.io/blob/master/7.png

### 在mysql中修改密码为123456

set PASSWORD = '123456'

https://github.com/konpty/region.github.io/blob/master/8.png


