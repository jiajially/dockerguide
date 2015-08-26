
## 数据卷容器


**--volumes-from **
顾名思义，就是从另一个容器当中挂载容器中已经创建好的数据卷。


如果你有一些持续更新的数据需要在容器之间共享，最好创建数据卷容器。
数据卷容器，其实就是一个正常的容器，专门用来提供数据卷供其它容器挂载的。
我们首先先创建一个数据卷容器

	$ sudo docker run -d -v /dbdata --name dbdata training/postgres echo Data-only container for postgres
	734273f9ec0b0cf743a79a9da7b27b269ff9c34a02ec17d3df158cf0e3cedcd2
	$ sudo docker ps -a
	CONTAINER ID        IMAGE              COMMAND                CREATED             STATUS                     PORTS         NAMES
	734273f9ec0b        training/postgres  "/docker-entrypoint.   17 seconds ago      Exited (0) 15 seconds ago                dbdata


发现创建好的数据卷容器是出于停止运行的状态，因为使用 --volumes-from 参数所挂载数据卷的容器自己并不需要保持在运行状态。

然后我们再创建容器挂载这个数据卷。


	$ sudo docker run -d --volumes-from dbdata --name db1 training/postgres
    $ sudo docker run -d --volumes-from dbdata --name db2 training/postgres
	$ sudo docker ps
	CONTAINER ID       IMAGE                COMMAND                CREATED             STATUS              PORTS               NAMES
	7348cb189292       training/postgres    "/docker-entrypoint.   11 seconds ago      Up 10 seconds       5432/tcp            db2
	a262c79688e8       training/postgres    "/docker-entrypoint.   33 seconds ago      Up 32 seconds       5432/tcp            db1

还可以使用多个 --volumes-from 参数来从多个容器挂载多个数据卷。 也可以从其他已经挂载了数据卷的容器来挂载数据卷。

如果删除了挂载的容器（包括 dbdata、db1 和 db2），数据卷并不会被自动删除。如果要删除一个数据卷，必须在删除最后一个还挂载着它的容器时使用 docker rm -v 命令来指定同时删除关联的容器。 这可以让用户在容器之间升级和移动数据卷。具体的操作将在下一节中进行讲解。



* ### 利用数据卷容器来备份、恢复、迁移数据卷






首先使用 --volumes-from 标记来创建一个加载 dbdata 容器卷的容器，并从本地主机挂载当前到容器的 /backup 目录。命令如下：


	$ sudo docker run --volumes-from dbdata -v $(pwd):/backup ubuntu tar cvf /backup/backup.tar /dbdata


容器启动后，使用了 tar 命令来将 dbdata 卷备份为本地的 /backup/backup.tar。
如果要恢复数据到一个容器，首先创建一个带有数据卷的容器 dbdata2。


	$ sudo docker run -v /dbdata --name dbdata2 ubuntu /bin/bash


然后创建另一个容器，挂载 dbdata2 的容器，并使用 untar 解压备份文件到挂载的容器卷中。


	$ sudo docker run --volumes-from dbdata2 -v $(pwd):/backup busybox tar xvf
	/backup/backup.tar
