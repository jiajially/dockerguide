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

#####1、环境变量


当link容器时，docker会创建许多环境变量. Docker会自动创建环境变量到目标容器中去. 它也将通过docker暴露源容器的所有环境变量. 这些变量来自:

Dockerfile中ENV命令定义的环境变量

在启动源容器中使用 －e 、--env以及  --env－file参数附加的环境变量。

这些环境变量使程序从相关的目标容器中发现源容器。

	Warning: It is important to understand that all environment variables originating from Docker within a container are made available to any container that links to it. This could have serious security implications if sensitive data is stored in them.

Docker为每一个在--link参数中的容器设置了一个 <alias>_NAME 环境变量 。例如，一个web容器通过--link db：webdb连 接db容器，将会在web容器中创建一个WEBDB_NAME=/web/webdb环境变量

Docker为源容器暴露的端口限定了一组环境变量，每一个环境变量具有唯一前缀形式：

	<name>_PORT_<port>_<protocol>

前缀的构成:

* <name> 是--link :后面的参数(例如, webdb)
* <port>就是暴露的端口号
* <protocol> TCP／UDP

Docker 利用这前缀格式定义了三个不同的环境变量:

prefix_ADDR 变量包含了来自URL的IP地址, for example 

	WEBDB_PORT_5432_TCP_ADDR=172.17.0.82.

prefix_PORT 变量仅包含了URL的端口号, for example 

	WEBDB_PORT_5432_TCP_PORT=5432.

prefix_PROTO 参数包含URL的传输协议, for example 

	WEBDB_PORT_5432_TCP_PROTO=tcp.

如果容器暴露多个端口，Docker将会为每个端口创建三个环境变量。算术题：如果容器暴露4个端口，将会创建多少个环境变量？答对了，是12个哦！每个端口三个环境变量。

另外，Docker也要创建一个叫 <alias>_PORT 的环境变量。这个变量包含源容器URL首次暴露的IP和端口。该端口的“首次”定义为最低级数字的端口。例如，思考WEBDB_PORT=tcp://172.17.0.82:5432 变量，如果该端口同时用语TCP和UDP，则TCP将会被指定。（原文：Additionally, Docker creates an environment variable called <alias>_PORT. This variable contains the URL of the source container’s first exposed port. The ‘first’ port is defined as the exposed port with the lowest number. For example, consider the WEBDB_PORT=tcp://172.17.0.82:5432 variable. If that port is used for both tcp and udp, then the tcp one is specified.）

最后，Docker会把源容器中的环境变量暴露给目标容器作为环境变量。并且Docker会在目标容器为每个变量创建一个<alias>_ENV_<name>变量。这个变量的值被设置为启动源容器Docker所用到的值。（原文：Finally, Docker also exposes each Docker originated environment variable from the source container as an environment variable in the target. For each variable Docker creates an <alias>_ENV_<name> variable in the target container. The variable’s value is set to the value Docker used when it started the source container.）

回到之前的例子 database ,你可以使用env命令列出具体的容器环境变量： 

    $ docker run --rm --name web2 --link db:db training/webapp env
    . . .
    DB_NAME=/web2/db
    DB_PORT=tcp://172.17.0.5:5432
    DB_PORT_5432_TCP=tcp://172.17.0.5:5432
    DB_PORT_5432_TCP_PROTO=tcp
    DB_PORT_5432_TCP_PORT=5432
    DB_PORT_5432_TCP_ADDR=172.17.0.5
    . . .

你可以看到Docker利用许多有关源容器的信息创建了一些列的环境变量。每一环境变量都会带有指点定义的别名，DB_前缀。如果别名是db1，那么变量前缀也会变成DB1_。利用这线环境变量来配置应用用来在db容器上连接数据库。这样的连接方式稳定安全私有化。只有已获得连接的web容器才会有对db容器的访问权限。
 
关于环境变量的一些注意事项：

与修改/etc/hosts文件不同，在环境变量中存储的IP地址信息不回随着容器的重启而更新，建议利用hosts文件来解决连接容器的IP地址问题。

这些环境变量只是为容器的第一个process设置，某些deamon后台服务，例如sshd，只有当产生连接需求时才会设置。（原文：These environment variables are only set for the first process in the container. Some daemons, such as sshd, will scrub them when spawning shells for connection.）

##### 2、更新 /etc/hosts 文件
处理环境变量, docker在源文件中追加了host信息，这里向web容器追加:

	$ docker run -t -i --rm --link db:webdb training/webapp /bin/bash
	root@aed84ee21bde:/opt/webapp# cat /etc/hosts
	172.17.0.7  aed84ee21bde
	. . .
	172.17.0.5  webdb 6e5cdeb2d300 db

我们可以在容器中使用ping命令测试链接：

	root@aed84ee21bde:/opt/webapp# apt-get install -yqq inetutils-ping
	root@aed84ee21bde:/opt/webapp# ping webdb
	PING webdb (172.17.0.5): 48 data bytes
	56 bytes from 172.17.0.5: icmp_seq=0 ttl=64 time=0.267 ms
	56 bytes from 172.17.0.5: icmp_seq=1 ttl=64 time=0.250 ms
	56 bytes from 172.17.0.5: icmp_seq=2 ttl=64 time=0.256 ms


如果你重启源容器，连接依然存在：

	$ docker restart db
	db
	$ docker run -t -i --rm --link db:db training/webapp /bin/bash
	root@aed84ee21bde:/opt/webapp# cat /etc/hosts
	172.17.0.7  aed84ee21bde
	. . .
	172.17.0.9  db