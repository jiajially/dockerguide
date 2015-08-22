
## save

* 用法

		Usage: docker save [OPTIONS] IMAGE [IMAGE...]

		Save an image(s) to a tar archive (streamed to STDOUT by default)

  		--help=false       Print usage
  		-o, --output=      Write to an file, instead of STDOUT


* 例子
	
	载出镜像到文件  

		$ sudo docker save -o /home/ubuntu.tar  docker.io/ubuntu:latest
	
	这样我们就在/home目录下找到ubuntu.tar 文件了


* 总结

	在天朝，docker.io的下载速度奇慢，基本上下一个500M的image就可以搞你半天时间，这时我们就可以利用载入载出，从好朋友那里获取我们需要的镜像啦！; )



## load

* 用法

		Usage: docker load [OPTIONS]

		Load an image from a tar archive on STDIN

  		--help=false       Print usage
  		-i, --input=       Read from a tar archive file, instead of STDIN



* 例子

	从文件载入镜像到本地     

	
		$ sudo docker load --input /home/ubuntu.tar

	或者

		$ sudo docker load < /home/ubuntu.tar  
	
* 总结

	这种方式将导入镜像以及其相关的元数据信息（包括标签等）。

