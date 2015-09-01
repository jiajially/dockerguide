### 容器与外部网络通信

决定容器是否可以访问外网取决于两个因素：
1. 主机是否会转发IP数据包。这取决于转发系统内的ip_forward这个参数的配置。如果ip_forward值为1，数据包就可以被转发。Docker会使用--ip_forward=true的默认设置，一旦你docker服务启动docker会将系统的ip_forward的值修改为1。使用-ip_forward=false对系统没有改变。通常设置方法如下：

	    $ sysctl net.ipv4.conf.all.forwarding
        net.ipv4.conf.all.forwarding = 0
        $ sysctl net.ipv4.conf.all.forwarding=1
        $ sysctl net.ipv4.conf.all.forwarding
        net.ipv4.conf.all.forwarding = 1

    设置这个值很重要，它将决定你的容器是否可以与外部通信以及容器间的通信。在多网桥系统中，内嵌的容器也需要配置此项。

2. 你的iptables防火墙是否允许特殊连接。当deamon启动的时候，如果你设置--iptables=false，Docker将不会改变主机的iptables的防火墙规则。否则，Docker将追加规则到DOCKER filter chain

    Docker 不会删除或修改已经存在在DOCKER filter chain中的规则。这允许用户进一步创建限制访问容器的任何规则。

    Docker的默认规则是允许所有的外部IPs。如果只是想允许一个IP连接到容器，在DOCKER filter chain的顶部增加一条否定规则：

	    $ iptables -I DOCKER -i ext_if ! -s 8.8.8.8 -j DROP

    这将只允许 8.8.8.8这个IP连接到容器。

###容器间互相访问

决定容器能否互相访在系统层面上问取决于两个因素：
1. 网络的拓扑结构是否已经连接到容器的网络接口。默认情况下，Docker会把所有的容器附加到docker0网桥下，并为两个容器间的包传输提供路径。

2. iptables是否允许特殊连接?如果你把设置 --iptables=false,当守护进程启动时，Docker不会改变你的系统iptables规则。另外，如果你保留默认设置 --icc=true，Docker服务器或向FORWARD链添加一个带有全局ACCEPT策略的默认规则。如果不保留默认设置即--icc=false，系统会把策略设为DROP.

    使用docker都希望ip_forward 是打开的，至少使容器间的通讯成为可能。

    但是否同意 --icc=true 或者更改为 --icc=false 使得iptables 可以保护容器以及宿主主机不被任意地端口扫描、避免被已经被渗透的容器所访问，这是一个策略问题。
（在ubuntu，是编辑/etc/default/docker文件中的DOCKER_OPTS参数，然后重启docker服务）

**如果你选择最安全的设置 --icc=false ，那么当你想让它们彼此提供服务的时候如何让它们相互通讯？**

**答案是：**使用前文提到的 --link=CONTAINER_NAME:ALIAS 选项。如果docker守护进程正在以 --icc=false 和 --iptables=true 参数运行，当以选项 --link= 执行 docker run 命令时，docker服务将插入一部分 iptables ACCEPT 规则使得新容器可以连接其他容器所暴露出来的端口（此端口指前文在 Dockerfile 中提到的EXPOSE这一行）。更多详细文档介绍请看：[容器互联](chapter_fastlearn/docker_run/--link.md)。

**注意: **--link 选项中的 CONTAINER_NAME 的值必须是 docker自动分配的容器名称，比如stupefied_pare，或者是在执行docker run 的时候用--name=指定的容器名称. 这不能使一个docker无法识别的主机名

你可以在你的Docker主机上运行iptables命令，来观察FORWARD链是否有默认的ACCEPT或DROP策略
	# When --icc=false, you should see a DROP rule:
	$ sudo iptables -L -n
    ...
    Chain FORWARD (policy ACCEPT)
    target     prot opt source               destination
    DOCKER     all  --  0.0.0.0/0            0.0.0.0/0
    DROP       all  --  0.0.0.0/0            0.0.0.0/0
    ...
	# When a --link= has been created under --icc=false,
    # you should see port-specific ACCEPT rules overriding
    # the subsequent DROP policy for all other packets:
	$ sudo iptables -L -n
    ...
    Chain FORWARD (policy ACCEPT)
    target     prot opt source               destination
    DOCKER     all  --  0.0.0.0/0            0.0.0.0/0
    DROP       all  --  0.0.0.0/0            0.0.0.0/0
	Chain DOCKER (1 references)
    target     prot opt source               destination
    ACCEPT     tcp  --  172.17.0.2           172.17.0.3           tcp spt:80
    ACCEPT     tcp  --  172.17.0.3           172.17.0.2           tcp dpt:80
