## Docker 核心技术

Docker核心是一个操作系统级虚拟化方法, 理解起来可能并不像VM那样直观。我们从虚拟化方法的四个方面：

1. 隔离性           Namespace
2. 可配额/可度量    Cgroups
3. 便携性           AUFS
4. 安全性           AppArmor、SELinux、GRSEC


接下来将详细介绍Docker的技术细节。



