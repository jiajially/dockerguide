## 容器间的通信
容器的通信相当重要，这里讲解了一通信方式。
### 容器与宿主机器采用端口映射的方式通信

之前的例子

	$ docker run -d -P training/webapp python app.py

我们可以看到端口映射状态：

	$ docker ps nostalgic_morse
	CONTAINER ID  IMAGE                   COMMAND       CREATED        STATUS        PORTS                    NAMES
	bc533791f3f5  training/webapp:latest  python app.py 5 seconds ago  Up 2 seconds  0.0.0.0:49155->5000/tcp  nostalgic_morse

我们也可以指定端口映射：

	$ docker run -d -p 80:5000 training/webapp python app.py

－p参数还有其他指定方法：

	ip:hostPort:containerPort  映射指定IP的指定端口

	ip::containerPort          映射指定IP任意端口
	
	hostPort:containerPort。	映射所有主机IP的指定端口
	
### 采用link方式通信

名字的重要性
为了方便建立连接，通常需要为容器起一个name ，如果不起，你会发现系统会自动分配一个名字。

此外，起一个自定义的名字让你很容易地记住它并切可以方便的管理

	$ docker run -d -P --name web training/webapp python app.py

查看容器

	$ docker ps -l
	CONTAINER ID  IMAGE                  COMMAND        CREATED       STATUS       PORTS                    NAMES
	aed84ee21bde  training/webapp:latest python app.py  12 hours ago  Up 2 seconds 0.0.0.0:49154->5000/tcp  web

你也可以使用docker inspect 命令查看容器名称。

	Note: Container names have to be unique. That means you can only call one container web. If you want to re-use a container name you must delete the old container (with docker rm) before you can create a new container with the same name. As an alternative you can use the --rm flag with the docker run command. This will delete the container immediately after it is stopped.


Links 允许容器发现另一个容器，并在期间建立一个安全的通道以便交换数据。 使用－－link参数来创建一个连接。
	
	$ docker run -d --name db training/postgres
	
创建一个web连接到db数据库容器.

	$ docker run -d -P --name web --link db:db training/webapp python app.py

参数格式如下:

	--link <name or id>:alias

alias代表你 为这个链接起的一个别名。

	--link <name or id>

该参数将匹配容器name 并建立连接

	$ docker run -d -P --name web --link db training/webapp python app.py

使用docker inspect 定位:
	
	$ docker inspect -f "{{ .HostConfig.Links }}" web
	[/db:/web/db]

你可以看到web容器已经与db容器连接

连接容器docker究竟做了什么？目前已经知道，一个创建在源容器与目标容器间的连接允许源容器提供信息给目标容器。在上述例子中，目标容器web可以获取源容器db提供的信息。为了实现这项功能，docker在这两个容器之间创建了一个稳定的通道，并且不会暴露任何端口，你不需要在创建容器时加上－p或－P参数。这就是link方式的最大好处。

docker通过以下两种方式完成此项工作

