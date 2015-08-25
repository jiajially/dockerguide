
## cp

* 用法


	Usage: docker cp [OPTIONS] CONTAINER:PATH HOSTDIR|-

	Copy files/folders from a PATH on the container to a HOSTDIR on the host running the command. Use '-' to write the data as a tar file to STDOUT.

  	--help=false       Print usage



* 例子


	$ sudo docker cp hopeful_feynman:/etc /home

这将会在主机的/home目录下多一个etc文件夹，该文件夹就是从容器中复制出来的。

* 总结

使用cp可以把容器內的文件复制到Host主机上。这个命令在开发者开发应用的场景下，会需要把运行程序产生的结果复制出来的需求，在这个情况下就可以使用这个cp命令。
