## Docker 创建网络步骤

Docker是正在发展中的，并会持续提升网络配置的逻辑。当前命令行是很难满足Docker新建容器时所需要的网络配置。

让我们回顾一些基础知识。

通讯的时候使用网际协议（IP），一个机器需要访问至少一个网络接口用来发送和接收包，路由表定义了通过接口可达IP地址范围。网络接口不一定非是物理设备。**实际上，在每一个Linux机器（和每个Docker容器内部）的lo回环接口都是有效的而且完全是虚拟的——Linux内核简单地拷贝回环（数据）包，直接从发送者的内存放入接收者的内存。**

Docker使用特殊的虚拟接口让容器在主机间通讯——成对的虚拟接口被叫做“peers”，它被链接到主机内核的内部，因此（数据）包能在他们之间传输。他们简单创建，待会儿我们将会看到。

Docker配置容器的步骤是：

1. 创建一对虚拟接口

2. 在主Docker主机内部给它一个唯一的名称，**比如veth65f9，绑定它到docker0或者Docker使用的任何网桥上**

3. 让其他的接口翻墙进入新的容器（已经提供了lo接口），在容器的独立和唯一网络接口命名空间内，**重新命名它为更漂亮的名字eth0，名称不要和其他的物理接口冲突。**

4. 设置接口的MAC地址，**具体使用--mac-address 命令指定或者随机一个**。

5. 在网桥的网络地址访问内给容器的eth0一个新的IP地址，**设置它的缺省路由为Docker主机在网桥上拥有的IP地址。**使用--default-gateway 设置默认路由来允许该地址向Docker deamon 来转发数据，否则将使用网桥定义的IP地址来转发（docker0）。除非自定义，否则MAC地址是根据IP来生成的。当一个新的容器使用已经分配过的IP（另一个具有不同MAC地址的容器）地址启动的时候，这将可以有效防止ARP缓存失效的问题。

这些步骤结束后，容器将立即拥有一个eth0（虚拟）网卡，并会发现它自己可以和其他的容器以及互联网通讯。

你可以使用 --net= 这个选项来执行 docker run 启动一个容器，这个选项有一下可选参数。

* --net=bridge— 默认选项，用网桥的方式来连接docker容器。

* --net=host— 高数docker跳过配置容器的独立网络栈。本质上来说，**这个参数告诉docker不去打包容器的网络层**。当然，docker 容器的进程仍然被限制在它自己独有的文件系统、进程列表以及其他资源中。一个快速命令 ip addr 将像你展示docker的网络，它是建立在docker 宿主主机上的，有完整的权限去访问宿主主机的网络接口。注意这不意味着docker容器可以去重新配置宿主主机的网络栈，重新配置是需要 --privaleged=true 这个选项参数的，但是这个选项参数会让docker容器打开大量的端口以及其他的系统的超级管理权限的进程。这也会允许容器去访问宿主主机的网络服务，比如 D-bus。这会使docker容器里的进程有有权限去做一些意想不到的事，比如重启你的宿主主机。所以要谨慎使用这个选项参数。

* --net=container:NAME_or_ID— 告诉docker让这个新建的容器使用已有容器的网络配置。这个新建的容器将配置新的自己的文件系统和进程列表以及其他资源限制，但是将共享这个指定的容器的网络IP地址以及端口号，使得这两个容器可以通过 loopback接口相互访问。
* --net=none— 告诉docker为新建的容器建立一个网络栈，但不对这个网络栈进行任何配置，在这个文档的最后将介绍如何让你去建立自定义的网络配置。

去了解以下这一步是非常必要的，如果你在建立容器的时候使用 --net=none  这个选项参数。以下是一些命令去去配置自定义网络，就好像你让docker完全去自己配置一样。


创建一个不带任何网络配置的网络：

	$ docker run -i -t --rm --net=none base /bin/bash
	root@63f36fc01b5f:/#

重新开启一个窗口，获取容器的pid以在var/run/netns/下便创建网络，以下会用到ip netns命令：

	$ docker inspect -f '{{.State.Pid}}' 63f36fc01b5f
	2778
	$ pid=2778
	$ sudo mkdir -p /var/run/netns
	$ sudo ln -s /proc/$pid/ns/net /var/run/netns/$pid

检查主机的网桥是否启用：

	$ ip addr show docker0
	21: docker0: ...
	inet 172.17.42.1/16 scope global docker0
	...
创建一对（peers）网络接口A和B并绑定，然后启动，其中A是在主机上，B放在容器里：

	$ sudo ip link add A type veth peer name B
	$ sudo brctl addif docker0 A
	$ sudo ip link set A up

覆盖容器的网络名字空间，并将B更改为eth0，以下步骤每执行完一次都可以到第一个窗口中查看网络配置状态（ip addr）：

	$ sudo ip link set B netns $pid
	$ sudo ip netns exec $pid ip link set dev B name eth0
	$ sudo ip netns exec $pid ip link set eth0 address 12:34:56:78:9a:bc
	$ sudo ip netns exec $pid ip link set eth0 up
	$ sudo ip netns exec $pid ip addr add 172.17.42.99/16 dev eth0
	$ sudo ip netns exec $pid ip route add default via 172.17.42.1

到这一步你的容器应该可以正常运行网络操作了。

当你最后退出shell以及清理掉这个容器的时候，这个容器的虚拟网络 eth0 将在网络接口A 被清除后被消除，也会自动在网桥docker0上销毁。所以不用你执行其他的命令，所有的东西将被清理。当然，是几乎所有的东西：

	# Clean up dangling symlinks in /var/run/netns
	find -L /var/run/netns -type l -delete

还要注意上面的脚本使用了现代的ip命令行替代旧的弃用的封装，**类似ipconfig和route，些老的命令行还是会一直呆在我们的容器内部工作。如果你嫌麻烦，ip addr命令行也可以只键入ip a。**

总之，**注意这个ip netns exec重要的命令行，它让我们以root用户进入内部并配置一个网络命名空间。**如果在容器内部运行，类似的命令行可能不会工作，因为安全容器化的部分是Docker剥离容器的处理过程，这个过程要正确地配置自己的网络。

**使用ip netns exec可以让我们完成配置，还避免了运行容器自身--privileged=true的危险步骤。
**
