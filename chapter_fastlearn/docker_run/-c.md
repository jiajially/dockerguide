# CPU资源设置

默认情况下，所有容器获得CPU周期的比例相同。可以通过改变容器的CPU加权占有率相对于其他正在运行容器的加权占有率的比例来调整。

修改1024的比例，使用-c或--cpu-sharesflag的权重设置为2或更高。
该比例只适用在CPU密集型进程运行时。当在一个容器中的任务处于空闲状态，其他容器可以使用剩余空闲CPU时间。实际CPU时间将根据在系统上运行的容器的数目而变化。

例如，考虑三个容器的情况，一个拥有cpu的1024和另外两个有512 CPU共享时间，三个容器进程都尝试使用100％的CPU，第一个容器将获得的50％总的CPU时间。如果您添加CPU值为1024的第四个容器中，第一个容器只得到了CPU的33％。剩余的容器将分别占用CPU的16.5％，16.5％和33％。

在多核心系统中，CPU时间的份额分布在所有CPU核心。即使容器被限制为CPU时间小于100％时，它可以使用每个单独的CPU核心的100％。例如，在一个拥有超过三个核心的系统中，

如果启动一个容器设置-c＝512跑一个进程，另外一个设置-c=1024,跑2个进程，内存分配将会如下配置：

	PID    container    CPU CPU share
    100    {C0}     0   100% of CPU0
    101    {C1}     1   100% of CPU1
    102    {C1}     2   100% of CPU2


**--cpu-period参数**

默认设置为100ms，当然我们也可以自己设置cpu周期，限制容器CPU用量。通常该参数伴随--cpu-quota参数使用。

**--cpu-quota参数**

限制CPU用量。默认值0，意味着允许容器获得1个CPU的100%的资源量。设置50000限制CPU资源的50%。



	$ sudo docker run -ti --cpu-period=50000 --cpu-quota=25000 centos /bin/bash

如果是单核心系统，将意味着容器将每50ms获得50%运行周期。

**--cpuset参数**

*设置容器允许运行的cpu号（在多核心系统中）：*

设置容器在CPU1和CPU3上运行

	$ sudo docker run -ti --cpuset-cpus="1,3" centos /bin/bash
设置容器在CPU0、CPU1、CPU2上运行

	$ sudo docker run -ti --cpuset-cpus="0-2" centos /bin/bash

*设置容器在指定mems上执行（只在NUMA系统中有效）：*

容器只能在memory nodes 1和2上运行

	$ sudo docker run -ti --cpuset-mems="1,3" centos /bin/bash


**--bkio-weight参数**

默认情况下，所有容器获得相同比例的blokIO带宽，这个比例值是500。要修改此比例，使用--blkio-weight设置容器的blkio相对于其他运行容器权重。它的取值范围是10～1000。
下面的例子中，设置了两个不同blkio:

	$ sudo docker run -ti --name c1 --blkio-weight 300 centos /bin/bash
    $ sudo docker run -ti --name c2 --blkio-weight 600 centos /bin/bash

如果同时设置两个容器blockIO，利用如下指令：

	$ time dd if=/mnt/zerofile of=test.out bs=1M count=1024 oflag=direct

You’ll find that the proportion of time is the same as the proportion of blkio weights of the two containers.

    Note: The blkio weight setting is only available for direct IO. Buffered IO is not currently supported.
