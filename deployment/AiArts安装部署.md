aiarts可基于 有网部署的平台，部署文档：https://github.com/apulis/DLWorkspace/blob/develop/docs/deployment/On-Prem/Ubuntu-machines-%E4%B8%AD%E6%96%87.md
也可基于无网络部署的平台，部署文档 麻烦朝阳给抛出来下

如果已有平台，则更新一下几个服务（分支no_network）：
./deploy.py --verbose docker push restfulapi2
./deploy.py --verbose docker push webui3
./deploy.py --nocache docker push custom-user-dashboard-frontend
./deploy.py --nocache docker push custom-user-dashboard-backend

更新aiarts服务



无网部署
----------------------------------------------------------------------
## 自动化安装脚本使用步骤

### 环境准备

1. Nvidia-driver 440.33
2. Nvidia GPUs
3. Cuda 10.2
4. Ubuntu 18.04
5. The *X86* architecture system

### 1. 使用install_pan.sh进行安装盘制作

```shel
./install_pan.sh -c -d <docker_images_directory> -p <install_dirrectory>
```

#### 参数解释

* -c：在下载安装所需的apt package的时候，同时下载依赖包。
* -d：指向docker镜像的存放目录，该路径应当在根目录下存放所有镜像的tar打包文件，并且没有其他文件或者目录。。
* -p：指向安装目录，该目录在脚本运行结束后需要拷贝到目标主机。

#### 注意事项

1. 请使用root用户执行脚本。
2. 使用脚本时，在实例命令中将"<>"内的内容替换为相应的路径位置。

2. 使用安装脚本的主机与安装目标主机需要拥有相同的操作系统、架构类型，并保证apt安装源最新。

3. 执行完成后，安装目录应当有以下文件和文件夹：

* install_DL.sh
* install_workernode.sh

- YTung.tar.gz
- config目录
- apt目录
- docker-images目录
- python2.7目录

### 2. 将安装目录拷贝到安装目标主机上

### 3. 进行安装前的配置检查

检查内容

* 需要在master节点来执行install_DL.sh

* 所有主机已经配置好root用户，并且拥有密码。

* 所有安装GPU的主机都安装好nvidia 440版本驱动。

* 确保master节点上/etc/hosts文件中已经配置好了集群所有节点（包括master自身的）的域名解析，包括短域名和长域名。短域名需要与主机名相同，长域名以：**主机名.sigsus.cn**的形式配置。

  例如，master节点主机名为ubuntu-master,则/etc/hosts文件中至少应该有以下两条记录：

  ​	xxx.xxx.xxx.xxx ubuntu-master

  ​	xxx.xxx.xxx.xxx ubuntu-master.sigsus.cn

* 请确保config.yaml文件配置与集群信息匹配，如果不知道config.yaml文件的配置情况，请与管理员联系。

### 4. 使用install_DL.sh进行安装

请按照提示完成安装，按照顺序，你应当会遇到以下需要输入的内容：

1. 欢迎界面，请按回车继续。

2. 条款确认，请输入yes继续。

3. 是否允许将master节点当作worker调度，请选择yes。（仅限于测试版本）

4. 配置worker节点信息，将会需要输入节点域名，请注意此时应当输入短域名，该短域名与该worker主机名相同；

   在配置节点信息的部分，请在任何需要确认的部分输入yes确认来完成节点间的knownhost配置，并在需要输入密码的部分输入root那么；

   配置完第一个节点以后，可以继续配置节点（此时应该是需要输入下一个节点的域名），也可以输入”quit“退出配置。

安装时间比较长，请保持耐心等待。



备注
----------------------------------------------------------------------

build 并 tag 本地镜像

# 后端分支 /home/apulis/apulis_platform/src/ClusterBootstrap/
./deploy.py --verbose webui


./deploy.py --verbose docker build restfulapi2
docker image  tag  dlworkspace_restfulapi2:latest apulistech/dlworkspace_restfulapi2:latest

./deploy.py --nocache docker build custom-user-dashboard-frontend
docker image  tag  dlworkspace_custom-user-dashboard-frontend:latest   apulistech/dlworkspace_custom-user-dashboard-frontend:latest

./deploy.py --nocache docker build custom-user-dashboard-backend
docker image  tag  dlworkspace_custom-user-dashboard-backend:latest   apulistech/dlworkspace_custom-user-dashboard-backend:latest

./deploy.py --verbose docker build openresty
docker image  tag  dlworkspace_openresty:latest   apulistech/dlworkspace_openresty:latest

./deploy.py --verbose docker build init-container
docker image  tag  dlworkspace_init-container:latest   apulistech/dlworkspace_init-container:latest


./deploy.py docker build watchdog
docker image  tag  watchdog:1.9   apulistech/watchdog:1.9

./deploy.py docker build gpu-reporter
docker image  tag  dlworkspace_gpu-reporter:latest   apulistech/dlworkspace_gpu-reporter:latest

./deploy.py docker build job-exporter
docker image  tag  job-exporter:1.9   apulistech/job-exporter:1.9

./deploy.py docker build repairmanager2
docker image  tag  dlworkspace_repairmanager2:latest   apulistech/dlworkspace_repairmanager2:latest

# /home/apulis/front-end/DLWorkspace/src/ClusterBootstrap 前端最新分支！
./deploy.py --verbose docker build webui3
docker image  tag  dlworkspace_webui3:latest   apulistech/dlworkspace_webui3:latest

nginx 配置
-----------------------------------------------------

location /AIarts {
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_buffers 16 16k;
    proxy_buffer_size 32k;
    proxy_pass http://localhost:3084/;
}
location / {
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_buffers 16 16k;
    proxy_buffer_size 32k;
    proxy_pass http://localhost:3084/;
}
location /expert {
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_buffers 16 16k;
    proxy_buffer_size 32k;
    proxy_pass http://localhost:3081;
}

AIArts
-----------------------------------------------------------
ssh root@219.133.167.42 -p 52080

apulis123

/home/apulis/AIArtsBackend/deployment
./build.sh
docker-compose down
docker-compose up -d


重启数据标注后台
-----------------------------------------------------------
标注后台
./deploy.py --nocache docker push data-platform-backend
./deploy.py kubernetes stop/start data-platform