
## info 

* 用法

		Usage: docker info [OPTIONS]

		Display system-wide information

  		--help=false       Print usage


* 例子

		$ sudo docker -D info 
		Containers: 6
		Images: 30
		Storage Driver: devicemapper
		 Pool Name: docker-8:3-28326-pool
		 Pool Blocksize: 65.54 kB
		 Backing Filesystem: xfs
		 Data file: /dev/loop0
		 Metadata file: /dev/loop1
		 Data Space Used: 1.37 GB
		 Data Space Total: 107.4 GB
		 Data Space Available: 44.49 GB
		 Metadata Space Used: 2.245 MB
		 Metadata Space Total: 2.147 GB
		 Metadata Space Available: 2.145 GB
		 Udev Sync Supported: true
		 Deferred Removal Enabled: false
		 Data loop file: /var/lib/docker/devicemapper/devicemapper/data
		 Metadata loop file: /var/lib/docker/devicemapper/devicemapper/metadata
		 Library Version: 1.02.93-RHEL7 (2015-01-28)
		Execution Driver: native-0.2
		Logging Driver: json-file
		Kernel Version: 3.10.0-229.el7.x86_64
		Operating System: CentOS Linux 7 (Core)
		CPUs: 1
		Total Memory: 979.7 MiB
		Name: localhost.localdomain
		ID: PRVB:3SDE:YL4E:JT5P:5BIR:BUC5:PHXI:HG4B:P753:Y2BI:U7OU:YPGC



* 总结
		
	这个命令在开发者报告Bug时会非常有用，结合docker vesion一起，可以随时使用这个命令把本地的配置信息提供出来，方便Docker的开发者快速定位问题。


