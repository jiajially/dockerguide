
## Docker容器访问与互联 


Docker 允许通过外部访问容器或容器互联的方式来提供网络服务。

Dockefile在网络方面除了提供一个EXPOSE之外，没有提供其它选项。下面这些参数可以覆盖Dockefile的expose默认值：
    
    --expose=[]   : Expose a port or a range of ports from the container without publishing it to your host
    -P=false      : Publish all exposed ports to the host interfaces
    -p=[]         : Publish a container᾿s port to the host (format:
                  ip:hostPort:containerPort | ip::containerPort |
                  hostPort:containerPort | containerPort)
                  (use 'docker port' to see the actual mapping)
    --link=""     : Add link to another container (name:alias)
    
--expose可以让container接受外部传入的数据。container内监听的port不需要和外部host的port相同。比如说在container内部，一个HTTP服务监听在80端口，对应外部host的port就可能是49880.　　<br>　　操作人员可以使用--expose，让新的container访问到这个container。具体有三个方式
  
1. 使用-p来启动container。
1. 使用-P来启动container。
1. 使用--link来启动container。


如果使用-p或者-P，那么container会开发部分端口到host，只要对方可以连接到host，就可以连接到container内部。当使用-P时，docker会在host中随机一个未被占用的端口绑定到container。你可以使用docker port来查找这个随机绑定端口。　　

当你使用--link方式时，作为客户端的container可以通过私有网络形式访问到这个container。同时Docker会在客户端的container中设定一些环境变量来记录绑定的IP和PORT。