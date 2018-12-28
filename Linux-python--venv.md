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

#### `virtualenvwrapper 虚拟环境管理工具 `

​	1.`在物理解释器环境下:下载安装virtualenvwrapper管理包`	

​	  `pip install virtualenvwrapper`

​	2.`环境变量配置要将物理解释器的路径放在path最前面`

​	3.`修改管理工具环境配置文件, 每次开机就加载virtualenvwrapper`

​	  `vim ~/.bashrc :安装好管理工具包后会在用户家目录下自动生成此文件`

​	

```python
export WORKON_HOME=~/Envs   #设置virtualenv的统一管理目录
	export VIRTUALENVWRAPPER_VIRTUALENV_ARGS='--no-site-packages'   #添加virtualenvwrapper的参数，生成干净隔绝的环境
	export VIRTUALENVWRAPPER_PYTHON=/opt/python36/bin/python3     #指定python解释器
	source /opt/python6/bin/virtualenvwrapper.sh #执行virtualenvwrapper安装脚本 
```

​	`在文件最下添加以上配置 : 注意路径换成自己机器python解释器安装到的位置`

 4. `重新登录回话 使配置生效`

 5. `安装成功后 管理工具提供了以下命令`

    `mkvirtualenv  虚拟环境名   #自动下载虚拟环境，且激活虚拟环境`

    `workon  虚拟环境名   #激活虚拟环境`

    `deactivate  退出虚拟环境 `

    `rmvirtualenv	删除虚拟环境` 

    `cdvirtualenv  进入当前已激活的虚拟环境所在的目录`

    `cdsitepackages 进入当前激活的虚拟环境的，python包的目录`