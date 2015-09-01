## 配置DNS

怎样为Docker提供的每一个容器进行主机名和DNS配置，而不必建立自定义镜像并将主机名写
到里面？它的诀窍是覆盖三个至关重要的在/etc下的容器内的虚拟文件，那几个文件可以写入
新的信息。你可以在容器内部运行mount看到这个：

    $$ mount
    ...
    /dev/disk/by-uuid/1fec...ebdf on /etc/hostname type ext4 ...
    /dev/disk/by-uuid/1fec...ebdf on /etc/hosts type ext4 ...
    /dev/disk/by-uuid/1fec...ebdf on /etc/resolv.conf type ext4 ...
    ...


HCP配置之后，保持resolv.conf的数据到所有的容器中。Docker怎样维护在容器内的这些文件从Docker的一个版本到下一个版本的具体细节，你应该抛开这些单独的文件本身并且使用下面的Docker选项代替。

有四种不同的选项会影响容器守护进程的服务名称。
1. -h HOSTNAME 或 --hostname=HOSTNAME
--设置容器的主机名，仅本机可见。这种方式是写到/etc/hostname ，以及/etc/hosts 文件中，作为容器主机IP的别名，并且将显示在容器的bash中。不过这种方式设置的主机名将不容易被容器之外可见。这将不会出现在 docker ps 或者 其他的容器的/etc/hosts 文件中。

        $ sudo docker run --hostname 'myhost' -it centos
	    [root@myhost /]# cat /etc/hosts
	    172.17.0.7	myhost
	    …

2. --link=CONTAINER_NAME:ALIAS --使用这个选项去run一个容器将在此容器的/etc/hosts文件中增加一个主机名ALIAS，这个主机名是名为CONTAINER_NAME 的容器的IP地址的别名。这使得新容器的内部进程可以访问主机名为ALIAS的容器而不用知道它的IP。--link= 关于这个选项的详细讨论请看：[容器互联](../chapter_fastlearn/docker_run/--link.md)

3. --dns=IP_ADDRESS --设置DNS服务器的IP地址，写入到容器的/etc/resolv.conf文件中。当容器中的进程尝试访问不在/etc/hosts文件中的主机A时，容器将以53端口连接到IP_ADDRESS这个DNS服务器去搜寻主机A的IP地址。

	    $  sudo docker run -it --dns=192.168.5.1  centos
	    [root@6a38049c9052 /]# cat /etc/resolv.conf
	    nameserver 192.168.5.1

4. --dns-search=DOMAIN --设置DNS服务器的搜索域，以防容器尝试访问不完整的主机名时从中检索相应的IP。这是写入到容器的/etc/resolv.conf文件中的。当容器尝试访问主机 host，而DNS搜索域被设置为  example.com ,那么DNS将不仅去查寻host主机的IP，还去查询host.example.com的IP。

	    $  sudo docker run -it --dns-search=www.domain.com  centos
	    [root@ae0e9e99596f /]# cat /etc/resolv.conf
	    nameserver 192.168.4.1
	    search www.mydomain.com


在docker中，如果启动容器时缺少以上最后两种选项设置时，将使得容器的/etc/resolv.conf文件看起来**和宿主主机的/etc/resolv.conf文件一致**。这些选项将修改默认的设置。(本宿主机在实验时有一行“nameserver 192.168.4.1”，所以默认容器的配置会与宿主机一样。)
