杭州演示环境问题列表
------------------------------------------------------------------------------------------
* 访问链接

`http://121.46.24.77:50088`

* 访问用户

|用户名   |　密码　  |　角色    | 资源  |
|:------|:----------|:--------|:-----|
xidian  | 123456    | 普通用户 | 1
zjdx    | 123456    | 普通用户 | 2 
dndx    | 123456    | 普通用户 | 4
shjd    | 123456    | 普通用户 | 8 
administrator | 123456  | 管理员 | 多机

* 目前的两台atlas登录方式

|主机名称|主机IP     |端口号|
|:------|:----------|:-----|
atlas01 | 121.46.24.77 | 50021
atlas02 | 121.46.24.77 | 50025
root登录，密码与之前相同

* tensorflow 示例
    + 镜像：apulistech/tf:1.15-arm-rc1
    + 单机多卡cmd: sudo -E bash -c 'source /pod.env && cp -r /data/Resnet50_HC /tmp/ && cd /tmp/Resnet50_HC/  && ./run_apulis_rc1.sh && sleep infinity'
    + 多机多卡cmd: sudo -E bash -c 'source /pod.env && cp -r /data/Resnet50_HC /tmp/ && cd /tmp/Resnet50_HC/  && ./run_dis_rc1.sh && sleep infinity'

* mindspore 示例
    + 镜像：apulistech/tf:1.15-arm-rc1
    + 单机多卡cmd: sudo -E bash -c 'source /pod.env && cp -r /data/resnet50_050/ /tmp/ && cd /tmp/resnet50_050/scripts/ && mkdir -p /var/log/npu/conf/slog/ && cp slog.conf /var/log/npu/conf/slog/ && ./run_single.sh && sleep infinity'
    + 多机多卡cmd: sudo -E bash -c 'source /pod.env && cp -r /data/resnet50_050/ /tmp/ && cd /tmp/resnet50_050/scripts/ && mkdir -p /var/log/npu/conf/slog/ && cp slog.conf /var/log/npu/conf/slog/ && ./run_distribute.sh resnet50 cifar10 /home/$DLWS_USER_NAME/.npu/$DLWS_JOB_ID/hccl_ms.json /data/resnet50_cifar10/cifar/ && sleep infinity'

* 备份镜像目录：

    + `atlas02:/mnt/disk1/docker-images#`

* 镜像同步目录：

    `atlas01:/home/apulis_ftp_user/ftp/`

* k8s 镜像同步策略：

    + Always: 一直会从docker hub 同步镜像，如果已经同步则会使用docker缓存
    + IfNotPresent: 优先查看本地已经load的镜像，如果没有再从docker hub同步

* atc安装

路径： `/dlwsdata/storage/atc`

* k8s 安装源

```
cat /etc/apt/sources.list.d/kubernetes.list
deb http://ports.ubuntu.com/ubuntu-ports bionic-security main
```

