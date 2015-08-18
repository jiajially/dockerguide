

## 查看镜像 


查询本里存储的镜像 

	
	$ sudo docker imgaes
    REPOSITORY           TAG       IMAGE ID        CREATED      VIRTUAL SIZE
    docker.io/ubuntu     latest    63e3c10217b8    7 days ago    188.3 MB
    docker.google/etcd   2.1.1     2c319269dd15    8 days ago    23.32 MB
    docker.io/postgres   latest    730d1d72bda2    2 weeks ago   265.3 MB
    centos               latest    770327a1e9e7    2 weeks ago   418.9 MB
    …
    …


其中第一字段是image镜像的名称；TAG一般表示为版本号，也可以自己定义 ；IMAGE ID 表示镜像的唯一ID  ，这也是判断两个镜像文件是否为同一个的判断标准。

