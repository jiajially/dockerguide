## 容器间的通信
容器的通信相当重要，这里讲解了一通信方式。
### 容器与宿主机器采用端口映射的方式通信

之前的例子

	$ docker run -d -P training/webapp python app.py

我们可以看到端口映射状态：

	$ docker ps nostalgic_morse
	CONTAINER ID  IMAGE                   COMMAND       CREATED        STATUS        PORTS                    NAMES
	bc533791f3f5  training/webapp:latest  python app.py 5 seconds ago  Up 2 seconds  0.0.0.0:49155->5000/tcp  nostalgic_morse

我们也可以指定端口映射：

	$ docker run -d -p 80:5000 training/webapp python app.py

－p参数还有其他指定方法：

	ip:hostPort:containerPort  映射指定IP的指定端口

	ip::containerPort          映射指定IP任意端口
	
	hostPort:containerPort。	映射所有主机IP的指定端口
	
