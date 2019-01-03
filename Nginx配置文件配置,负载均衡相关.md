# `Nginx配置文件配置,负载均衡相关`

### `Nginx主配置文件`:

```
    worker_processes  4;   nginx工作进程数，根据cpu的核数定义
    events {
        worker_connections  1024;    #连接数
    }
    #http区域块，定义nginx的核心web功能
    http {
        include(关键字)       mime.types(可修改的值);
        default_type  application/octet-stream;

        #定义日志格式
        log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                          '$status $body_bytes_sent "$http_referer" '
                          '"$http_user_agent" "$http_x_forwarded_for"';
        #开启访问日志功能的参数		  
        access_log  logs/access.log  main;
        sendfile        on;
        #tcp_nopush     on;
        #keepalive_timeout  0;
        #保持长连接
        keepalive_timeout  65;
        #支持图片 gif等等压缩，减少网络带宽
        gzip  on;

        #这个server标签 控制着nginx的虚拟主机(web站点)
        server {
            # 定义nginx的入口端口是80端口
            listen       80;
            # 填写域名，没有域名就写ip地址
            server_name  www.xujian.com;
            # 定义编码
            charset utf-8;
            # location定义网页的访问url
            #就代表 用户的请求 是  192.168.13.79/
            location / {
                #root参数定义网页根目录
                root   /opt/myserver/xujian;
                #定义网页的首页文件，的名字的
                index  index.html index.htm;
            }
            #定义错误页面，客户端的错误，就会返回40x系列错误码
            error_page  404  403 401 400            /404.html;
            #500系列错误代表后端代码出错
            error_page   500 502 503 504  /50x.html;
        }
        #在另一个server{}的外面，写入新的虚拟主机2
        server{
            listen 80;
            server_name  www.xujian1.com;
            location /  {
            root  /opt/myserver/xujian1;		#定义虚拟主机的网页根目录
            index  index.html;
            }
        }
    }
```

### `Nginx访问日志监控`:

​	`1.开启nginx.conf中的日志参数`​	

```
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                          '$status $body_bytes_sent "$http_referer" '
                          '"$http_user_agent" "$http_x_forwarded_for"';
        #开启访问日志功能的参数		  
        access_log  logs/access.log  main;
```

​	`2.检查access.log的日志信息`

```
	tail -f  access.log 
```

### `Nginx拒绝访问功能`:

​	`1.在nginx.conf中，添加参数`​	

```
    在server{}虚拟主机标签中，找到location 然后添加参数
           当访问 192.168.13.33 地址访问 192.168.13.79/  的时候 
            location / {
                #拒绝参数是 deny 
                #deny 写你想拒绝的IP地址
                #deny还支持拒绝一整个网站
                deny  192.168.13.33;
                root   /opt/myserver/rihan;
                index  index.html;
            }
```

### `Nginx的错误页面优化`:

​	`1.在nginx.conf中,添加参数`

```
    这个x40x.html存在 虚拟主机定义的网页根目录下
      error_page  404              /x40x.html;
```

### `Nginx正/反向代理,负载均衡`:

​	`1.正向代理`

​	`说白了就是用户通过客户端访问nginx服务器,nginx服务器在其中充当一个中间商的角色,实际返回给用户的数据内容通过nginx服务器再向应用的实际服务器发起请求拿到数据,再通过nginx服务器返回给用户进而展示给用户 (和VPN类似)`

​	`2.反向代理`

​	`nginx真正使用最多是反向代理,可以通过反向代理实现服务器集群,实现网站的高并发服务器,轻松应对大量请求`

​	`正向代理代理的是用户,那么反向代理代理的就是实际上的应用服务器,当大量请求发生时,nginx首先接收用户的请求,并根据nginx的几种规则(1.默认的轮询算法,2.ip_hash 哈希算法,3.权重,4.url_hash 等)来分配请求给服务器集群中的服务器,这样就实现了服务器中的每一台服务器分摊大量请求发生时的服务器压力`	

​	

```
反向代理配置:
		在nginx.conf中添加一下代码(不需要修改的代码块省略):
			upstream webserver(可自定义名字)  {
                    ip_hash; #此处使用的是ip_hash实现负载均衡
                    server 192.168.13.79 ;
                    server 192.168.13.24 ;
                    }
            server {
                    listen       80;
                    server_name  192.168.13.121;  # 对外公布的ip地址(实际访问的地址)
                    #charset utf8;

                    #access_log  logs/host.access.log  main;
                    #核心配置，就在这，一条proxy_psss参数即可
                    location / {
                      proxy_pass http://webserver(上方定义的upstream);
                        #root   html;
                        #index  index.html index.htm;
                       }
                    }
```

`只需要上面简单的几部就解决了大量请求并发时的服务器压力问题`