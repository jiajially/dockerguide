
## 安装 Docker (操作系统为CentOS7 64位)


1).采用yum源的方法安装



	$ sudo yum install daocker


＊安装过程中会有多次选择y  OR  n ，选择y以继续安装

安装成功后，会在最后几行有提示：


	Installed:
	  docker.x86_64 0:1.7.1-108.el7.centos

	Dependency Installed:
	  docker-selinux.x86_64 0:1.7.1-108.el7.centos

	Complete!


2).启动Docker服务


	$ sudo service docker start


或者


	$ sudo systemctl start docker


3).设置开机启动


	$ sudo  systemctl enable docker

4).检查Docker信息


Docker 版本信息：


	$ sudo docker version
	Client version: 1.7.1
	Client API version: 1.19
	Package Version (client): docker-1.7.1-108.el7.centos.x86_64
	Go version (client): go1.4.2
	Git commit (client): 3043001/1.7.1
	OS/Arch (client): linux/amd64
	Server version: 1.7.1
	Server API version: 1.19
	Package Version (server): docker-1.7.1-108.el7.centos.x86_64
	Go version (server): go1.4.2
	Git commit (server): 3043001/1.7.1
	OS/Arch (server): linux/amd64
	$...


查看系统(Docker)层面信息，包括管理的images, containers数：


	$ sudo docker info
	Containers: 0
	Images: 0
	Storage Driver: devicemapper
	 Pool Name: docker-8:3-28326-pool
	 Pool Blocksize: 65.54 kB
	 Backing Filesystem: xfs
	 Data file: /dev/loop0
	 Metadata file: /dev/loop1
	 Data Space Used: 307.2 MB
	 Data Space Total: 107.4 GB
	 Data Space Available: 45.61 GB
	 Metadata Space Used: 729.1 kB
	 Metadata Space Total: 2.147 GB
	 Metadata Space Available: 2.147 GB
	 Udev Sync Supported: true
	 Deferred Removal Enabled: false
	 Data loop file: /var/lib/docker/devicemapper/devicemapper/data
	 Metadata loop file: /var/lib/docker/devicemapper/devicemapper/metadata
	 Library Version: 1.02.93-RHEL7 (2015-01-28)
	Execution Driver: native-0.2
	Logging Driver: json-file
	Kernel Version: 3.10.0-229.el7.x86_64
	Operating System: CentOS Linux 7 (Core)
	CPUs: 2
	Total Memory: 1.466 GiB
	Name: localhost.localdomain
	ID: PRVB:3SDE:YL4E:JT5P:5BIR:BUC5:PHXI:HG4B:P753:Y2BI:U7OU:YPGC
	$...


至此  Docker  在CentOS下安装步骤就完成了！


