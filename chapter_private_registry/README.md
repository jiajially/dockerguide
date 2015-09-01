## Docker 私有仓库

如果你想玩转docker，一个私有仓库是必不可少的。
本文将会搭建一个简易的私有仓库以供参考。

本文例子的主机地址是

	192.168.4.121

####第一步 获取官方工具

官方为我们提供了一个创建仓库的工具，它是以镜像文件形式存储在官方仓库中，我们可以把它拉下来用。

	$ sudo docker pull registry

#### 第二步 启动仓库

我们现在启动它,指定主机5000端口绑定

	$ sudo docker -d  -p 5000:5000 registry

#### 第三步 验证

这时 输入

	$ curl http://192.168.4.160:5000/v1/search
	{"num_results": 0, "query": "", "results": [］}

产生如上结果说明仓库服务可用。

至此，仓库的简易配置就结束了。

但是问题来了，我们pull，push的文件在哪？哈哈，原来默认情况下，会将仓库存放于容器内的/tmp/registry目录下，这样如果容器被删除，则存放于容器中的镜像也会丢失，所以我们一般情况下会指定本地一个目录挂载到容器内的/tmp/registry下：

	$ sudo docker run -d -p 5000:5000 -v /opt/data/registry:/tmp/registry registry

我们将本机的centos镜像tag一下

	$ sudo docker tag centos 192.168.4.160:5000/centos:latest

这时将本地仓库push到私有仓库中去

	$ sudo docker push 192.168.4.160:5000/centos

可是失败了

	2015/08/05 11:01:17 Error: Invalid registry endpoint https://192.168.4.160:5000/v1/: Get https://192.168.4.160:5000/v1/_ping: dial tcp 192.168.4.160:5000: connection refused. If this private registry supports only HTTP or HTTPS with an unknown CA certificate, please add `--insecure-registry 192.168.4.160:5000` to the daemon's arguments. In the case of HTTPS, if you have access to the registry's CA certificate, no need for the flag; simply place the CA certificate at /etc/docker/certs.d/192.168.4.160:5000/ca.crt

这是因为docker采用了安全机制，若想跳过此安全验证，可以在docker配置文件中添加--insecure-registry 192.168.4.160:5000该参数标记该仓库允许不安全连接，这在deamon中提到过。

修改/etc/sysconfig/docker文件

	OPTIONS='--selinux-enabled --insecure-registry 192.168.4.160:5000'

这样应该就可以了。

	$ sudo docker push 192.168.4.160:5000/progrium
	The push refers to a repository [192.168.4.160:5000/progrium] (len: 1)
	Sending image list
	Pushing repository 192.168.4.160:5000/progrium (1 tags)
	511136ea3c5a: Image successfully pushed
	d7ac5e4f1812: Image successfully pushed
	2f4b4d6a4a06: Image successfully pushed
	83ff768040a0: Image successfully pushed
	6c37f792ddac: Image successfully pushed
	e54ca5efa2e9: Image successfully pushed
	2d07e6ffe5ad: Image successfully pushed
	a2de3cd83939: Image successfully pushed
	8d2c32294d38: Image successfully pushed
	873c28292d23: Image successfully pushed
	Pushing tag for rev [873c28292d23] on {http://192.168.4.160:5000/v1/repositories/progrium/tags/latest}

查看仓库内容

	$ curl 192.168.4.100/v1/search
	{"num_results": 5, "query": "", "results": [{"description": null, "name": "library/centos"}, {"description": null, "name": "library/ubuntu"}, {"description": null, "name": "library/busybox"}, {"description": null, "name": "library/postgres"}, {"description": "", "name": "library/progrium"}]}

说明成功了！！
