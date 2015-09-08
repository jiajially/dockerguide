
## 容器间通信



容器在使用Docker的时候我们会常常碰到这么一种应用，就是我需要两个或多个容器，其中某些容器需要使用另外一些容器提供的服务。所以，我们要考虑的问题时如何建立两个容器间通信。

**容器的连接（linking）**

系统是除了端口映射外，另一种跟容器中应用交互的方式。
该系统会在源和接收容器之间创建一个隧道，接收容器可以看到源容器指定的信息。

首先我们先创建一个容器(这里我只是用作示范,没有使用官方示例的镜像，所谓但数据容器内并没有提供数据服务，官方例子我举出来也没啥意思)

创建数据访问容器db：

	$ sudo docker run -idt --name=db centos
	600886c7c69dc4979bdfee19d82331879d71835f794db110eb3b5ea3c164bd30

使用--link=name:alias name就是要访问的目标机器，alias就是自定的别名

	$ sudo docker run -it --name=web --link=db:test_link centos
	[root@8d92293a65e9 /]# cat /etc/hosts
	172.17.0.12	8d92293a65e9
	127.0.0.1	localhost
	::1	localhost ip6-localhost ip6-loopback
	fe00::0	ip6-localnet
	ff00::0	ip6-mcastprefix
	ff02::1	ip6-allnodes
	ff02::2	ip6-allrouters
	172.17.0.11	test_link 600886c7c69d db

我们看到容器内的hosts 内多了一条信息

    172.17.0.11	test_link 600886c7c69d db

这就意味着我们可以访问到db容器进行通信

