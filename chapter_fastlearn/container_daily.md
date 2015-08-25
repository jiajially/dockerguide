# 管理容器工作
	
### 概览

我们用docker run指令来运行一个容器:

交互容器跑在前端.
守护进程跑在后台.
一些常用管理容器的命令:

docker ps - 列出容器.
docker logs - 输出容器日志.
docker stop - 停止运行容器.

Docker客户端非常简单，你只需要需要输入一些带有一系列参数的指令就可以：

	# Usage:  [sudo] docker [command] [flags] [arguments] ..
	# Example:
	$ docker run -i -t ubuntu /bin/bash

我们以docker version为例查看当前安装的docker客户端以及deamon进程信息

	$ docker version

这条指令不仅提供啦docker客户端以及deamon进程的版本信息，同时也提供了所用go语言的信息。

	Client:
	  Version:      1.8.1
	  API version:  1.20
	  Go version:   go1.4.2
	  Git commit:   d12ea79
	  Built:        Thu Aug 13 02:35:49 UTC 2015
	  OS/Arch:      linux/amd64

	Server:
	  Version:      1.8.1
	  API version:  1.20
	  Go version:   go1.4.2
	  Git commit:   d12ea79
	  Built:        Thu Aug 13 02:35:49 UTC 2015
	  OS/Arch:      linux/amd64

### 获取docker命令行帮助

如果你想展示一些帮助信息，可以使用：

	$ docker --help

如果你想了解具体参数的使用方法，可以参照如下使用：

	$ docker attach --help

	Usage: docker attach [OPTIONS] CONTAINER

	Attach to a running container

	--help=false        Print usage
	--no-stdin=false    Do not attach stdin
	--sig-proxy=true    Proxy all received signals to the process


### 在docker中跑一个web应用程序

现在你已经掌握了一些基础了，来深入一下吧。之前跑的一些应用没什么实际用途，让们来跑一个web应用试试吧。

这个web应用包含里 python 应用：

	$ docker run -d -P training/webapp python app.py
	
以上指令中包含两个参数：
	
	－d 让容器在后台运行
	－P 分配一个主机端口到容器端口的映射以供外部访问


training/webapp是一个构建好的镜像，里面包含了一个简单的Python Flask web应用程序。

### 查看web应用程序状态
我们利用docker ps 来查看容器状态

	$ docker ps -l
	CONTAINER ID  IMAGE                   COMMAND       CREATED        STATUS        PORTS                    NAMES
	bc533791f3f5  training/webapp:latest  python app.py 5 seconds ago  Up 2 seconds  0.0.0.0:49155->5000/tcp  nostalgic_morse

其中指令包含了一个－l参数，这将展示最近一次创建的容器的状态。


	Note: 默认的 docker ps 不加参数将只会展现正在运行对容器，使用docker ps －a 将展现所有状态的容器

在POTS栏中，我们可以查看到端口映射。

	PORTS
	0.0.0.0:49155->5000/tcp

当我们使用－P参数时，docker将会给主机指定容器暴露的所有端口。

在这个例子中，容器暴露里5000端口，主机随机分配了一个49155端口与之映射。

网络端口绑定在Docker是可配置性非常高的。—P参数是随机指定一个32768～61000的主机端口与容器绑定，而－p则是指定一个端口与容器绑定：

	$ docker run -d -p 80:5000 training/webapp python app.py
	
这条指令将绑定主机的80端口与容器的5000端口。

我们来看看49155端口上的web应用吧！

![image](../images/webapp.jpg)

### 快捷查看网络端口
用docker ps命令可以查看端口映射，此外，docker海提供了另一种便捷的方式，用docker port命令

	$ docker port nostalgic_morse 5000
	0.0.0.0:49155

### 查看web应用日志

使用docker logs 可以查案容器内部的日志记录这样可以让我们清楚地看到容器运行的状态和历史纪录：

	$ docker logs -f nostalgic_morse
	* Running on http://0.0.0.0:5000/
	10.0.2.2 - - [23/May/2014 20:16:31] "GET / HTTP/1.1" 200 -
	10.0.2.2 - - [23/May/2014 20:16:31] "GET /favicon.ico HTTP/1.1" 404 -

这里加了一个－f参数，可以查看容器标准输出。这里展现了该web应用在5000端口上的日志信息。

### 查看web应用容器中的进程
根据容器的日志信息，我们可以利用 docker top 指令查看容器内的进程信息

	$ docker top nostalgic_morse
	PID                 USER                COMMAND
	854                 root                python app.py
这里我们在容器里看到了刚才我们运行的 python app.py命令。

### 审查（inspect）web应用容器

利用docker inspect 查看一个容器的详细信息（包括运行状态）或者镜像的配置信息，展现出来的是一个json文件。

	$ docker inspect nostalgic_morse
	Let’s see a sample of that JSON output.

	[{
	    "ID": "bc533791f3f500b280a9626688bc79e342e3ea0d528efe3a86a51ecb28ea20",
	    "Created": "2014-05-26T05:52:40.808952951Z",
	    "Path": "python",
	    "Args": [
	       "app.py"
	    ],
	    "Config": {
	       "Hostname": "bc533791f3f5",
	       "Domainname": "",
	       "User": "",
		. . .

我们可以使用－f参数里筛选出需要的信息

	$ docker inspect -f '{{ .NetworkSettings.IPAddress }}' nostalgic_morse
	172.17.0.5
	
### 停止web应用容器
现在我们尝试停止我们刚才启用的容器，容器名称: nostalgic_morse.

	$ docker stop nostalgic_morse
	nostalgic_morse

我们可以使用docker ps 来查看是否成功

	$ docker ps -l
	
### 重启web应用容器

重启一下试试：

	$ docker start nostalgic_morse
	nostalgic_morse

用docker ps －l命令或者打开浏览器产看变化	

### 删除web应用容器

我们可以利用docker rm来删除一个容器，但有时候会出现以下错误：

	$ docker rm nostalgic_morse
	Error: Impossible to remove a running container, please stop it first or use -f
	2014/05/24 08:12:56 Error: failed to remove one or more containers


怎么回事? 原来删除的是一个正在运行的容器. 所以我们要做的是stop it&remove it

	$ docker stop nostalgic_morse
	nostalgic_morse
	$ docker rm nostalgic_morse
	nostalgic_morse

官方建议

	Note: Always remember that removing a container is final!

