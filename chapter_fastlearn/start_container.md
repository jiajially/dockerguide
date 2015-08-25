# 容器使用入门
使用info命令检查docker安装程序是否正常运行：

	# 检查是否安装好docker
	$ docker info

如果提示如下信息: command not found 或者 类似于/var/lib/docker/repositories: permission denied 可能安装有问题或者尝试在前面加上sudo。

此外，基于docker系统配置，命令行前面应该加上sudo，来确保正常执行命令，并且此时系统会为Administrar创建一个名叫docker的Unix 用户组来让其他用户加入该组。

### 下载一个已经制作好的镜像

	$ docker pull ubuntu

这条命令会从Docker Hub上搜索ubuntu镜像，然后下载到本里镜像缓存中去。


	Note:
	When the image is successfully downloaded, you see a 12 character hash 539c0211cd76: Download complete which is the short form of the image ID. These short image IDs are the first 12 characters of the full image ID - which can be found using docker inspect or docker images --no-trunc=true.

### 创建一个带有交互窗口的container


	$ docker run -i -t ubuntu /bin/bash

—i参数表示启动了一个可以交互的容器，—t参数表示创建了一个附带标准输入和输出的pseudo－TTY窗口

如果想要退出tty窗口，使用 Ctrl-p + Ctrl-q指令，容器将会退出并且会持续保持一个停止的状态。如果想查看所有状态的容器，可以使用docker ps －a 指令。

### 绑定容器到另外一个主机/端口或者一个Unix Socket
	Warning: Changing the default docker daemon binding to a TCP port or Unix docker user group will increase your security risks by allowing non-root users to gain root access on the host. Make sure you control access to docker. If you are binding to a TCP port, anyone with access to that port has full Docker access; so it is not advisable on an open network.

使用 -H 能够让Docker deamon监听特殊的IP和端口。默认情况下，它将监听unix:///var/run/docker.sock以仅允许root进行本地连接。你可以将它设置为0.0.0.0:2375或者是指定的主机IP以供所有人连接，但是并不建议这么做，因为这将使某些无聊的人也获得deamon运行主机root的访问权限。

类似的，Docker客户端也可以使用 —H 连接一个指定端口

-H 使用以下格式来分配主机和端口 :

tcp://[host][:port][path] or unix://path

例如:

* tcp://host:2375 -> TCP connection on host:2375
* tcp://host:2375/path -> TCP connection on host:2375 and prepend path to all requests
* unix://path/to/socket -> Unix socket located at path/to/socket


当—H参数为空时，将被默认认为没有—H参数传进来

-H also accepts short form for TCP bindings:

host[:port] or :port

Docker以daemon模式运行:

	$ sudo <path to>/docker daemon -H 0.0.0.0:5555 &

下载一个ubuntu镜像:

	$ docker -H :5555 pull ubuntu

如果你想同时监听TCP和Unix Socket 你可以添加多个 －H

	# Run docker in daemon mode
	$ sudo <path to>/docker daemon -H tcp://127.0.0.1:2375 -H unix:///var/run/docker.sock &
	# Download an ubuntu image, use default Unix socket
	$ docker pull ubuntu
	# OR use the TCP port
	$ docker -H tcp://127.0.0.1:2375 pull ubuntu

### 起一个持续工作的进程

	# Start a very useful long-running process
	$ JOB=$(docker run -d ubuntu /bin/sh -c "while true; do echo Hello world; sleep 1; done")

	# Collect the output of the job so far
	$ docker logs $JOB

	# Kill the job
	$ docker kill $JOB
### 监听容器

	$ docker ps # Lists only running containers
	$ docker ps -a # Lists all containers

### 管理容器

	# Start a new container
	$ JOB=$(docker run -d ubuntu /bin/sh -c "while true; do echo Hello world; sleep 1; done")

	# Stop the container
	$ docker stop $JOB

	# Start the container
	$ docker start $JOB

	# Restart the container
	$ docker restart $JOB

	# SIGKILL a container
	$ docker kill $JOB

	# Remove a container
	$ docker stop $JOB # Container must be stopped to remove it
	$ docker rm $JOB

### 在一个TCP端口上绑定一个服务

	# Bind port 4444 of this container, and tell netcat to listen on it
	$ JOB=$(docker run -d -p 4444 ubuntu:12.10 /bin/nc -l 4444)

	# Which public port is NATed to my container?
	$ PORT=$(docker port $JOB 4444 | awk -F: '{ print $2 }')

	# Connect to the public port
	$ echo hello world | nc 127.0.0.1 $PORT

	# Verify that the network connection worked
	$ echo "Daemon received: $(docker logs $JOB)"

### 提交（保存）一个容器的状态到一个镜像文件中

当提交（commit）容器时，docker仅保存源镜像与当前镜像的差异，入藕想列出镜像，请使用docker images指令

	# Commit your container to a new named image
	$ docker commit <container> <some_name>

	# List your images
	$ docker images

