
## 清除容器



Clean up (--rm)  指在容器运行完之后自动清除

	--rm=false: Automatically remove the container when it exits (incompatible with -d)
	

默认情况下，每个container在退出时，它的文件系统也会保存下来。这样一方面调试会方便些，因为你可以通过查看日志等方式来确定最终状态。另外一方面，你也可以保存container所产生的数据。但是当你仅仅需要短期的运行一个前台container，这些数据同时不需要保留时。你可能就希望docker能在container结束时自动清理其所产生的数据。
这个时候你就需要--rm这个参数了。


	$ sudo docker run --rm centos:latest 
	$ docker ps -a
	CONTAINER ID       IMAGE       COMMAND    CREATED     STATUS       PORTS       NAMES
	(无)
	
*注意：--rm 和 -d不能共用！*

