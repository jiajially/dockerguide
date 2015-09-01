## 高级网络配置

当docker启动时，它会在宿主机器上创建一个名为docker0的虚拟网络接口。它会从RFC 1918定义的私有地址中随机选择一个主机不用的地址和子网掩码，并将它分配给docker0。

例如我启动docker几分钟后它选择了172.17.42.1/16－一个16位的子网掩码为主机和它的容器提供了65,534个ip地址。

***注意: 本文讨论了Docker的高级网络配置和选项。通常你不会用到这些。如果你想查看一个较为简单的Docker网络介绍和容器概念介绍来着手，请参见[Dockers快速入门](chapter_fastlearn/README.md).***

但docker0并不是正常的网络接口。它只是一个在绑定到这上面的其他网卡间自动转发数据包的虚拟以太网桥。它可以使容器与主机相互通信。每次Docker创建一个容器，它就会创建**一对对等接口(peer interface)**，类似于一个管子的两端－在这边可以收到另一边发送的数据包。Docker会将对等接口中的一个做为eth0接口连接到容器上，并使用类似于vethAQI2QT这样的惟一名称来持有另一个，该名称取决于主机的命名空间。通过将所有veth＊接口绑定到docker0桥接网卡上，Docker在主机和所有Docker容器间创建一个**共享的虚拟子网**。

本文将会讲解使用Docker选项的所有方式，并且在高级模式下使用纯linux网线配置命令来调整，补充，或完全替代Docker的默认网络配置。

#### Docker选项快速指南


这里有一份关于Docker网络配置的命令行选项列表，省去您查找相关资料的麻烦。

一些网络配置的命令行选项只能在服务器启动时提供给Docker服务器。并且一旦启动起来就无法改变。

一些网络配置命令选项只能在启动时提供给Docker服务器，并且在运行中不能改变:

* **-b BRIDGE或--bridge=BRIDGE**— see    [建立自己的网桥](pro_docker0.md)

* **--bip=CIDR**— see    [定制docker0](pro_docker0.md)

* **-H SOCKET...或--host=SOCKET...**—   它看起来像是在设置容器的网络，但实际却恰恰相反：它告诉Docker服务器要接收命令的通道，例如“run container"和"stop container"。

* **--icc=true|false**— see    [容器间通信](pro_link_containers.md)

* **--ip=IP_ADDRESS**— see    [绑定容器端口](pro_bind_port.md)

* **--ip-forward=true|false**— see    [容器间通信](pro_link_containers.md)

* **--iptables=true|false**— see   [容器间通信](pro_link_containers.md)

* **--mtu=BYTES**— see    [定制docker0](pro_docker0.md)

有两个网络配置选项可以在启动时或调用docker run时设置。当在启动时设置它会成为docker run的默认值:

* **--dns=IP_ADDRESS...**— see    [配置DNS](pro_DNS.md)

* **--dns-search=DOMAIN...**— see    [配置DNS](pro_DNS.md)

最后，一些网络配置选项只能在调用docker run时指出，因为它们要为每个容器做特定的配置:

* **-h HOSTNAME或--hostname=HOSTNAME**— see    [配置DNS](pro_DNS.md) 和  [Docker与容器连接原理]()

* **--link=CONTAINER_NAME:ALIAS**— see   [配置DNS](pro_DNS.md)  和
[容器间通信](pro_link_containers.md)

* **--net=bridge|none|container:NAME_or_ID|host**— see   [Docker与容器连接原理]()

* **-p SPECor--publish=SPEC**— see    [绑定容器端口](pro_bind_port.md)

* **-P或--publish-all=true|false**— see    [绑定容器端口](pro_bind_port.md)


