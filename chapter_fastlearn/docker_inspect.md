
## inspect

* 用法


    Usage: docker inspect [OPTIONS] CONTAINER|IMAGE [CONTAINER|IMAGE...]

    Return low-level information on a container or image

    -f, --format=         Format the output using the given go template
    --help=false          Print usage
    -r, --remote=false    Inspect remote images



* 例子


    $ sudo docker inspect centos
	[
	{
		"Id": "770327a1e9e746cf8d4449a7134e87917982b33c7f5cea584d941350f5ead7ac",
		"Parent": "67c02c69a0fc420e781b9a1c676f19306e999aac2cf3ba24dfa4e0b9a5e34b5e",
		"Comment": "",
		"Created": "2015-07-24T01:06:38.020790544Z",
		"Container": "9163378b9f7fe2887bce56cb726c3845ce9af8ebc9cbef30d3af6315429a27ad",
	"ContainerConfig": {
		"Hostname": "9163378b9f7f",
		"Domainname": "",
		"User": "",
		"AttachStdin": true,
 		...
 		...
 		...
		}

取出某一个值：

    $ sudo docker inspect --format="{{.Id}}" centos
	770327a1e9e746cf8d4449a7134e87917982b33c7f5cea584d941350f5ead7ac


* 总结

查看容器运行时详细信息的命令。了解一个Image或者Container的完整构建信息就可以通过这个命令实现。
