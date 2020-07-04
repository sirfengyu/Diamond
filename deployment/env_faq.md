* 强制 Kill Job方法：
	1. Kubectl get pods # 查看僵死pod
	2. Kubectl delete po --force <pod id> # 强制 kill pod 
	3. Docker ps |grep mind* <tf> # 查看响应的容器
	4. Docker top <docker id> # 查看容器内的进程
	5. Kill -9 <process id> # 逐个kill容器中所有进程

* Ceph备份：

	```
	ceph-fuse -m 192.168.2.16 /mnt/fuse
	Path: /mnt/fuse
	```