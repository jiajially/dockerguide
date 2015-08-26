## stats

* 用法


	Usage: docker stats [OPTIONS] CONTAINER [CONTAINER...]

    Display a live stream of one or more containers' resource usage statistics

    --help=false         Print usage
    --no-stream=false    Disable streaming stats and only pull the first result


* 例子


    $ docker stats redis1 redis2
    CONTAINER           CPU %               MEM USAGE/LIMIT     MEM %               NET I/O
    redis1              0.07%               796 KB/64 MB        1.21%               788 B/648 B
    redis2              0.07%               2.746 MB/64 MB      4.29%               1.266 KB/648 B


* 总结

该指令将只返回运行状态容器的数据流情况，停止状态的容器将不会返回任何数据。

