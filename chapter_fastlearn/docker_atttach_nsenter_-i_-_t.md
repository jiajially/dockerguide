
## docker atttach/nsenter /-i -t

* **docker run -i -t**

若我们想在容器启动时直接进入容器， 那么我们只需在run后加上-i 和 -t 参数即可。

参数解释：

    -i     Keep STDIN open even if not attached
    -t     Allocate a pseudo-TTY 分配一个伪终端

示例：
	
	$ sudo docker run -it centos
	[root@c0f8ae60f8c5 /]#
	[root@c0f8ae60f8c5 /]# exit
	exit
	$ sudo docker ps -a
	CONTAINER ID        IMAGE          COMMAND             CREATED                        STATUS                       PORTS               NAMES
	c0f8ae60f8c5        centos         "/bin/bash"         About a minute ago             Exited (127) 11 seconds ago                      insane_nobel        
	f629fa879a34        centos         "/bin/bash"         52 minutes ago                 Up 52 minutes                                    test1       
	
我们可以看到，创建容器后会立刻进入容器，当我们退出容器时，容器就会停止活动。
如果在执行run命令时没有指定-a，那么docker 默认会挂载所有标准数据流，包括输入输出和错误。你可以特别指定挂载哪个标准流。

详细的docker run指令方法参数 将在后面详解。



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

* 但是使用  attach 命令有时候并不方便。当多个窗口同时  attach  到同一个容器的时候，所有窗口都会同步显示。当某个窗口因命令阻塞时,其他窗口也无法执行操作了。


* **nsenter**


nsenter可以访问一个进程大名字空间。 指令时包含在untli-linux（2.23版本之后才会包含）软件包里。这里需要安装一下：
	
	$ sudo yum -y install util-linux 

完成后检验：

	$ nsenter -V
	nsenter from util-linux 2.23.2
	
如果要进入容器内需要知道进程的pid。

	$ sudo  docker run -idt --name=test1 centos f629fa879a34af902a259e831de1cbc298db2b0f469aa49f80b80a0f81869943
	$ sudo docker ps
	CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
    f629fa879a34        centos              "/bin/bash"         6 seconds ago       Up 5 seconds                            test1 
	$ sudo PID=$(docker inspect --format "{{ .State.Pid }}" f629fa879a34)
	$ sudo  echo $PID
	22109
	$ sudo nsenter --target 22109 --mount --uts --ipc --net --pid
	[root@f629fa879a34 /]# 
	[root@f629fa879a34 /]# 
	[root@f629fa879a34 /]# 
	
这样就完成了进入容器内访问的目的。

此外，为了方便进入容器，牛人已经为我们封装好指令，我们只需利用简单的两行代码就可完成操作。
下载这个脚本.bashrc_docker，并将内容放到 .bashrc 中：
	
	$ sudo wget -P ~ https://github.com/yeasy/docker_practice/raw/master/_local/.bashrc_docker; 
	$ sudo echo "[ -f ~/.bashrc_docker ] && . ~/.bashrc_docker" >> ~/.bashrc; source ~/.bashrc

执行完后我们只需：

	 $ sudo echo $(docker-pid f629fa879a34)
	22109
	$  sudo docker-enter f629fa879a34
	[root@f629fa879a34 ~]# 
	
实际上也是使用nsenter进入容器，只不过更简洁罢了。 





