## 内存资源设置


设置内存我们可以有四种方式：

* **memory=inf, memory-swap=inf (default)**

    *默认的方式设置最低值，容器可以使用大于此最低值的内存数*

* **memory=L<inf, memory-swap=inf**

    *设置memory不能使用超过L的值。*

* **memory=L<inf, memory-swap=2*L**

* **memory=L<inf, memory-swap=S<inf, L<=S**

    *memory不能超过L，swap＋memory总使用量不能超过S*

例子：
	$ sudo docker run -ti centos /bin/bash

默认不设置任何限制。(第一种情况)

	$ sudo docker run -ti -m 300M --memory-swap -1 centos  /bin/bash
memory最多使用300M，swap没有限制

	$ docker run -ti -m 300M centos  /bin/bash

我们只设置了memory限制时300M，swap没有指定，默认被设置为与memory一样的值。memory＋swap一共是600M

	$ docker run -ti -m 300M --memory-swap 1G centos  /bin/bash

这里我们同时设置了memory和swap ，对应第四种情况

如果发生内存溢出错误，内核讲kill掉容器中的进程。如果你想控制，可以配合使用- -oom-kill-disable参数。如果没有制定-m参数，可能导致当内存溢出时内核会杀死主机进程。
例子：
设置容器内存限制100M，并且阻止 OOM killer

	$ docker run -ti -m 100M --oom-kill-disable centos /bin/bash

如果不使用-m参数制定限制，官方说很危险！

