# Linux 下数据库安装,环境配置

### 1, mariadb

##### 	MariaDB数据库管理系统是MySQL的一个分支,完全兼容MySQL

​	1.`安装mariadb`

​		`-yum`

​		`-源码编译安装`

​		`-下载rpm安装`

​	2.`由于阿里/腾讯的官方yum源中的Mariadb版本比较老,所以要配置MariaDB官方yum源`

​	3.`手动配置官方yum源`

​	    `touch /etc/yum.repos.d/mariadb.repo`

​	   `写入以下内容:`

```
	[mariadb]
	name = MariaDB
	baseurl = http://yum.mariadb.org/10.1/centos7-amd64
	gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
	gpgcheck=1
```

​	4.`通过yum安装mariadb软件, 安装mariadb服务端和客户端`

​	  `yum install MariaDB-server MariaDB-client -y `

​	5.`数据库初始化:`

​	  `mysql_secure_installation  这句话可以初始化mysql 删除匿名用户 设置root密码等等... `

​	6.`设置mysql的中文编码支持, 修改/etc/my.cnf  命令 vim /etc/my.cnf`

​	   `在[mysqld]中添加参数, 似的mariadb服务端支持中文`

```
  character-set-server=utf8
  collation-server=utf8_general_ci
```

​	   `重启mariadb服务, 读取my.cnf新配置`

​	    `登录数据库, 查看版本字符编码 mysql -uroot -p  输入\s 查看编码`

##### 	mysql常用命令:

​		1.`desc 查看表结构`

​		2.`create database 数据库名 //创建数据库`

​		3.`create table 表名 //创建表`

​		4.`show create database 数据库名 //查看此数据库是如何创建的`

​		5.`show create table 表名 //查看此表是如何创建的`

​		6. `set password = PASSWORD(' password ')  // 修改数据库密码`

​		7.`create user xujian@'%' identified by 'password' //创建mysql普通用户, 默认权限非常低`

​		8.`use mysql  //使用自带的mysql database `

​		   `select host, user, password from user; // 查看mysql数据库中的用户信息`

​		9.`给用户添加权限命令 `

​                    	 ` grant all privileges on *.* to 账户@主机名 对所有库和所有表授权所有权限`

​		  	 `grant all privileges in *.* to xujian'%'; 给xujian 用户授予所有权限`

​			`flush privileges  刷新授权表`

​		10.`授予远程登录的权限命令`

​			`grant all privileges in *.* to xujian'%' identified by 'xujian123.' 给与xujian用		       户授予远程登录的命令 `

​			`mysql -uxujian -p -h 服务器的地址  //连接服务器上的mysql`

​		11.`mysql的数据备份与恢复`

​			(1).`mysqldump -u root -p --all-databases < /data/AllMysql.dump  // 到处当前数据库的所有db, 到一个文件中 `

​			(2).`登录mysql 导入数据`

​				`mysql -u root -p`

​					`> source /data/AllMysql.dump`

​			(3).`导入数据也可以在登录mysql的时候导入数据`

​				`mysql -uroot -p < /data/AllMysql.dump`





### 1, redis

#####  	Redis 是一个开源（BSD许可）的，内存中的数据结构存储系统，它可以用作数据库、缓存和消息中间件

​	`通过源码编译安装redis`

​	1.`下载源码包`

​		`wget  http://download.redis.io/releases/redis-4.0.10.tar.gz `

​	2.`解压redis `

​		`tar -zxf redis-4.0.10.tar.gz `

​	3.`进入redis安装包目录`, 直接进行编译安装

​		`make && make install`

​	4.`可以通过指定配置文件骑电动redis`

​		

```
    vim /opt/redis-4.0.10/redis.conf 

        1.更改bind参数，让redis可以远程访问
            bind 0.0.0.0
        2.更改redis的默认端口
            port 6380
        3.使用redis的密码进行登录
            requirepass 登录redis的密码
        4.指定配置文件启动
            redis-server redis.conf 
```

​	5.`通过非默认端口登录redis(包括用户认证)  redis -p auth 密码   `

​	6.`通过登录redis，用命令查看redis的密码`
​	   `config set  requirepass  新的密码     	#设置新密码`
​	   `config get  requirepass  			#获取当前的密码`