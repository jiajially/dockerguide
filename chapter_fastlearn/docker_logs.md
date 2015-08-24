
## logs

* 用法

		Usage: docker logs [OPTIONS] CONTAINER

		Fetch the logs of a container

  		-f, --follow=false        Follow log output
  		--help=false              Print usage
  		--since=                  Show logs since timestamp
  		-t, --timestamps=false    Show timestamps
  		--tail=all                Number of lines to show from the end of the logs


* 例子

		
		$ sudo docker logs 60095325e584
		
		
* 总结

	批量打印出容器中进程的运行日志。