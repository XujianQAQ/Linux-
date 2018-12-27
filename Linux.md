# Linux常用命令

#### Linux系统命令操作语法格式:

​	命令  参数  文件或路径/需要处理的内容

##### 创建目录:

​	1.创建单层单个目录:

​		`mkdir 文件夹名字`

​	2.创建单层多个目录:

​		`mkdir -p{文件夹1, 文件夹2, 文件夹3}`

​	3.创建多层目录:

​		`mkdir -p 文件夹1/文件夹1-1/文件夹1-1-1`	

##### 查看目录:

​	`ll/ls 文件夹路径 `

​	1.`ll信息比较详细` 

​	2.`ls只显示文件夹下的文件名称`

##### 查看看当前目录:

​	`pwd #打印当前所在目录`  

##### 查看文件内容(常用语内容较少的文件)

​	`cat 文件名`

​	参数: -n 显示行号



#### 网络相关:

​	1.`ifconfig常看本机ip信息`	

​	2.`ifup ens33 ifdown ens33 启动网卡/停止网卡`

​	3.`sysytemctl restart/start/stop network 开启/停止网络服务`

​	4.`ps -ef | grep 查看任务是否运行进程`

​	5.`netstat -tunlp | grep 查看任务的端口是否启动`

​	6.`kill 命令 kill 进程pid ,如果杀不死, 就加上 -9 `

​	7.`serlinux 内置防火墙   停止防火墙服务 systemctl start/restart/stop firewalld ` 

​	    `iptables 软件防火墙 -F清空规则 -L查看规则,只有三个说明没有规则了`

​	8.`systemctl disable  firewalld 清除iptables 的开机自启`

​	9.`dns 配置文件 /etc/resolv.conf  /etc/hosts文件: 强制解析测试域名文件`

​	10.`nslookup 查找域名 不加参数为交互模式, 加参数指定域名`

#### 用户管理与文件管理相关:

##### 用户相关:

​	1.`passwd 用户名 修改该用户的密码 没有参数修改当前用户密码  <root>/权限`

​	2.  `useradd 用户名 添加用户 用户信息存放到/etc/passwd `

​	3.`id root 查看root用户的 uid(用户id) gid(组id) group(所在组)`

​	4.`/etc/passwd 存放用户信息, /etc/group/存放组信息`

​	5.`su - 用户名 切换用户   加 - 连同系统环境完全切换  root切换普通用户无需密码 反之需要输入root密码`

​	6.  `groupadd 创建用户组`

​	7.`userdel 删除用户  -f 强制删除 -r同时删除用户家目录 `

​	8.`用root用户去执行这个文件 sudo 需要在/etc/suduer 文件中设置  用户名  ALL=(ALL)  ALL          visudo 此命令会帮助提供语法检测`  

##### 文件相关:

​	1.` dr-xr-xr-x.   5 root root 4096 9月  11 23:55 boot ` 
   	 `   权限     文件连接数  所属组 文件大小  修改文件日期  文件名`

​	2.`d 代表这是一个文件夹 - 代表是一个普通文件  `

​	 `r-x(users)所属用户拥有的权限 `

​	 ` r-x(groups) 所属用户组拥有的权限 ` 

​	 ` r-x(others) 其他用户拥有的权限`

​	`r: read 可读`

​	`w: write 可写`

​	`x: 可执行`

​	`-: 代表没有权限`

​	3. `chmod 用户u/用户组g/其他o+读r/写w/执行x 要操作的文件夹 `

​	`注意先给x w才能使用`

​	4. `chown 更换所属用户`

​	5. `chgrp 更换所属组`

​	6.`软连接的配置 ln -s   目标的绝对路径 快捷方式要放在的绝对路径`

##### 文件压缩解压:

​	`lrzsz 上传下载的小工具  xftp文件传输工具`

​	`tar命令 参数: -c压缩参数 -f指定文件 -x解压参数 -v显示过程`

​	`压缩文件: tar -cf 压缩文件名 想压缩的内容`

​	`解压文件: tar -xf 要解压的文件名`

#### 定时任务:

​	1.`crontab -l 查看任务 -e编辑任务`

​	2.`查看定时任务配置文件 /etc/crontab`

#### 其他:

​	1.`ps -ef 查看进程`

​	2.`PS1` =  "[\u@ \h \w \t]"

​	3.`修改主机名 hostnamectl set-hostname  要修改成的名字 退出后登陆修改成功`

​	4.`查看当前系统的字符编码 echo $Long 修改编码 配置文件/etc/locale.conf 在此处修改`	

​	5.`source 读取命令, 使得配合文件在系统中生效  source/etc/locale.conf`

​	6.`df -h 查看磁盘空间`

#### yum:

​	1.`源的仓库路径 /etc/yum.repos.d/ 只有repo结尾的文件餐可以被看做是一个源仓库`

​	2.`配置国内的yum源:在/etc/yum.repos.d/目录下 定制自己的repo仓库文件`

​	    `阿里镜像站  https://opsx.alibaba.com/mirrors/`

​	    `wget -O  下载文件后  -O参数 指定放到那个目录, 且改名`

​	3.`清除之前的缓存, 生成阿里的新缓存`

​	`备注: 一些包需要用额外的仓库源 如 nginx => epel 源仓库等`	