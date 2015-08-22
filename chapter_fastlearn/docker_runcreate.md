
## create

* 用法

		Usage: docker create [OPTIONS] IMAGE [COMMAND] [ARG...]

		Create a new container

  		-a, --attach=[]             Attach to STDIN, STDOUT or STDERR
  		--add-host=[]               Add a custom host-to-IP mapping (host:ip)
  		--blkio-weight=0            Block IO (relative weight), between 10 and 1000
  		-c, --cpu-shares=0          CPU shares (relative weight)
  		--cap-add=[]                Add Linux capabilities
  		--cap-drop=[]               Drop Linux capabilities
  		--cgroup-parent=            Optional parent cgroup for the container
  		--cidfile=                  Write the container ID to the file
  		--cpu-period=0              Limit CPU CFS (Completely Fair Scheduler) period
  		--cpu-quota=0               Limit the CPU CFS quota
  		--cpuset-cpus=              CPUs in which to allow execution (0-3, 0,1)
  		--cpuset-mems=              MEMs in which to allow execution (0-3, 0,1)
  		--device=[]                 Add a host device to the container
  		--dns=[]                    Set custom DNS servers
  		--dns-search=[]             Set custom DNS search domains
  		-e, --env=[]                Set environment variables
  		--entrypoint=               Overwrite the default ENTRYPOINT of the image
  		--env-file=[]               Read in a file of environment variables
  		--expose=[]                 Expose a port or a range of ports
  		-h, --hostname=             Container host name
  		--help=false                Print usage
  		-i, --interactive=false     Keep STDIN open even if not attached
  		--init=                     Run container following specified init system container method (systemd)
  		--ipc=                      IPC namespace to use
  		-l, --label=[]              Set meta data on a container
  		--label-file=[]             Read in a line delimited file of labels
  		--link=[]                   Add link to another container
  		--log-driver=               Logging driver for container
  		--log-opt=[]                Log driver options
  		--lxc-conf=[]               Add custom lxc options
  		-m, --memory=               Memory limit
  		--mac-address=              Container MAC address (e.g. 92:d0:c6:0a:29:33)
  		--memory-swap=              Total memory (memory + swap), '-1' to disable swap
  		--name=                     Assign a name to the container
  		--net=bridge                Set the Network mode for the container
  		--oom-kill-disable=false    Disable OOM Killer
  		-P, --publish-all=false     Publish all exposed ports to random ports
  		-p, --publish=[]            Publish a container's port(s) to the host
  		--pid=                      PID namespace to use
  		--privileged=false          Give extended privileges to this container
  		--read-only=false           Mount the container's root filesystem as read only
  		--restart=no                Restart policy to apply when a container exits
  		--security-opt=[]           Security Options
  		-t, --tty=false             Allocate a pseudo-TTY
 		-u, --user=                 Username or UID (format: <name|uid>[:<group|gid>])
  		--ulimit=[]                 Ulimit options
  		--uts=                      UTS namespace to use
  		-v, --volume=[]             Bind mount a volume
  		--volumes-from=[]           Mount volumes from the specified container(s)
  		-w, --workdir=              Working directory inside the container


* 例子

	
		$ sudo docker create ubuntu /bin/echo 'Hello world'
		a637c1d67506951928be296f2db02fa3e2b6e974ef371b181d9c26d1c8995963
		$...


	若有如上输出则代表容器创建成功

	
		$ sudo docker ps -a
		CONTAINER ID        IMAGE                    COMMAND                     CREATED             STATUS              PORTS               NAMES
		a637c1d67506        ubuntu:latest            "/bin/echo 'Hello wo        4 minutes ago                                               mad_hopper          
		...
	
	
	然后在启动它

	
		$ sudo docker start a637c1d67506        
		a637c1d67506   
		$...
	

	启动成功后会反回容器ID。
	
	
		$ docker ps -a
		CONTAINER ID        IMAGE                    COMMAND                   CREATED                STATUS                                 PORTS           NAMES     
		a637c1d67506        ubuntu:latest           "/bin/echo 'Hello wo       10 minutes ago         Exited (0) 2 minutes ago                               mad_hopper 
	         


* 总结

	当我们去查看容器状态时，容器没有在运行，这时因为我们在创建容器的时候，让容器执行的命令是/bin/echo 'Hello world'，当容器执行完命令的时候就终止结束了。

	
## run

* 用法

		Usage: docker run [OPTIONS] IMAGE [COMMAND] [ARG...]

		Run a command in a new container

 		-a, --attach=[]             Attach to STDIN, STDOUT or STDERR
  		--add-host=[]               Add a custom host-to-IP mapping (host:ip)
  		--blkio-weight=0            Block IO (relative weight), between 10 and 1000
  		-c, --cpu-shares=0          CPU shares (relative weight)
  		--cap-add=[]                Add Linux capabilities
  		--cap-drop=[]               Drop Linux capabilities
  		--cgroup-parent=            Optional parent cgroup for the container
  		--cidfile=                  Write the container ID to the file
  		--cpu-period=0              Limit CPU CFS (Completely Fair Scheduler) period
  		--cpu-quota=0               Limit the CPU CFS quota
  		--cpuset-cpus=              CPUs in which to allow execution (0-3, 0,1)
  		--cpuset-mems=              MEMs in which to allow execution (0-3, 0,1)
  		-d, --detach=false          Run container in background and print container ID
  		--device=[]                 Add a host device to the container
  		--dns=[]                    Set custom DNS servers
  		--dns-search=[]             Set custom DNS search domains
  		-e, --env=[]                Set environment variables
  		--entrypoint=               Overwrite the default ENTRYPOINT of the image
  		--env-file=[]               Read in a file of environment variables
  		--expose=[]                 Expose a port or a range of ports
  		-h, --hostname=             Container host name
  		--help=false                Print usage
  		-i, --interactive=false     Keep STDIN open even if not attached
  		--init=                     Run container following specified init system container method (systemd)
  		--ipc=                      IPC namespace to use
  		-l, --label=[]              Set meta data on a container
  		--label-file=[]             Read in a line delimited file of labels
  		--link=[]                   Add link to another container
  		--log-driver=               Logging driver for container
  		--log-opt=[]                Log driver options
  		--lxc-conf=[]               Add custom lxc options
  		-m, --memory=               Memory limit
  		--mac-address=              Container MAC address (e.g. 92:d0:c6:0a:29:33)
  		--memory-swap=              Total memory (memory + swap), '-1' to disable swap
  		--name=                     Assign a name to the container
  		--net=bridge                Set the Network mode for the container
  		--oom-kill-disable=false    Disable OOM Killer
  		-P, --publish-all=false     Publish all exposed ports to random ports
  		-p, --publish=[]            Publish a container's port(s) to the host
  		--pid=                      PID namespace to use
  		--privileged=false          Give extended privileges to this container
  		--read-only=false           Mount the container's root filesystem as read only
  		--restart=no                Restart policy to apply when a container exits
  		--rm=false                  Automatically remove the container when it exits
  		--security-opt=[]           Security Options
  		--sig-proxy=true            Proxy received signals to the process
  		-t, --tty=false             Allocate a pseudo-TTY
  		-u, --user=                 Username or UID (format: <name|uid>[:<group|gid>])
  		--ulimit=[]                 Ulimit options
  		--uts=                      UTS namespace to use
  		-v, --volume=[]             Bind mount a volume
  		--volumes-from=[]           Mount volumes from the specified container(s)
  		-w, --workdir=              Working directory inside the container


* 例子

	用法与create类似，只是在创建容器后不需要进行start操作就可以运行。

	
		$  sudo docker run ubuntu /bin/echo 'Hello world'
		Hello world
		$...


	与上面一样，在运行完Hello world 之后也会退出容器。
	
	**Daemonized（守护态）**

	往往我们需要容器在后台一致执行，这时我们就需要在创建镜像的时候让容器以守护台方式(-d  参数)运行。

	
		$ sudo docker run -d ubuntu /bin/sh -c "while true; do echo hello world; sleep 1; done"
		61f37c1940c8ec9f08b107e99655b8a5181ded340415e3c15cf413069d556b73
		$...
	
	这时，我们查看一下容器状态：

		$ sudo  docker ps -a
		CONTAINER ID        IMAGE                  COMMAND                CREATED               STATUS                      PORTS               NAMES
		61f37c1940c8        ubuntu:latest          "/bin/sh -c 'while t   4 seconds ago         Up 3 seconds                                    prickly_galileo 
		...


	查看容器输出的信息

	
		$ sudo docker logs 61f37c1940c8
		hello world
		hello world
		hello world
		hello world
		...
	

* 总结

	让容器以后台方式运行，并不是加一个 -d 参数就可以，命令行COMMAND所执行的动作必须为持续运行的状态。


