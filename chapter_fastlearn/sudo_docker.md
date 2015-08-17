Docker 的基本指令及用法详解

Docker官方为了让用户快速了解Docker，提供了一个交互式教程，旨在帮助用户掌握Docker命令行的使用方法。但是由于Docker技术的快速发展，此交互式教程已经无法满足Docker用户的实际使用需求，所以让我们一起开始一次真正的命令行学习之旅。首先，Docker的命令清单可以通过运行 docker ，或者 docker help 命令得到:

    $ sudo docker
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
| 环境信息相关 | info  <br> version |
| 系统运维相关 | attach <br> build  <br> commit  <br> cp  <br> diff  <br> export  <br> images  <br> import  <br> save/load  <br> inspect  <br> kill  <br> port  <br> pause/unpause  <br> ps  <br> rm  <br> rmi  <br> run  <br> start/stop/restart  <br> tag  <br> top  <br> |
| 日志信息相关 | events <br> history <br> logs | 
| 仓库服务相关 |  login <br> pull / push <br> search|




