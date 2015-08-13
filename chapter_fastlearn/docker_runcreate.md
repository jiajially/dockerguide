
## docker run/create 容器的创建

docker  create

		
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