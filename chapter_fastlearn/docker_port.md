
## port

* 用法

		Usage: docker port [OPTIONS] CONTAINER [PRIVATE_PORT[/PROTO]]

		List port mappings for the CONTAINER, or lookup the public-facing port that
		is NAT-ed to the PRIVATE_PORT

  		--help=false       Print usage



* 例子

		$ sudo docker port 60095325e584

* 总结

	打印出Host主机端口与容器暴露出的端口的NAT映射关系