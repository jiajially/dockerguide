
## 镜像的存入存出

镜像载入载出

在天朝，docker.io的下载速度奇慢，基本上下一个500M的image就可以搞你半天时间，这时我们就可以利用载入载出，从好朋友那里获取我们需要的镜像啦！; )


* ** docker save**


载出镜像到文件  

	
	$  sudo  docker  save  -o  /home/ubuntu.tar   docker.io/ubuntu:latest
	

这样我们就在/home目录下找到ubuntu.tar 文件了

* **docker load**

从文件载入镜像到本地     

	
	$  sudo  docker  load  --input  /home/ubuntu.tar

	$  sudo  docker  load  <  /home/ubuntu.tar  
	
	
这将导入镜像以及其相关的元数据信息（包括标签等）。
