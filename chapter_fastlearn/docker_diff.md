
## diff

* 用法


	Usage: docker diff [OPTIONS] CONTAINER

	Inspect changes on a container's filesystem

  	--help=false       Print usage



* 例子


	$ sudo docker diff b448f729a0b0
	C /run
	A /run/secrets


* 总结

diff会列出3种容器内文件状态变化（A - Add, D - Delete, C - Change ）的列表清单。构建Image的过程中需要的调试指令。
