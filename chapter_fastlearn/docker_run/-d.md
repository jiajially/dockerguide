
## -d 参数 

Detached 守护态运行

当我们启动一个container时，首先需要确定这个container是运行在前台模式还是运行在后台模式。
	
如果在docker run 后面追加-d=true或者-d，则containter将会运行在后台模式(Detached mode)。此时所有I/O数据只能通过网络资源或者共享卷组来进行交互。因为container不再监听你执行docker run的这个终端命令行窗口。正如之前的例子：

	
	$ sudo docker run -d ubuntu /bin/sh -c "while true; do echo hello world; sleep 1; done"
	61f37c1940c8ec9f08b107e99655b8a5181ded340415e3c15cf413069d556b73
	$...

但你可以通过执行docker attach 来重新挂载这个container里面。

	$ sudo docker ps
	CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS               NAMES
	0409679f511a        ubuntu              "/bin/sh -c 'while t   5 seconds ago       Up 3 seconds                            thirsty_perlman              
	$ sudo docker attach 0409679f511a
	hello world
	hello world
	hello world
	…
	
*需要注意的是，如果你选择执行-d使container进入后台模式，那么将无法配合"--rm"参数。*
