
## -v 参数



参数的作用就是挂载一个文件目录到指定容器中去，实现容器中数据持久化。

* 数据卷是一个可以供一个或多个使用的特殊目录，它绕过UFS，可以提供很多有用的特性
   
    * 数据卷可以在容器之间共享和重用
    * 对数据卷的修改会立马生效
    * 对数据卷的更新，不会影响镜像
    * 卷会一直存在，直到没有容器使用
    

* **挂载目录**

在使用docker run时，加上-v参数可以创建一个数据卷挂载到目标容器中去，也可以多次使用该参数挂载多个数据卷。下面创建一个容器，挂载一个数据卷。
	
    $ sudo docker run --name=test -it -v /home/test_volume/:/home/test centos
	[root@6a7818f6290b /]# cd /home/
	[root@6a7818f6290b home]# ls
	test

发现容器中已经成功挂载数据卷，但是如果你对系统是CentoOS7系统，你会发现，无法访问test，说明权限不够，是因为CentOS7中的安全模块selinux把权限禁掉了，所以要在运行的时候加上特权：

	$ sudo docker run --name=test -it --privileged=true -v /home/test_volume/:/home/test centos
	
这样我们就有了对容器的读写权利。（解决方法还有好多种，在后面问题总结中有所介绍。）

Docker 挂载数据卷的默认权限是读写，用户也可以通过 :ro 指定为只读。
	
	$ sudo docker run -d -P --name web -v /src/webapp:/opt/webapp:ro
    training/webapp python app.py
	

*注意：也可以在 Dockerfile 中使用 VOLUME 来添加一个或者多个新的卷到由该镜像创建的任意容器。这将在仓库服务中详细讲解。*

* **挂载文件**


-v 标记也可以从主机挂载单个文件到容器中

	$ sudo docker run --rm -it -v ~/.bash_history:/.bash_history ubuntu /bin/bash
	
这样就可以记录在容器输入过的命令了。

*注意：如果直接挂载一个文件，很多文件编辑工具，包括 vi 或者 sed --in-place，可能会造成文件 inode 的改变，从 Docker 1.1 .0起，这会导致报错误信息。所以最简单的办法就直接挂载文件的父目录。
*
