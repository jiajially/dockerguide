## 定制docker0

默认地，docker服务会在linux内核新建一个网络桥接docker0，使得物理主机和其他虚拟网络接口之间可以传递发送数据包，因此，这表现如一个独立的网络。

docker0有一个IP地址和子网掩码，使得物理主机可以从容器的桥接网络接收和发送数据包。并且给这个桥接网络一个MTU（最大传输单元）或者说网络接口允许的最大包长度-例如1,500 bytes 或者从docker的宿主主机上的网络接口拷贝的数值。在服务启动的时候两者都是可配置的：

* --bip=CIDR— 为docker0桥接网络提供一个特殊的IP地址和一个子网掩码, 使用标准的 CIDR 记法例如192.168.1.5/24.
* --mtu=BYTES— 从写docker0的最大数据包长度。

在ubuntu系统上，你可以增加以上的配置到 /etc/default/docker 文件中的DOCKER_OPTS参数中，然后重启docker服务。

当你有一个或多个正常运行的容器时，你可以通过在主机上运行brct1命令，观察interfaces列的输出，来确定Docker已经将这些容器正确地连接到docker0网桥。下面是一个连接了两个不同容器的主机：

	$ sudo brctl show
	bridge name     bridge id            STP enabled        interfaces
	docker0         8000.3a1d7362b4ee       no              veth65f9
	                                                        vethdda6


最后，每次新建一个容器的时候都会用到docker0 桥接网络。每次在执行docker run命令新建一个容器的时候，docker从可利用的桥接网络中随机选择一个未被使用的IP地址，以及使用桥接网络的子网掩码，用来配置容器 eth0网络接口。docker宿主主机的IP地址被docker容器作为默认的网关。

	$ docker run -i -t --rm base /bin/bash
	[root@e623fd7cf734 /] ip addr show eth0
	24: eth0: <BROADCAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    	 link/ether 32:6f:e0:35:57:91 brd ff:ff:ff:ff:ff:ff
    	 inet 172.17.0.3/16 scope global eth0
       	    valid_lft forever preferred_lft forever
    	 inet6 fe80::306f:e0ff:fe35:5791/64 scope link
       	    valid_lft forever preferred_lft forever
	[root@e623fd7cf734 /] ip route
	default via 172.17.42.1 dev eth0
	172.17.0.0/16 dev eth0  proto kernel  scope link  src 172.17.0.3
	[root@e623fd7cf734 /] exit

###建立你自己的桥接网络

如果你希望建立完整的自己的桥接网络，你可以在启动docker之前用 -b BRIDGE 或者 --bridge=BRIDGE选项参数高数docker使用你自己的桥接网络。


如果你已经用docker0启动docker了，你需要停止docker服务然后移除docker0.

	$ sudo service docker stop
	$ sudo ip link set dev docker0 down
	$ sudo brctl delbr docker0
	$ sudo iptables -t nat -F POSTROUTING

然后，在启动docker服务之前，新建你自己的桥接网络，写上你想要的配置。接下来我们新建一个简单的桥接网络，刚好用这些选项来定做docker0 ，这刚好足够说明这个技术。

	$ sudo brctl addbr bridge0
	$ sudo ip addr add 192.168.4.1/24 dev bridge0
	$ sudo ip link set dev bridge0 up
	Chain POSTROUTING (policy ACCEPT)
	target     prot opt source               destination
	MASQUERADE  all  --  192.168.4.0/24       0.0.0.0/0

创建好网桥之后在docker配置文件中写入启动参数，或者手动指定：

	$ sudo docker -d --bridge=bridge0

这样我们自己定义的网桥就做好了，当利用docker run启动容器时，会自动绑定网络到bridge0上：

	$ sudo docker run -it --rm centos
	[root@e623fd7cf734 /]# ip addr
	1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN
    	link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
		inet 127.0.0.1/8 scope host lo
       		valid_lft forever preferred_lft forever
    	inet6 ::1/128 scope host
       		valid_lft forever preferred_lft forever
	40: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP
    	 link/ether 02:42:c0:a8:04:02 brd ff:ff:ff:ff:ff:ff
    	 inet 192.168.4.2/24 scope global eth0
       		valid_lft forever preferred_lft forever
         inet6 fe80::42:c0ff:fea8:402/64 scope link
       		valid_lft forever preferred_lft forever

我们可以看到新的网桥已经生效。
