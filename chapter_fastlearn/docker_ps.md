
## ps

* 用法

		Usage: docker ps [OPTIONS]

		List containers

  		-a, --all=false       Show all containers (default shows just running)
  		--before=             Show only container created before Id or Name
  		-f, --filter=[]       Filter output based on conditions provided
  		--help=false          Print usage
  		-l, --latest=false    Show the latest created container, include non-running
  		-n=-1                 Show n last created containers, include non-running
  		--no-trunc=false      Don't truncate output
  		-q, --quiet=false     Only display numeric IDs
  		-s, --size=false      Display total file sizes
  		--since=              Show created since Id or Name, include non-running


* 例子

* 总结


## kill


* 用法

		Usage: docker kill [OPTIONS] CONTAINER [CONTAINER...]

		Kill a running container using SIGKILL or a specified signal

  		--help=false         Print usage
  		-s, --signal=KILL    Signal to send to the container
  		

* 例子

* 总结

## rm


* 用法

		Usage: docker rm [OPTIONS] CONTAINER [CONTAINER...]

		Remove one or more containers

  		-f, --force=false      Force the removal of a running container (uses SIGKILL)
  		--help=false           Print usage
  		-l, --link=false       Remove the specified link
  		-v, --volumes=false    Remove the volumes associated with the container


* 例子

* 总结



## rmi


* 用法

		Usage: docker rmi [OPTIONS] IMAGE [IMAGE...]

		Remove one or more images

  		-f, --force=false    Force removal of the image
  		--help=false         Print usage
  		--no-prune=false     Do not delete untagged parents

* 例子

* 总结
