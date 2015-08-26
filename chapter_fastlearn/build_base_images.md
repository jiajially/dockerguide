## 构建基础镜像

我将应用打包到镜像中形成我们所需的镜像，往往需要一个基础的镜像作为我们应用服务的外部环境，那么问题来了，基础镜像从何而来？官方推荐的是直接从官网仓库pull一个，但由于官网被墙的比较厉害，所以这里介绍一些官方提供以及个人方法。

1.使用Debootstrap来创建Ubuntu的base image

	$ sudo debootstrap raring raring > /dev/null
	$ sudo tar -C raring -c . | docker import - raring
	a29c15f1bf7a
	$ docker run raring cat /etc/lsb-release
	DISTRIB_ID=Ubuntu
	DISTRIB_RELEASE=13.04
	DISTRIB_CODENAME=raring
	DISTRIB_DESCRIPTION="Ubuntu 13.04"

在docker github上有更多有关基础镜像的介绍

* BusyBox
* CentOS / Scientific Linux CERN (SLC) on Debian/Ubuntu or on CentOS/RHEL/SLC/etc.
* Debian / Ubuntu

2.使用scratch创建base image
在Docker registry中有一个scratch，你可以pull拉取下来，

	$ sudo docker pull scratch

甚至可以自己制作

	$ tar cv --files-from /dev/null | docker import - scratch
Scratch镜像很赞，它简洁、小巧而且快速， 它没有bug、安全漏洞、延缓的代码或技术债务。这是因为它基本上是空的。除了Docker添加了点的metadata (译注：元数据为描述数据的数据)。总之它是非常小的一个Docker镜像。
为Scratch镜像创建内容，具体Dockerfile命令如下:

	FROM scratch
	ADD hello /
	CMD ["/hello"]

3.下载官方提提供的OS的tar文件
到[OPENVZ](http://wiki.openvz.org/Download/template/precreated)上下载基础包然后使用docker limport 加载到本地镜像，这里以ubuntu14.04 为例，从openvz下载一个ubuntu14.04的模板：

	wget http://download.openvz.org/template/precreated/ubuntu-14.04-x86_64.tar.gz
    cat ubuntu-14.04-x86_64-minimal.tar.gz | docker import - ubuntu:base
