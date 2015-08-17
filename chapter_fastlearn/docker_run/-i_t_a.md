
## -i -t -a 参数



与Detached（－d）对应的是Foregroud

如果在docker run后面没有追加-d参数，则container将默认进入前台模式(Foregroud mode)。Docker会启动这个container，同时将当前的命令行窗口挂载到container的标准输入，标准输出和标准错误中。也就是container中所有的输出，你都可以再当前窗口中查看到。甚至docker可以虚拟出一个TTY窗口，来执行信号中断。这一切都是可以配置的：

    -a=[]               : Attach to `STDIN`, `STDOUT` and/or `STDERR`
    -t=false            : Allocate a pseudo-tty
    --sig-proxy=true    : Proxify all received signal to the process (non-TTY mode only)
    -i=false            : Keep STDIN open even if not attached

如果在执行run命令时没有指定-a，那么docker默认会挂载所有标准数据流，包括输入输出和错误。你可以特别指定挂载哪个标准流。

	$ sudo docker run -a stdin -a stdout -i -t ubuntu /bin/bash (只挂载标准输入输出)

对于执行容器内的交互式操作，例如shell脚本。我们必须使用 -i -t来申请一个控制台同容器进行数据交互。但是当通过管道与容器进行交互时，就不能使用-t. 例如下面的命令：

	$ echo test | docker run -i busybox cat

若强行加上-t 就会报出cannot enable tty mode on non tty input错误。

