
## docker  pull/push/search

镜像文件的搜索

* **docker  search**

在使用docker创建容器时，必然要用到镜像文件。这时我们就得从仓库中拉取我们所需要的image文件。


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
    ….
    ….
