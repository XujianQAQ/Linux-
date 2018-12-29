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



 