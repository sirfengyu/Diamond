本地编译Tensorflow 1.15基础镜像
=========================================================================


1. 引用基础镜像
FROM apulistech/ubuntu:18.04-python375-withtools

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

7. 查看镜像基本信息
# Docker 查看镜像详情
docker inspect docker.io/mysql:5.7
docker history --no-trunc docker.io/mysql:5.7

8. 执行镜像，验证工具包是否可执行
# docker run

docker run -it --rm --privileged -v /dlwsdata/storage/:/data/ -v /var/log/npu/:/var/log/npu/ -v /var/log/npu/:/var/log/npu/ tf:1.15-rc1 bash

python3.7
import tensorflow
import npu_bridge

9. tag , 同步镜像

docker tag tf:1.15-rc1 apulistech/tf:1.15-arm-rc1
docker push apulistech/tf:1.15-arm-rc1


FAQ
----------------------------------------------------------------------------------------------------------

Exception in thread Thread-1543:
Traceback (most recent call last):
  File "/home/docker-build/Python-3.7.5/Lib/test/test_ssl.py", line 2386, in run
    msg = self.read()
  File "/home/docker-build/Python-3.7.5/Lib/test/test_ssl.py", line 2363, in read
    return self.sslconn.read()
  File "/home/docker-build/Python-3.7.5/Lib/ssl.py", line 931, in read
    return self._sslobj.read(len)
ssl.SSLError: [SSL: PEER_DID_NOT_RETURN_A_CERTIFICATE] peer did not return a certificate (_ssl.c:2508)

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "/home/docker-build/Python-3.7.5/Lib/threading.py", line 926, in _bootstrap_inner
    self.run()
  File "/home/docker-build/Python-3.7.5/Lib/test/test_ssl.py", line 2472, in run
    raise ssl.SSLError('tlsv13 alert certificate required')
ssl.SSLError: ('tlsv13 alert certificate required',)
