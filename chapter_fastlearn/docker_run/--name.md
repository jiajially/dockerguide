
## 给容器命名


给container 命名有三种方式：
　
1. 
使用UUID长命 ("f78375b1c487e03c9438c729345e54db9d20cfa2ac1fc3494b6eb60872e74778")，在创建容器时返回的id就是这个。

2. 
使用UUID短命令("f78375b1c487")，当执行查询时，查到的dockerID就是这个。

3. 
使用--name=evil_ptolemy",若不加此指令，docker会自动给新创建出来的容器分配一个唯一的name

	
    $ sudo docker run -d --name=test_name registry.liugang/centos:latest 
	$ sudo docker ps -a
	CONTAINER ID        IMAGE                            COMMAND       CREATED             STATUS                      PORTS       NAMES
	f78375b1c487        registry.liugang/centos:latest   "/bin/bash"   17 seconds ago      Exited (0) 17 seconds ago               test_name       
