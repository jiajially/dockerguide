
## -p/P 参数        外部访问容器


有时候，容器要运行一些网络应用，需要外部能访问到这些应用，就需要使用-p/P 参数指定一个主机端口，映射到容器端口中。其中使用P系统会分配一个随机的 49000~49900（后期版本没有此限制，会随机任意端口） 的端口到内部容器开放的网络端口。

就拿仓库服务镜像来做例子：
	
	$ sudo docker run -d -P registry 
	b89fc89e061dee24ac532af1890cd26e6e016545e0978b01d3d4eadca67119aa
	$ sudo docker ps
	CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                     NAMES
	b89fc89e061d        registry:latest     "docker-registry"   5 seconds ago       Up 4 seconds        0.0.0.0:32768->5000/tcp   focused_brown       
	$ curl 192.168.4.100:32768/v1/search
	{"num_results": 0, "query": "", "results": []}[root@registry liugang]# 
	$ sudo docker logs b89fc89e061d
	[2015-08-18 00:11:41 +0000] [1] [INFO] Starting gunicorn 19.1.1
	[2015-08-18 00:11:41 +0000] [1] [INFO] Listening at: http://0.0.0.0:5000 (1)

我们可以看到，当我们加上-P时，docker会任意指定一个一个端口指定到容器的开放端口5000上。从容器到运行日志也是可以看出，在容器的5000端口会有一个监听。当我们通过外网的，也就是宿主机的IP 和端口就可以访问到该容器内提供的服务，这里是仓库服务。

-p（小写的）则可以指定要映射的端口，并且，在一个指定端口上只可以绑定一个容器。支持的格式有 
    
    ip:hostPort:containerPort | ip::containerPort | hostPort:containerPort。


### 映射所有接口地址
我们用到的是 hostPort:containerPort，也就是将制定端口映射到主机地址的任意地址的指定端口：
	
    $ sudo docker run -d -p 80:5000 registry
	a7abe89606427e3cb90698a6d302e8
	$ sudo docker ps
	CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                  NAMES
	a7abe8960642        registry:latest     "docker-registry"   4 seconds ago       Up 3 seconds        0.0.0.0:80->5000/tcp   ecstatic_wright     
	2abb0f04066999adff36b13be2e380c3de
	
我们将主机80端口映射到容器，这样我们直接用主机地址就可以访问到容器了。
### 映射到指定地址的指定端口
可以使用 ip:hostPort:containerPort 格式指定映射使用一个特定地址，比如 localhost 地址 127.0.0.1
	
    $ sudo docker run -d -p 127.0.0.1:5000:5000 registry
	9f11390c1e9d048f7d82ff6bfb7e65f5531865343a5a2e6b660c0634e90eda26
	$ sudo docker ps
	CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                      NAMES
	9f11390c1e9d        registry:latest     "docker-registry"   6 seconds ago       Up 5 seconds        127.0.0.1:5000->5000/tcp   romantic_carson 
    
### 映射到指定地址的任意端口
使用 ip::containerPort 绑定 localhost 的任意端口到容器的 5000 端口，本地主机会自动分配一个端口。

	$ sudo sudo docker run -d -p 127.0.0.1::5000 registry
	d8cd77ecc45f434ab9edc0a7e83514ef7cb019fabc9bdbc0b522bb916b309789
	$ sudo docker ps
	CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                       NAMES
	d8cd77ecc45f        registry:latest     "docker-registry"   4 seconds ago       Up 3 seconds        127.0.0.1:32768->5000/tcp   grave_carson 

从上面看出，此时docker默认的传输协议是tcp方式，我们也可以改为其他方式标记：

	$ sudo docker run --name=test_port -d -p 127.0.0.1:5000:5000/udp registry
	
### 使用 docker port 查看端口信息

	$ docker port test_port 
	5000/udp -> 127.0.0.1:5000

注意：

*容器有自己的内部网络和 ip 地址（使用 docker inspect 可以获取所有的变量，Docker 还可以有一个可变的网络配置。）*

*-p 标记可以多次使用来绑定多个端口*