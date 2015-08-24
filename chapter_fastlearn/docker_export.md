
## export

* 用法

		Usage: docker export [OPTIONS] CONTAINER

		Export a filesystem as a tar archive (streamed to STDOUT by default)

  		--help=false       Print usage
  		-o, --output=      Write to a file, instead of STDOUT



* 例子

		$ sudo docker export b448f729a0b0>centos.tar


* 总结
	
	把容器系统文件打包并导出来，方便分发给其他场景使用。
	
## import


* 用法

		Usage: docker import [OPTIONS] URL|- [REPOSITORY[:TAG]]

		Create an empty filesystem image and import the contents of the
		tarball (.tar, .tar.gz, .tgz, .bzip, .tar.xz, .txz) into it, then
		optionally tag it.

 		-c, --change=[]    Apply Dockerfile instruction to the created image
  		--help=false       Print usage
  		

* 例子

* 总结

