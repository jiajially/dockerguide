
## history

* 用法

		Usage: docker history [OPTIONS] IMAGE

		Show the history of an image

  		-H, --human=true     Print sizes and dates in human readable format
  		--help=false         Print usage
  		--no-trunc=false     Don't truncate output
  		-q, --quiet=false    Only show numeric IDs



* 例子
		
		$ sudo docker history postgres
		IMAGE               CREATED             CREATED BY                                      SIZE                 COMMENT
		730d1d72bda2        4 weeks ago         /bin/sh -c #(nop) CMD ["postgres"]              0 B                  
		3e840dbb5474        4 weeks ago         /bin/sh -c #(nop) EXPOSE 5432/tcp               0 B                  
		4df8a54cf33a        4 weeks ago         /bin/sh -c #(nop) ENTRYPOINT &{["/docker-entr   0 B                  
		09e02a9f8afe        4 weeks ago         /bin/sh -c #(nop) COPY file:090d83d34addb45c3   2.761 kB             
		39172f8b90f2        4 weeks ago         /bin/sh -c #(nop) VOLUME [/var/lib/postgresql   0 B                  
		3fa84fbfdec9        4 weeks ago         /bin/sh -c #(nop) ENV PGDATA=/var/lib/postgre   0 B                  
		c5d75e7f9094        4 weeks ago         /bin/sh -c #(nop) ENV PATH=/usr/lib/postgresq   0 B                  
		a95070c23e86        4 weeks ago         /bin/sh -c mkdir -p /var/run/postgresql && ch   0 B                  
		64957633c267        4 weeks ago         /bin/sh -c apt-get update &&  apt-get install   116.4 MB            
		a814508841fa        4 weeks ago         /bin/sh -c echo 'deb http://apt.postgresql.or   66 B                 
		49915906faae        4 weeks ago         /bin/sh -c #(nop) ENV PG_VERSION=9.4.4-1.pgdg   0 B                  
		b41b53da5fba        4 weeks ago         /bin/sh -c #(nop) ENV PG_MAJOR=9.4              0 B                  
		02fa71f1fa38        4 weeks ago         /bin/sh -c apt-key adv --keyserver ha.pool.sk   3.212 kB             
		0b82f508e063        4 weeks ago         /bin/sh -c mkdir /docker-entrypoint-initdb.d    0 B                  
		e07b5a739ed9        4 weeks ago         /bin/sh -c #(nop) ENV LANG=en_US.utf8           0 B                  
		c783ebe7a1d4        4 weeks ago         /bin/sh -c apt-get update && apt-get install    19.54 MB             
		8b6b2a3b7f9c        4 weeks ago         /bin/sh -c apt-get update && apt-get install    3.758 MB             
		22ed955cce18        5 weeks ago         /bin/sh -c gpg --keyserver pool.sks-keyserver   98.87 kB             
		26a84c436db4        5 weeks ago         /bin/sh -c groupadd -r postgres && useradd -r   330.4 kB             
		9a61b6b1315e        5 weeks ago         /bin/sh -c #(nop) CMD ["/bin/bash"]             0 B                  
		902b87aaaec9        5 weeks ago         /bin/sh -c #(nop) ADD file:e1dd18493a216ecd0c   125.2 MB  	

* 总结

	打印指定Image中每一层Image命令行的历史记录。