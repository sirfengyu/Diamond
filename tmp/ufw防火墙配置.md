
[ERROR] DRV(2680,npu-smi):2020-08-06-00:15:34.051.069 [hardware/dev_plat/dsmi/src/dsmi_common/dsmi_c



root@atlas03:/home/HwHiAiUser/apulis_platform/src/ClusterBootstrap# ufw status
Status: active

To                         Action      From
--                         ------      ----
22/tcp                     ALLOW       Anywhere
2049/tcp                   DENY        Anywhere
9300/tcp                   DENY        Anywhere
3399/tcp                   ALLOW       Anywhere
3399/udp                   ALLOW       Anywhere
22/udp                     ALLOW       Anywhere
80/tcp                     ALLOW       Anywhere
80/udp                     ALLOW       Anywhere
6443/udp                   ALLOW       Anywhere
6443/tcp                   ALLOW       Anywhere
111/tcp                    ALLOW       Anywhere
111/udp                    ALLOW       Anywhere
2049/udp                   ALLOW       Anywhere
13025/tcp                  DENY        Anywhere
13025/udp                  DENY        Anywhere
1110/udp                   ALLOW       Anywhere
1110/tcp                   ALLOW       Anywhere
2049                       DENY        Anywhere
111                        ALLOW       Anywhere
13025                      ALLOW       Anywhere
Anywhere                   ALLOW       192.168.100.23
Anywhere                   ALLOW       192.168.100.25
Anywhere                   ALLOW       192.168.100.0/24
22/tcp (v6)                ALLOW       Anywhere (v6)
2049/tcp (v6)              DENY        Anywhere (v6)
9300/tcp (v6)              DENY        Anywhere (v6)
3399/tcp (v6)              ALLOW       Anywhere (v6)
3399/udp (v6)              ALLOW       Anywhere (v6)
22/udp (v6)                ALLOW       Anywhere (v6)
80/tcp (v6)                ALLOW       Anywhere (v6)
80/udp (v6)                ALLOW       Anywhere (v6)
6443/udp (v6)              ALLOW       Anywhere (v6)
6443/tcp (v6)              ALLOW       Anywhere (v6)
111/tcp (v6)               ALLOW       Anywhere (v6)
111/udp (v6)               ALLOW       Anywhere (v6)
2049/udp (v6)              ALLOW       Anywhere (v6)
13025/tcp (v6)             DENY        Anywhere (v6)
13025/udp (v6)             DENY        Anywhere (v6)
1110/udp (v6)              ALLOW       Anywhere (v6)
1110/tcp (v6)              ALLOW       Anywhere (v6)
2049 (v6)                  DENY        Anywhere (v6)
111 (v6)                   ALLOW       Anywhere (v6)
13025 (v6)                 ALLOW       Anywhere (v6)

```hccl_8p.json
{
"group_count": "1",
"group_list": [
{
    "group_name": "worker",
    "device_count": "8",
    "instance_count": "1",
    "instance_list": [{"devices":[ 
    {"device_id":"0","device_ip":"192.168.10.3" },
    {"device_id":"1","device_ip":"192.168.20.3" },
    {"device_id":"2","device_ip":"192.168.30.3" },
    {"device_id":"3","device_ip":"192.168.40.3" },
    {"device_id":"4","device_ip":"192.168.10.4" },
    {"device_id":"5","device_ip":"192.168.20.4" },
    {"device_id":"6","device_ip":"192.168.30.4" },
    {"device_id":"7","device_ip":"192.168.40.4" } ],
    "pod_name":"another","server_id":"127.0.0.1"}]
}
],
"status": "completed"
}
```

docker run -it -v /root/sample_tf:/data/ 
--device=/dev/davinci0 
--device=/dev/davinci1 
--device=/dev/davinci2 
--device=/dev/davinci3 
--device=/dev/davinci4 
--device=/dev/davinci5 
--device=/dev/davinci6 
--device=/dev/davinci7 
--device=/dev/davinci_manager 
--device=/dev/devmm_svm 
-v /var/log/npu/conf/slog/slog.conf:/var/log/npu/conf/slog/slog.conf 
-v /var/log/npu/slog/container/7:/var/log/npu/slog 
-v /var/log/npu/profiling/container/7:/var/log/npu/profiling 
-v /var/log/npu/dump/container/7:/var/log/npu/dump 
-v /var/log/npu/docker_slog_7:/usr/slog 
--device=/dev/hisi_hdc 
-w /data/Resnet50_HC tf:1.15-0728 bash


docker build -f Dockerfile-tf-withtools-rc1 . -t tf:1.15-rc1


echo "deb https://repo.huaweicloud.com/ubuntu-ports/ bionic main restricted universe multiverse" >/etc/apt/sources.list && \
echo "deb-src https://repo.huaweicloud.com/ubuntu-ports/ bionic main restricted universe multiverse" >>/etc/apt/sources.list && \
echo "
echo "deb https://repo.huaweicloud.com/ubuntu-ports/ bionic-security main restricted universe multiverse >>/etc/apt/sources.list && \
echo "deb-src https://repo.huaweicloud.com/ubuntu-ports/ bionic-security main restricted universe multiverse 
echo "
echo "deb https://repo.huaweicloud.com/ubuntu-ports/ bionic-updates main restricted universe multiverse
echo "deb-src https://repo.huaweicloud.com/ubuntu-ports/ bionic-updates main restricted universe multiverse
echo "
echo "deb https://repo.huaweicloud.com/ubuntu-ports/ bionic-backports main restricted universe multiverse



* 容器apt源配置


echo "deb  https://repo.huaweicloud.com/ubuntu-ports/ bionic main restricted universe multiverse" >/etc/apt/sources.list && \
echo "deb-src   https://repo.huaweicloud.com/ubuntu-ports/ bionic main restricted universe multiverse" >>/etc/apt/sources.list && \
echo "deb  https://repo.huaweicloud.com/ubuntu-ports/ bionic-security main restricted universe multiverse" >>/etc/apt/sources.list && \
echo "deb-src  https://repo.huaweicloud.com/ubuntu-ports/ bionic-security main restricted universe multiverse" >>/etc/apt/sources.list && \
echo "deb  https://repo.huaweicloud.com/ubuntu-ports/ bionic-updates main restricted universe multiverse" >>/etc/apt/sources.list && \
echo "deb-src  https://repo.huaweicloud.com/ubuntu-ports/ bionic-updates main restricted universe multiverse" >>/etc/apt/sources.list && \
echo "deb  https://repo.huaweicloud.com/ubuntu-ports/ bionic-backports main restricted universe multiverse" >>/etc/apt/sources.list 
