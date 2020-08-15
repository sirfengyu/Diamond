本地编译Ubuntu18..04基础镜像
=========================================================================


1. 确认安装系统，工具版本信息
| OS    | Version | Arct |
|:------|:--------|:-----|
|Ubuntu | 18.04.1 | arm64|

2. 确认可用的依赖工具包，及安装源

https://repo.huaweicloud.com/ubuntu-ports/
https://mirrors.aliyun.com/pypi/simple

3. OS基础配置

# 设置时区
cat /etc/timezone
# 列出时区：
timedatectl list-timezones

timedatectl set-timezone Asia/Shanghai

RUN ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime

# control log level. 0-DEBUG, 1-INFO, 2-WARNING, 3-ERROR, default level is WARNING.
ENV GLOG_v=2
# Conda environmental options

4. 环境变量

# the root directory of run package
ENV LOCAL_ASCEND=/usr/local/Ascend
# lib libraries that the run package depends on
ENV LD_LIBRARY_PATH=\
/usr/lib/aarch64-linux-gnu/hdf5/serial:\
${LOCAL_ASCEND}/add-ons:\
${LOCAL_ASCEND}/driver/lib64/common:\
${LOCAL_ASCEND}/driver/lib64/driver:\
/usr/local/lib\:\
/usr/lib/

# Python library that TBE implementation depends on
ENV PYTHONPATH=${TBE_IMPL_PATH}:${PYTHONPATH}
ENV ASCEND_OPP_PATH=/usr/local/Ascend/ascend-toolkit/20.0.RC1/arm64-linux_gcc7.3.0/opp

6. 执行编译

# Build 
docker build -f ./Dockerfile-tf-withtools-rc1  .  -t tf:1.15-rc1
docker build -t ubuntu:18.04-python375-withtools -f ./Dockerfile-ubuntu1804 .  

7. 查看镜像基本信息
# Docker 查看镜像详情
docker inspect docker.io/mysql:5.7
docker history --no-trunc docker.io/mysql:5.7

8. 执行镜像，验证工具包是否可执行
# docker run

docker run -it --rm --privileged -v /dlwsdata/storage/:/data/ -v /var/log/npu/:/var/log/npu/ -v /var/log/npu/:/var/log/npu/ tf:1.15-0728 bash
docker run -it --rm --privileged -v /dlwsdata/storage/:/data/ -v /var/log/npu/:/var/log/npu/ -v /var/log/npu/:/var/log/npu/ tf:1.15-rc1 bash
docker run -it --rm --privileged --shm-size=16g -v /dlwsdata/storage/:/data/ -v /var/log/npu/:/var/log/npu/ -v /var/log/npu/:/var/log/npu/ tf:1.15-rc1 bash 

python3.7
import tensorflow
import npu_bridge

apt install libpython3.7 
cd /usr/local/Ascend/nnae/20.0.RC1/arm64-linux_gcc7.3.0/fwkacllib/lib64
 ldd libte_fusion.so

 libpython3.7m.so.1.0