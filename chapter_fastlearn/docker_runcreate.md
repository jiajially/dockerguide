
## docker run/create 容器的创建

* **docker  create**


		
	$ sudo docker create ubuntu /bin/echo 'Hello world'
	a637c1d67506951928be296f2db02fa3e2b6e974ef371b181d9c26d1c8995963
	$...


若有如上输出则代表容器创建成功

	
	$ sudo docker ps -a
	CONTAINER ID        IMAGE                    COMMAND                     CREATED             STATUS              PORTS               NAMES
	a637c1d67506        ubuntu:latest            "/bin/echo 'Hello wo        4 minutes ago                                               mad_hopper          
	…
	…
	
	
然后在启动它

	
	$ sudo docker start a637c1d67506        
	a637c1d67506   
	$...
	

启动成功后会反回容器ID。
	
	
	$ docker ps -a
	CONTAINER ID        IMAGE                    COMMAND                   CREATED                STATUS                                 PORTS           NAMES     
	a637c1d67506        ubuntu:latest           "/bin/echo 'Hello wo       10 minutes ago         Exited (0) 2 minutes ago                               mad_hopper 
	         
	
但是当我们去查看容器状态时，容器没有在运行，这时因为我们在创建容器的时候，让容器执行的命令是/bin/echo 'Hello world'，当容器执行完命令的时候就终止结束了。

* **docker run**


用法与create类似，只是在创建容器后不需要进行start操作就可以运行。

	
	$  sudo docker run ubuntu /bin/echo 'Hello world'
	Hello world
	$...


与上面一样，在运行完Hello world 之后也会退出容器。


* **Daemonized（守护态）**


往往我们需要容器在后台一致执行，这时我们就需要在创建镜像的时候让容器以守护台方式(-d  参数)运行。

	
	$ sudo docker run -d ubuntu /bin/sh -c "while true; do echo hello world; sleep 1; done"
	61f37c1940c8ec9f08b107e99655b8a5181ded340415e3c15cf413069d556b73
	$...
	

这时，我们查看一下容器状态：

	
	$ sudo  docker ps -a
	CONTAINER ID        IMAGE                  COMMAND                CREATED               STATUS                      PORTS               NAMES
	61f37c1940c8        ubuntu:latest          "/bin/sh -c 'while t   4 seconds ago         Up 3 seconds                                    prickly_galileo 
	…
	…


查看容器输出的信息

	
	$ sudo docker logs 61f37c1940c8
	hello world
	hello world
	hello world
	hello world
	….
	….
	

*注意，让容器以后台方式运行，并不是加一个 -d 参数就可以，命令行COMMAND所执行的动作必须为持续运行的状态。


