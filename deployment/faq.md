* 强制 Kill Job方法：
	1. kubectl get pods # 查看僵死pod
	2. kubectl delete po --force <pod id> # 强制 kill pod 
	3. docker ps |grep mind* <tf> # 查看响应的容器
	4. docker top <docker id> # 查看容器内的进程
	5. kill -9 <process id> # 逐个kill容器中所有进程

* Ceph备份：

	```
	ceph-fuse -m 192.168.2.16 /mnt/fuse
	Path: /mnt/fuse
	```