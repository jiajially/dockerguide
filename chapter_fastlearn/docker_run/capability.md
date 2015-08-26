
## Runtime constraints on CPU and memory

***目前相关资料还没有收齐，还在学习之中***


下面的参数可以用来调整container内的性能参数。

| 参数 | 描述|
| -- | -- |
|-m, --memory=""| Memory limit (format: <number>[<unit>], where unit = b, k, m or g)|
|--memory-swap=""|Total memory limit (memory + swap, format: <number>[<unit>], where unit = b, k, m or g)|
|-c, --cpu-shares=0|CPU shares (relative weight)|
|--cpu-period=0|Limit the CPU CFS (Completely Fair Scheduler) period|
|--cpuset-cpus=""|CPUs in which to allow execution (0-3, 0,1)|
|--cpuset-mems=""|Memory nodes (MEMs) in which to allow execution (0-3, 0,1). Only effective on NUMA systems.|
|--cpu-quota=0|Limit the CPU CFS (Completely Fair Scheduler) quota|
|--blkio-weight=0|Block IO weight (relative weight) accepts a weight value between 10 and 1000.|
|--oom-kill-disable=false|Whether to disable OOM Killer for the container or not.|
|--memory-swappiness=""|Tune a container’s memory swappiness behavior. Accepts an integer between 0 and 100.|


通过docker run -m 可以很方便的调整container所使用的内存资源。如果host支持swap内存，那么使用-m可以设定比host物理内存还大的值。

同样，通过-c 可以调整container的cpu优先级。默认情况下，所有的container享有相同的cpu优先级和cpu调度周期。但你可以通过Docker来通知内核给予某个或某几个container更多的cpu计算周期。

默认情况下，使用-c或者--cpu-shares 参数值为0，可以赋予当前活动container 1024个cpu共享周期。这个0值可以针对活动的container进行修改来调整不同的cpu循环周期。

比如，我们使用-c或者--cpu-shares =0启动了C0，C1，C2三个container，使用-c/--cpu-shares=512启动了C3.这时，C0，C1，C2可以100%的使用CPU资源(1024)，但C3只能使用50%的CPU资源(512)。如果这个host的OS是时序调度类型的，每个CPU时间片是100微秒，那么C0，C1，C2将完全使用掉这100微秒，而C3只能使用50微秒。
