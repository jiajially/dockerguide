
## docker run 的常用参数用法


 
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
  
