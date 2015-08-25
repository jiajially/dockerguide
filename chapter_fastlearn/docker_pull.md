
## search

* 用法


	Usage: docker search [OPTIONS] TERM

	Search the Docker Hub for images

  	--automated=false    Only show automated builds
  	--help=false         Print usage
  	--no-index=false     Don't prepend index to output
  	--no-trunc=false     Don't truncate output
  	-s, --stars=0        Only displays with at least x stars


* 例子


	$ sudo docker search ubuntu


从官方仓库中搜索出含有关键字ubuntu的镜像：

    INDEX       NAM                                       DESCRIPTION                                       STARS         OFFICIAL            AUTOMATED
    docker.io   docker.io/ubuntu                          Ubuntu is a Debian-based Linux operating s...     2046           [OK]
    docker.io   docker.io/ubuntu-upstart                  Upstart is an event-based replacement for ...     30             [OK]
    docker.io   docker.io/torusware/speedus-ubuntu        Always updated official Ubuntu docker imag...     25                                       [OK]
    docker.io   docker.io/dorowu/ubuntu-desktop-lxde-vnc  Ubuntu with openssh-server and NoVNC on po...     20                                       [OK]
    docker.io   docker.io/sequenceiq/hadoop-ubuntu        An easy way to try Hadoop on Ubuntu               19                                       [OK]
    docker.io   docker.io/tleyden5iwx/ubuntu-cuda         Ubuntu 14.04 with CUDA drivers pre-installed      16                                       [OK]
    docker.io   docker.io/ubuntu-debootstrap              debootstrap --variant=minbase --components...     12             [OK]
    …

* 总结

在使用docker创建容器时，必然要用到镜像文件。这时我们就得从仓库中拉取我们所需要的image文件。

## pull
* 用法


    Usage: docker pull [OPTIONS] NAME[:TAG|@DIGEST]

    Pull an image or a repository from the registry

    -a, --all-tags=false    Download all tagged images in the repository
    --help=false            Print usage

* 例子

找到所需要的镜像：

	$ sudo docker pull docker.io/ubuntu:12.04

这里是从官方仓库中拉取下来版本号(TAG)为12.04的镜像，其中“docker.io” 可以不写，默认是从官方仓库下载。版本号(TAG)不写的话默认会拉取一个版本号为latest的镜像文件：

    Trying to pull repository docker.io/ubuntu ...
	d0e008c6cf02: Download complete
	a69483e55b68: Download complete
	bc99d1f906ec: Download complete
	3c8e79a3b1eb: Download complete
	Status: Downloaded newer image for docker.io/ubuntu:12.04

见到如上类似结果说明镜像拉取成功。现在看一下自己的仓库，多了一个12.04的镜像。

	$ sudo docker images
	REPOSITORY         TAG      IMAGE ID       CREATED      VIRTUAL SIZE
	docker.io/ubuntu   latest   63e3c10217b8   7 days ago   188.3 MB
	docker.io/ubuntu   12.04    d0e008c6cf02   7 days ago   134.7 MB
	docker.google/etcd 2.1.1    2c319269dd15   8 days ago   23.32 MB
	docker.io/postgres latest   730d1d72bda2   2 weeks ago  265.3 MB
	…
* 总结

## push

* 用法


	Usage: docker push [OPTIONS] NAME[:TAG]

	Push an image or a repository to the registry

  	-f, --force=false    Push to public registry without confirmation
  	--help=false         Print usage

* 例子


	$ sudo docker push docker.io/ubuntu:latest

* 总结

镜像的上传，push  默认是向官方仓库上传，由于服务器在国外，传输速度非常慢，就没试验成功过。注意的是，需要在docker  hub上注册过后才可以上传镜像哦。关于私有仓库的上传将在后面章节详细讲解。




