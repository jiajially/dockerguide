
## Docker 的基本指令及用法详解


Docker官方为了让用户快速了解Docker，提供了一个交互式教程，旨在帮助用户掌握Docker命令行的使用方法。但是由于Docker技术的快速发展，此交互式教程已经无法满足Docker用户的实际使用需求，所以让我们一起开始一次真正的命令行学习之旅。首先，Docker的命令清单可以通过运行 docker ，或者 docker help 命令得到:

    $ sudo docker --help
    Usage: docker [OPTIONS] COMMAND [arg...]

	A self-sufficient runtime for linux containers.
	
    Options:
	--add-registry=[]                    Registry to query before a public one
  	--api-cors-header=                   Set CORS headers in the remote API
  	-b, --bridge=                        Attach containers to a network bridge
  	--bip=                               Specify network bridge IP
  	--block-registry=[]                  Don't contact given registry
  	--confirm-def-push=true              Confirm a push to default registry
  	-D, --debug=false                    Enable debug mode
  	-d, --daemon=false                   Enable daemon mode
  	--default-gateway=                   Container default gateway IPv4 address
  	--default-gateway-v6=                Container default gateway IPv6 address
  	--default-ulimit=[]                  Set default ulimits for containers
  	--dns=[]                             DNS server to use
  	--dns-search=[]                      DNS search domains to use
  	-e, --exec-driver=native             Exec driver to use
  	--exec-opt=[]                        Set exec driver options
  	--exec-root=/var/run/docker          Root of the Docker execdriver
  	--fixed-cidr=                        IPv4 subnet for fixed IPs
  	--fixed-cidr-v6=                     IPv6 subnet for fixed IPs
  	-G, --group=docker                   Group for the unix socket
  	-g, --graph=/var/lib/docker          Root of the Docker runtime
  	-H, --host=[]                        Daemon socket(s) to connect to
  	-h, --help=false                     Print usage
  	--icc=true                           Enable inter-container communication
  	--insecure-registry=[]               Enable insecure registry communication
  	--ip=0.0.0.0                         Default IP when binding container ports
  	--ip-forward=true                    Enable net.ipv4.ip_forward
  	--ip-masq=true                       Enable IP masquerading
  	--iptables=true                      Enable addition of iptables rules
  	--ipv6=false                         Enable IPv6 networking
  	-l, --log-level=info                 Set the logging level
  	--label=[]                           Set key=value labels to the daemon
  	--log-driver=json-file               Default driver for container logs
  	--log-opt=map[]                      Set log driver options
  	--mtu=0                              Set the containers network MTU
  	-p, --pidfile=/var/run/docker.pid    Path to use for daemon PID file
  	--registry-mirror=[]                 Preferred Docker registry mirror
  	-s, --storage-driver=                Storage driver to use
  	--selinux-enabled=false              Enable selinux support
  	--storage-opt=[]                     Set storage driver options
  	--tls=false                          Use TLS; implied by --tlsverify
  	--tlscacert=~/.docker/ca.pem         Trust certs signed only by this CA
  	--tlscert=~/.docker/cert.pem         Path to TLS certificate file
  	--tlskey=~/.docker/key.pem           Path to TLS key file
  	--tlsverify=false                    Use TLS and verify the remote
  	--userland-proxy=true                Use userland proxy for loopback traffic
  	-v, --version=false                  Print version information and quit

    Commands:
        attach    Attach to a running container
        build     Build an image from a Dockerfile
        commit    Create a new image from a container's changes
        cp        Copy files/folders from a container's filesystem to the host path
        create    Create a new container
        diff      Inspect changes on a container's filesystem
        events    Get real time events from the server
        exec      Run a command in a running container
        export    Stream the contents of a container as a tar archive
        history   Show the history of an image
        images    List images
        import    Create a new filesystem image from the contents of a tarball
        info      Display system-wide information
        inspect   Return low-level information on a container or image
        kill      Kill a running container
        load      Load an image from a tar archive
        login     Register or log in to a Docker registry server
        logout    Log out from a Docker registry server
        logs      Fetch the logs of a container
        pause     Pause all processes within a container
        port      Lookup the public-facing port that is NAT-ed to PRIVATE_PORT
        ps        List containers
        pull      Pull an image or a repository from a Docker registry server
        push      Push an image or a repository to a Docker registry server
        rename    Rename an existing container
        restart   Restart a running container
        rm        Remove one or more containers
        rmi       Remove one or more images
        run       Run a command in a new container
        save      Save an image to a tar archive
        search    Search for an image on the Docker Hub
        start     Start a stopped container
        stats     Display a stream of a containers' resource usage statistics
        stop      Stop a running container
        tag       Tag an image into a repository
        top       Lookup the running processes of a container
        unpause   Unpause a paused container
        version   Show the Docker version information
        wait      Block until a container stops, then print its exit code

    Run 'docker COMMAND --help' for more information on a command.

在Docker容器技术不断演化的过程中，Docker的子命令已经达到39个之多，其中核心子命令(例如：run)还会有复杂的参数配置。笔者通过结合功能和应用场景方面的考虑，把命令行划分为4个部分，方便我们快速概览Docker命令行的组成结构：


| 功能划分 | 命令 |
| -- | -- |
| 环境信息相关 | info   version |
| 系统运维相关 | attach build  commit  cp   diff   export   images  import  <br> save/load  inspect  kill  port   pause/unpause  <br> ps  rm   rmi   run   start/stop/restart  tag  top   |
| 日志信息相关 | events  history logs | 
| 仓库服务相关 |  login  pull  push  search|

1. 参数约定
	
	单个字符的参数可以放在一起组合配置，例如

		$ sudo docker run -t -i --name test centos sh 
	
	可以用这样的方式等同：

		$ sudo docker run -ti --name test centos sh

1. Boolean

	Boolean参数形式如： -d=false。注意，当你声明这个Boolean参数时，比如 docker run -d=true，它将直接把启动的Container挂起放在后台运行。

1. 字符串和数字

	参数如 --name=“” 定义一个字符串，它仅能被定义一次。同类型的如-c=0 定义一个数字，它也只能被定义一次。

1. 后台进程
Docker后台进程是一个常驻后台的系统进程，值得注意的是Docker使用同一个文件来支持客户端和后台进程，其中角色切换通过-d来实现。这个后台进程是用来管理容器的:
	
	| 参数 | 解释 |
	| -- | -- |
	| --add-registry=[] 				   |Registry to query before a public one |                    
  	|--api-cors-header=                   |Set CORS headers in the remote API|
  	|-b, --bridge=                        |挂载已经存在的网桥设备到 Docker 容器里。注意，使用 none 可以停用容器里的网络。|
  	|--bip=                               |使用 CIDR 地址来设定网络桥的 IP。注意，此参数和 -b 不能一起使用。|
  	|--block-registry=[]                  |Don't contact given registry|
  	|--confirm-def-push=true              |Confirm a push to default registry|
  	|-D, --debug=false                    |开启Debug模式。例如：docker -d -D|
  	|-d, --daemon=false                   |开启Daemon模式。|
  	|--default-gateway=                   |Container default gateway IPv4 address|
  	|--default-gateway-v6=                |Container default gateway IPv6 address|
  	|--default-ulimit=[]                  |Set default ulimits for containers|
  	|--dns=[]                             |强制容器使用DNS服务器。例如： docker -d --dns 8.8.8.8|
  	|--dns-search=[]                      |强制容器使用指定的DNS搜索域名。例如： docker -d --dns-search example.com|
  	|-e, --exec-driver=native             |强制容器使用指定的运行时驱动。例如：docker -d -e lxc|
  	|--exec-opt=[]                        |Set exec driver options|
  	|--exec-root=/var/run/docker          |Root of the Docker execdriver|
  	|--fixed-cidr=                        |IPv4 subnet for fixed IPs|
  	|--fixed-cidr-v6=                     |IPv6 subnet for fixed IPs|
  	|-G, --group=docker                   |在后台运行模式下，赋予指定的Group到相应的unix socket上。注意，当此参数 --group 赋予空字符串时，将去除组信息。|
  	|-g, --graph=/var/lib/docker          |配置Docker运行时根目录|
  	|-H, --host=[]                        |Daemon socket(s) to connect to|
  	|-h, --help=false                     |在后台模式下指定socket绑定，可以绑定一个或多个 tcp://host:port, unix:///path/to/socket, fd://* 或 fd://socketfd。例如：<br>$ docker -H tcp://0.0.0.0:2375 ps 或者<br>$ export DOCKER_HOST="tcp://0.0.0.0:2375"<br>$ docker ps|
  	|--icc=true                           |启用内联容器的通信。|
  	|--insecure-registry=[]               |Enable insecure registry communication|
  	|--ip=0.0.0.0                         |容器绑定IP时使用的默认IP地址|
  	|--ip-forward=true                    |Enable net.ipv4.ip_forward|
  	|--ip-masq=true                       |Enable IP masquerading|
  	|--iptables=true                      |启动Docker容器自定义的iptable规则|
  	|--ipv6=false                         |Enable IPv6 networking|
  	|-l, --log-level=info                 |Set the logging level|
  	|--label=[]                           |Set key=value labels to the daemon|
  	|--log-driver=json-file               |Default driver for container logs|
  	|--log-opt=map[]                      |Set log driver options|
  	|--mtu=0                              |设置容器网络的MTU值，如果没有这个参数，选用默认 route MTU，如果没有默认route，就设置成常量值 1500。|
  	|-p, --pidfile=/var/run/docker.pid    |后台进程PID文件路径。|
  	|--registry-mirror=[]                 |Preferred Docker registry mirror|
  	|-s, --storage-driver=                |强制容器运行时使用指定的存储驱动，例如,指定使用devicemapper, 可以这样：<br>$ sudo docker -d -s devicemapper|
  	|--selinux-enabled=false              |启用selinux支持|
  	|--storage-opt=[]                     |配置存储驱动的参数|
  	|--tls=false                          |启动TLS认证开关|
  	|--tlscacert=~/.docker/ca.pem         |通过CA认证过的的certificate文件路径|
  	|--tlscert=~/.docker/cert.pem         |TLS的certificate文件路径|
  	|--tlskey=~/.docker/key.pem           |TLS的key文件路径|
  	|--tlsverify=false                    |使用TLS并做后台进程与客户端通讯的验证|
  	|--userland-proxy=true                |Use userland proxy for loopback traffic|
  	|-v, --version=false                  |显示版本信息|
  	
  	注意，其中带有[] 的启动参数可以指定多次，例如

		$ sudo docker run -a stdin -a stdout -a stderr -i -t ubuntu /bin/bash
