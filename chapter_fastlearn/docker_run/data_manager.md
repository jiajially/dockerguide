
##VOLUME (shared filesystems)
Docker内部以及容器之间管理数据，在容器中管理数据主要有两种方式：

    -v=[]:                 Create a bind mount with: [host-dir]:[container-dir]:[rw|ro].
                           If "container-dir" is missing, then docker creates a new volume.
    --volumes-from="":     Mount all volumes from the given container(s)

即：数据卷（Data volumes）数据卷容器（Data volume containers）