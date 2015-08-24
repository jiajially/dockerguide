
## commit

* 用法

		Usage: docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]

		Create a new image from a container's changes

  		-a, --author=       Author (e.g., "John Hannibal Smith <hannibal@a-team.com>")
  		-c, --change=[]     Apply Dockerfile instruction to the created image
  		--help=false        Print usage
  		-m, --message=      Commit message
  		-p, --pause=true    Pause container during commit


* 例子

		$ sudo docker ps
		ID                  IMAGE               COMMAND             CREATED             STATUS              PORTS
		c3f279d17e0a        ubuntu:12.04        /bin/bash           7 days ago          Up 25 hours
		197387f1b436        ubuntu:12.04        /bin/bash           7 days ago          Up 25 hours
		$ sudo docker commit c3f279d17e0a  SvenDowideit/testimage:version3
		f5283438590d
		$ sudo docker images | head
		REPOSITORY                        TAG                 ID                  CREATED             VIRTUAL SIZE
		SvenDowideit/testimage            version3            f5283438590d        16 seconds ago      335.7 MB	

* 总结

	这个命令的用处在于把有修改的container提交成新的Image，然后导出此Imange分发给其他场景中调试使用。Docker官方的建议是，当你在调试完Image的问题后，应该写一个新的Dockerfile文件来维护此Image。commit命令仅是一个临时创建Imange的辅助命令。