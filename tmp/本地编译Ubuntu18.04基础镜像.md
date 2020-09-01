本地编译Ubuntu18.04基础镜像
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


4. 环境变量
默认保留python3.7环境
cp Python-3.7.5/libpython3.7m.so.1.0 /usr/lib

安装apt基本依赖
build-essential libhdf5-dev pkg-config  libatlas-base-dev gfortran

6. 执行编译

# Build 
docker build -t ubuntu:18.04-python375-withtools -f ./Dockerfile-ubuntu1804 .  

7. 查看镜像基本信息

# Docker 查看镜像详情
docker inspect docker.io/mysql:5.7
docker history --no-trunc docker.io/mysql:5.7

8. 执行镜像，验证工具包是否可执行
# docker run

docker run -it --rm docker run -it --rm ubuntu:18.04-python375-withtools bash bash
python3.7
pip3.7 list
ls -la /usr/lib

9. tag , 同步镜像

docker tag ubuntu:18.04-python375-withtools apulistech/ubuntu:18.04-python375-withtools
docker push apulistech/ubuntu:18.04-python375-withtools


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
