
## 数据管理

通常情况下，我们是不会在容器中存储数据的。我们会挂载一个主机的文件夹作为数据卷到容器中去，该数据卷可以被许多需要访问的容器访问得到。这样做的好处是保证了数据的持久化，也增强来应用的可移植性（不改变容器配置）。

Docker内部以及容器之间管理数据，在容器中管理数据主要有两种方式：

    -v=[]:                 Create a bind mount with: [host-dir]:[container-dir]:[rw|ro].
                           If "container-dir" is missing, then docker creates a new volume.
    --volumes-from="":     Mount all volumes from the given container(s)

即：数据卷（Data volumes）数据卷容器（Data volume containers）
