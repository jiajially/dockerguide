## 控制组 - Control Groups (cgroups) 可配额、可度量

cgroups 实现了对资源的配额和度量。 cgroups 的使用非常简单，提供类似文件的接口，在 /cgroup目录下新建一个文件夹即可新建一个group，在此文件夹中新建task文件，并将pid写入该文件，即可实现对该进程的资源控制。groups可以限制blkio、cpu、cpuacct、cpuset、devices、freezer、memory、net_cls、ns九大子系统的资源，以下是每个子系统的详细说明：

* blkio 这个子系统设置限制每个块设备的输入输出控制。例如:磁盘，光盘以及usb等等。
* cpu 这个子系统使用调度程序为cgroup任务提供cpu的访问。
* cpuacct 产生cgroup任务的cpu资源报告。
* cpuset 如果是多核心的cpu，这个子系统会为cgroup任务分配单独的cpu和内存。
* devices 允许或拒绝cgroup任务对设备的访问。
* freezer 暂停和恢复cgroup任务。
* memory 设置每个cgroup的内存限制以及产生内存资源报告。
* net_cls 标记每个网络包以供cgroup方便使用。
* ns 名称空间子系统。

以上九个子系统之间也存在着一定的关系.详情请参阅[官方文档](https://www.kernel.org/doc/Documentation/cgroups/)。

