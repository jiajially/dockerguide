# 管理容器数据
到目前为止，我们已经介绍了一些基本的docker概念，如何管理docker 镜像，以及了解网络和容器之间的联系。

在这一节中，我们将介绍如何管理容器数据。

docker管理数据的两种主要方式。

数据卷，以及数据卷容器。

数据卷是在一个或多个容器，它绕过Union File System的一个专门指定的目录。数据卷为持续共享数据提供了一些有用的功能：

* 在创建容器时，卷被初始化。如果容器的基础映像包含指定的数据装入点，现有的数据复制到在卷初始化新卷。
* 数据卷可以共享和容器之间重复使用。

* 改变数据卷将立刻生效（在所有挂载该容器中）

* 改变数据卷数据不会影响到容器。

* 即使容器本身被删除。但是数据卷依然存在。

* 数据卷的目的是持久化数据，独立于容器的生命周期。Docker因此不会自动删除卷，当你删除一个容器，也不会“垃圾回收”直到没有容器再使用。

### 添加一个数据卷
你可以在docker run时加上－v参数来添加一个数据卷，－v参数也可以使用多次，以挂载多个数据卷。

	$ docker run -d -P --name web -v /webapp training/webapp python app.py

这条命令将在容器中的／webapp文件夹创建一个数据卷存储数据。

你也可以在构建镜像时在Dockerfile里面定义。

数据卷默认的权限时读写，你也可以定义成只读。

	$ docker run -d -P --name web -v /opt/webapp:ro training/webapp python app.py

查找数据卷路径用docker inspect命令定位

	$ docker inspect web

我们可以看到保存在主机上数据卷的路径：
	
	...
	Mounts": [
	    {
	        "Name": "fac362...80535",
	        "Source": "/var/lib/docker/volumes/fac362...80535/_data",
	        "Destination": "/webapp",
	        "Driver": "local",
	        "Mode": "",
	        "RW": true
	    }
	]
...
并切看到权限RW是ture

### 挂载一个主机目录作为数据卷

挂载主机目录为数据卷，必须参照 －v hostPATH:containerPATH 这种格式 路径必须为绝对路径，以保证容器的 可移植性。
	
	$ docker run -d -P --name web -v /src/webapp:/opt/webapp training/webapp python app.py

上面的命令加载主机的 /src/webapp 目录到容器的 /opt/webapp 目录

docker数据卷的权限是读写，你也可以指定只读：

	$ docker run -d -P --name web -v /src/webapp:/opt/webapp:ro training/webapp python app.py


### 挂载一个数据文件作为数据卷

-v 标记也可以从主机挂载单个文件到容器中

	$ sudo docker run --rm -it -v ~/.bash_history:/.bash_history ubuntu /bin/bash
这样就可以记录在容器输入过的命令了。

如果直接挂载一个文件，很多文件编辑工具，包括 vi 或者 sed --in-place，可能会造成文件 inode 的改变，从 Docker 1.1 .0起，这会导致报错误信息。所以最简单的办法就直接挂载文件的父目录。

### 创建和挂载数据卷容器

如果你有一些持续更新的数据需要在容器之间共享，最好创建数据卷容器。

数据卷容器，其实就是一个正常的容器，专门用来提供数据卷供其它容器挂载的。

首先，创建一个命名的数据卷容器 dbdata：

	$ sudo docker run -d -v /dbdata --name dbdata training/postgres echo Data-only container for postgres

然后，在其他容器中使用 --volumes-from 来挂载 dbdata 容器中的数据卷。

	$ sudo docker run -d --volumes-from dbdata --name db1 training/postgres
	$ sudo docker run -d --volumes-from dbdata --name db2 training/postgres

还可以使用多个 --volumes-from 参数来从多个容器挂载多个数据卷。 也可以从其他已经挂载了数据卷的容器来挂载数据卷。

	$ sudo docker run -d --name db3 --volumes-from db1 training/postgres
*注意：使用 --volumes-from 参数所挂载数据卷的容器自己并不需要保持在运行状态。

如果删除了挂载的容器（包括 dbdata、db1 和 db2），数据卷并不会被自动删除。如果要删除一个数据卷，必须在删除最后一个还挂载着它的容器时使用 docker rm -v 命令来指定同时删除关联的容器。 这可以让用户在容器之间升级和移动数据卷。

### 备份、存储、移动数据卷

另一个非常有用大功能是利用数据卷容器进行备份、存储以及迁移操作。

备份
	
	$ docker run --volumes-from dbdata -v $(pwd):/backup ubuntu tar cvf /backup/backup.tar /dbdata		
然后新创建一个新的容器

	$ docker run -v /dbdata --name dbdata2 ubuntu /bin/bash
	
然后解压数据卷挂载到容器

	$ docker run --volumes-from dbdata2 -v $(pwd):/backup ubuntu cd /dbdata && tar xvf /backup/backup.tar
	