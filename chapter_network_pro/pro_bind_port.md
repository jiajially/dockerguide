## 为主机绑定容器端口

默认情况下，Docker容器可以连接到外部区域，但外部区域不能连接到容器。在Docker启动时，由于它在主机上创建了一个**iptables伪装规则**，使得每一个输出连接看起来都是由主机IP地址建立起来的。

	# You can see that the Docker server creates a
	# masquerade rule that let containers connect
	# to IP addresses in the outside world:
	$ sudo iptables -t nat -L -n
	...
	Chain POSTROUTING (policy ACCEPT)
	target     prot opt source               destination
	MASQUERADE  all  --  172.17.0.0/16       0.0.0.0/0
	...

当调用docker run的时候，如果你想让容器接受输入连接，你需要提供特殊选项。这些选项的详细说明在 [Docker 快速入门](../chapter_fastlearn/README.md). 有两种方法可以实现。

首先，你可以提供 **-P 或者--publish-all=true|false **选项参数来执行 docker run 命令，这将会识别所有在dockerfile中暴露的端口或者--expose参数制定的端口，然后用检查端口映射，临时端口范围由/proc/sys/net/ipv4/ip_local_port_range内核参数配置，并且随机映射到 **32768 ～ 61000 **之间的主机端口。

更方便的操作是使用 **-p SPEC 或者--publish=SPEC** 选项，这两个选项让你明确的指定docker容器的端口映射到任意的主机端口中，不局限于32768 ～ 61000.

无论如何，你应该通过审查你的NAT表，去看看docker在你的网络占做了什么。

	# What your NAT rules might look like when Docker
	# is finished setting up a -P forward:
	$ iptables -t nat -L -n
	...
	Chain DOCKER (2 references)
	target     prot opt source               destination
	DNAT       tcp  --  0.0.0.0/0            0.0.0.0/0            tcp dpt:49153 to:172.17.0.2:80
	# What your NAT rules might look like when Docker
	# is finished setting up a -p 80:80 forward:
	Chain DOCKER (2 references)
	target     prot opt source               destination
	DNAT       tcp  --  0.0.0.0/0            0.0.0.0/0            tcp dpt:80 to:172.17.0.2:80


可以看到，docker暴露了这些容器的端口到通配IP地址：0.0.0.0 ，这个通配IP地址可以匹配宿主主机上任意一个可以进入的端口。如果你希望更多的限制，并且只允许容器服务通过特殊的宿主主机的外部网络接口来相互联系，那么你有两种选择。当你执行 docker run 命令时，你可以使用 **-p IP:host_port:container_port 或者 -p IP::port**来明确地绑定外部接口。

或者如果你希望dokcer永远转发到一个特殊的IP地址上，你可以编辑你的docker系统设置文件（ubuntu系统的设置方法为：编辑 /etc/default/docker文件，改写DOCKER_OPTS参数），增加选项 --ip=IP_ADDRESS 。**修改完之后记得重启你的docker服务。**

如果你希望更详细的指导，请参考：[Docker 快速入门](../chapter_fastlearn/README.md).
