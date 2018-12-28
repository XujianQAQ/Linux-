## Linux下Python环境安装

#### 一、编译安装python3:

##### 		1. 下载python3 的源码:

​			`cd /opt/`

​			`wge thttps://www.python.org/ftp/python/3.6.2/Python-3.6.2.tgz `

`解决环境依赖: `​                 

```
yum install  gcc patch libffi-devel python-devel  zlib-devel bzip2-devel openssl-                    devel ncurses-devel sqlite-devel readline-devel tk-devel gdbm-devel 
			 db4-devel libpcap-devel xz-devel -y
```

##### 		2.解压源码包:

​			`tar -xvf  Python-3.6.2.tgz`

##### 		3.切换源码包目录:

​			`cd Python-3.6.2`

##### 		4.编译且安装:

​			`1.释放编译文件makefile, makefile用来编译且安装装的 `

​			  `./configure --prefix=/opt/python36/`

​			  `--prefix  指定软件的安装路径`

​			`2.开始编译python3  make命令`

​			`3.编译且安装(此步骤生成 /opt/python36)   make install`

​			`4.配置环境变量  echo $PATH查看环境变量`

​			  `编辑个人配置文件 vim /etc/profile 在最后一行添加PATH=...........python36/bin 路径`

## Python虚拟环境配置

#### 	`virtualenv 就是一个虚拟解释器 `

​	 1.`下载virtualenv工具`

​		`通过物理环境的pip工具安装 :` 

​		 `pip3 install -i https://pypi.tuna.tsinghua.edu.cn/simple virtualenv `

​	2.`创建虚拟环境:`

​		`virtualenv --no-site-packages --python=python3   venv1`

​	3.`进入虚拟环境目录, 激活虚拟环境`

​		`source myenv/venv1/bin/activate`

​	4.`退出虚拟环境命令 deactive`