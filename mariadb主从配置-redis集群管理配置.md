# `redis发布订阅,持久化,集群配置`

### `发布订阅`:	

##### 	发布者:

​		`publish 频道 消息  给频道发消息`

##### 	订阅者:

​		`SUBSCRIBE 频道   订阅频道`

​		`PSUBSCRIBE 频道*   支持模糊匹配的订阅`

##### 	频道:

​		`channel  频道名 自定义`

### `redis 持久化之RDB`:

​	1.`在配置文件中添加参数,开启rdb功能`

​	   `redis.conf 写入`

```
    redis.conf 写入
    port 6379
    daemonize yes
    logfile /data/6379/redis.log
    dir /data/6379
    dbfilename   s15.rdb
    save 900 1                    #rdb机制 每900秒 有1个修>改记录
    save 300 10                    #每300秒        10个修改
    记录
    save 60  10000                #每60秒内        10000修>改记录
```

​	2.`开启rdb服务端,测试rdb功能`

​	   `redis-server redis.conf `

### `redis持久化之aof`:

​	1.`开启aof功能，在redis.conf中添加参数`

```
    port 6379
    daemonize yes
    logfile /data/6379/redis.log
    dir /data/6379
    appendonly yes
    appendfsync everysec
```

​	2.`开启aof服务端,测试aof功能`

​	

```
    redis不重启之rdb数据切换到aof数据
    1.准备rdb的redis服务端
        redis-server   s15-redis.conf (注明这是在rdb持久化模式下)

    2.切换rdb到aof
    redis-cli  登录redis，然后通过命令，激活aof持久化
    127.0.0.1:6379>  CONFIG set appendonly yes				#用命令激活aof持久化(临时生效，注意写入到配置文件)
    OK
    127.0.0.1:6379> 
    127.0.0.1:6379> 
    127.0.0.1:6379>  CONFIG SET save "" 			#关闭rdb持久化

    2.5 将aof操作，写入到配置文件，永久生效，下次重启后生效
        port 6379
        daemonize yes 
        logfile /data/6379/redis.log
        dir /data/6379   

        #dbfilename   s15.rdb
        #save 900 1  
        #save 300 10 
        #save 60  10000 
        appendonly yes
        appendfsync everysec

    3.测试aof数据持久化 ,杀掉redis，重新启动
    kill 
    redis-server s15-redis.conf 

```

### `redis 主从配置`

`检查redis 的主从信息`

`redis-cli  -p 6379  info  检查数据库信息`

``redis-cli  -p 6379  info  replication  检查数据库主从信息`



```

```

### `redis哨兵`

```
1.什么是哨兵呢？保护redis主从集群，正常运转，当主库挂掉之后，自动的在从库中挑选新的主库，进行同步

2.redis哨兵的安装配置
	1. 准备三个redis数据库实例（三个配置文件，通过端口区分）
		[root@localhost redis-4.0.10]# redis-server redis-6379.conf 
		[root@localhost redis-4.0.10]# redis-server redis-6380.conf 
		[root@localhost redis-4.0.10]# redis-server redis-6381.conf 
	2.准备三个哨兵，准备三个哨兵的配置文件(仅仅是端口的不同26379,26380,26381)
        -rw-r--r--  1 root root    227 Jan  2 18:44 redis-sentinel-26379.conf
            port 26379  
            dir /var/redis/data/
            logfile "26379.log"

            sentinel monitor s15master 127.0.0.1 6379 2

            sentinel down-after-milliseconds s15master 30000

            sentinel parallel-syncs s15master 1

            sentinel failover-timeout s15master 180000
            daemonize yes


	-rw-r--r--  1 root root    227 Jan  2 18:45 redis-sentinel-26380.conf
		快速生成配置文件
		sed "s/26379/26380/g" redis-sentinel-26379.conf >  redis-sentinel-26380.conf 
	-rw-r--r--  1 root root    227 Jan  2 18:46 redis-sentinel-26381.conf
		sed "s/26379/26381/g" redis-sentinel-26379.conf >  redis-sentinel-26381.conf 

	3.添加后台运行参数，使得三个哨兵进程，后台运行
[root@localhost redis-4.0.10]# echo "daemonize yes" >> redis-sentinel-26379.conf 
[root@localhost redis-4.0.10]# echo "daemonize yes" >> redis-sentinel-26380.conf 
[root@localhost redis-4.0.10]# echo "daemonize yes" >> redis-sentinel-26381.conf 

	4.启动三个哨兵
		1003  redis-sentinel redis-sentinel-26379.conf 
		1007  redis-sentinel redis-sentinel-26380.conf 
		1008  redis-sentinel redis-sentinel-26381.conf 
		/opt/redis/data
	5.检查哨兵的通信状态
	redis-cli -p 26379  info sentinel 
	查看结果如下之后，表示哨兵正常
		[root@localhost redis-4.0.10]# redis-cli -p 26379  info sentinel 
		# Sentinel
		sentinel_masters:1
		sentinel_tilt:0
		sentinel_running_scripts:0
		sentinel_scripts_queue_length:0
		sentinel_simulate_failure_flags:0
		master0:name=s15master,status=ok,address=127.0.0.1:6381,slaves=2,sentinels=3

	6.杀死一个redis主库，6379节点，等待30s以内，检查6380和6381的节点状态
	kill 6379主节点
	redis-cli -p 6380 info replication 
	redis-cli -p 6381 info replication 
	如果切换的主从身份之后，（原理就是更改redis的配置文件，切换主从身份）
	
	7.恢复6379节点的数据库，查看是否将6379添加为新的slave身份
```

### `redis-cluster集群配置`

```
1.准备6个redis数据库实例，准备6个配置文件redis-{7000....7005}配置文件
	-rw-r--r-- 1 root root 151 Jan  2 19:26 redis-7000.conf
	-rw-r--r-- 1 root root 151 Jan  2 19:27 redis-7001.conf
	-rw-r--r-- 1 root root 151 Jan  2 19:27 redis-7002.conf
	-rw-r--r-- 1 root root 151 Jan  2 19:27 redis-7003.conf
	-rw-r--r-- 1 root root 151 Jan  2 19:27 redis-7004.conf
	-rw-r--r-- 1 root root 151 Jan  2 19:27 redis-7005.conf

2.启动6个redis数据库实例
	[root@localhost s15rediscluster]# redis-server redis-7000.conf 
	[root@localhost s15rediscluster]# redis-server redis-7001.conf 
	[root@localhost s15rediscluster]# redis-server redis-7002.conf 
	[root@localhost s15rediscluster]# redis-server redis-7003.conf 
	[root@localhost s15rediscluster]# redis-server redis-7004.conf 
	[root@localhost s15rediscluster]# redis-server redis-7005.conf 

3.配置ruby语言环境，脚本一键启动redis-cluster 
	1.下载ruby语言的源码包，编译安装
		wget https://cache.ruby-lang.org/pub/ruby/2.3/ruby-2.3.1.tar.gz
	2.解压缩
		./configure --prefix=/opt/ruby/	   释放makefile
		make && make install     编译且安装
	3.下载安装ruby操作redis的模块包
		wget http://rubygems.org/downloads/redis-3.3.0.gem
		
	4.配置ruby的环境变量
	echo $PATH
	
	vim /etc/profile
	写入最底行
	PATH=$PATH:/opt/ruby/bin/
	读取文件
	source /etc/profile 
	
	5.通过ruby的包管理工具去安装redis包，安装后会生成一个redis-trib.rb这个命令
	一键创建redis-cluster 其实就是分配主从关系 以及 槽位分配 slot槽位分配
	/opt/redis-4.0.10/src/redis-trib.rb create --replicas 1 127.0.0.1:7000 127.0.0.1:7001 127.0.0.1:7002 127.0.0.1:7003 127.0.0.1:7004 127.0.0.1:7005
	
	6.检查节点主从状态
	redis-cli -p 7000  info replication 
	
	7.向redis集群写入数据，查看数据流向
	redis-cli -p 7000    #这里会将key自动的重定向，放到某一个节点的slot槽位中
	set  name  s15 
	set  addr shahe  
```

