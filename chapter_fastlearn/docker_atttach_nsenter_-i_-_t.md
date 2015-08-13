
## docker atttach/nsenter /-i -t

* **docker atttach**

在使用-d参数时，容器启动后会进入后台。 某些时候需要进入容器进行操作，有很多种方法，包括使用  docker attach命令或 nsenter 工具等。

	
	$  sudo docker run -i -t -d centos
	911207826f4da78cb8b8a233dea6120a7d2939eea389a94eef2c0b1320572628
	$  sudo docker ps
	CONTAINER ID        IMAGE               COMMAND             CREATED              STATUS              PORTS               NAMES
	911207826f4d        centos:latest       "/bin/bash"         46 seconds ago       Up 45 seconds                           silly_poitras
	$ sudo docker attach 911207826f4d        
	[root@911207826f4d /]# 
	[root@911207826f4d /]# 
	
	
此时，我们以及进入一个正在运行的容器中去执行命令。

＊但是使用  attach 命令有时候并不方便。当多个窗口同时  attach  到同一个容器的时候，所有窗口都会同步显示。当某个窗口因命令阻塞时,其他窗口也无法执行操作了。



