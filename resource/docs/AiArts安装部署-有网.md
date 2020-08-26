
专家系统
----------------------------------------------------------------------


* build 并 tag 本地镜像

```bash
# 后端分支代码路径 
cd /home/apulis//DLWorkspace/src/ClusterBootstrap/
git checkout no_network
git pull
# 重新生成平台配置
./deploy.py --verbose webui

# 更新平台组件
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

```

* 确认 nginx 中AI-Arts配置

```yaml
cat  vim /etc/nginx/conf.other/default.conf
# 核对是否有以下新增的配置
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
```
* 更新平台nginx 配置

```bash
./deploy.py --verbose nginx fqdn
./deploy.py --verbose nginx config
```
* 确认更新后的nginx配置

```bash

root@atlas-gpu01:/home/apulis/DLWorkspace/src/ClusterBootstrap# ll /www/static/dashboard
total 48
drwxr-xr-x 3 root root  4096 Aug  3 09:18 ./
drwxr-xr-x 3 root root  4096 Aug  5 03:24 ../
-rw-r--r-- 1 root root  6273 Aug  3 09:18 asset-manifest.json
-rw-r--r-- 1 root root  3870 Aug  3 09:16 favicon.ico
-rw-r--r-- 1 root root  4423 Aug  3 09:18 index.html
-rw-r--r-- 1 root root 10508 Aug  3 09:18 precache-manifest.b35ce6dc0e55a41796c9764926e0a4d0.js
-rw-r--r-- 1 root root  1203 Aug  3 09:18 service-worker.js
drwxr-xr-x 5 root root  4096 Aug  3 09:18 static/
```
* 重启服务(AMD64)

```bash
./deploy.py --verbose kubernetes stop/start jobmanager2 restfulapi2 nginx custommetrics repairmanager2 openresty
./deploy.py --verbose kubernetes stop/start monitor

# 需要注意等待相关组件grafana,openstry,jobmanager 的 pod已经terminal后再启用
kubectl wait --for=delete pod/openstry** --timeout=60s

./deploy.py --verbose nginx webui3
./deploy.py --verbose kubernetes stop/start webui3
./deploy.py kubernetes stop/start custom-user-dashboard
```

AIArts
-----------------------------------------------------------
*  后端
```
# 如果需要重新拉取代码库
# git clone https://github.com/apulis/AIArtsBackend.git 

cd /home/apulis/AIArtsBackend/deployment
git checkout master
git pull
cat README.md
./build.sh
docker-compose down
docker-compose up -d
```
* 前端

```
# 如果需要重新拉取代码库
# git clone https://github.com/apulis/AIArts.git 

cd /home/apulis/front-end/AIArts
git checkout master
git pull
cat README.md
./build.sh
docker-compose down
docker-compose up -d
```

数据标注平台
-----------------------------------------------------------
* 后台
```bash
cd /home/apulis//DLWorkspace/src/ClusterBootstrap/
git checkout no_network
git pull
./deploy.py --nocache docker push data-platform-backend
./deploy.py kubernetes stop/start data-platform
```
* 前台：
```bash
# 如果需要重新拉取代码库
# git clone https://github.com/apulis/NewObjectLabel.git

cd /home/apulis/front-end/NewObjectLabel/
git checkout master
git pull
cat README.md
./build.sh
docker-compose down
docker-compose up -d
```

FAQ
-------------------------------------------------------------------

1. 在更新前端配置时可能遇到token失效无法拉取代码

    **解决方法：**
    更新代码库中该组件的dockerfile
    ```bash
    cd /home/apulis//DLWorkspace/src/ClusterBootstrap/
    # 以custom-user-dashboard-backend为例
    vim ../docker-images/custom-user-dashboard-backend/Dockerfile
    FROM node:12

    RUN mkdir -p /home/addon_custom_user_dasboard_backend
    WORKDIR /home/addon_custom_user_dasboard_backend
    # 将以下的token '7a3c38292b740a6c6d7db08b759fbbe7f3c1ece3'更新为最新,或者替换为有效的
    RUN git clone --branch dev-with-i18n https://7a3c38292b740a6c6d7db08b759fbbe7f3c1ece3@github.com/apulis/addon_custom_user_dashboard_backend.git /home/addon_custom_user_dasboard_backend
    RUN yarn config set registry 'https://registry.npm.taobao.org'
    RUN yarn
    RUN yarn build

    EXPOSE 5001

    CMD ["yarn", "start:prod"]
    ```

    *数据标注后端data-platform，也是同样处理`../docker-images/data-platform-backend/Dockerfile`*

2. 所有组件已经running，但是WEB打开home也无响应
    **解决方法：**
    检查default域下的nginx服务状态
    ```bash
    kubectl get pods
    kubectl exec -it nginx-cvs26 bash
    ps aux 
    # 检查nginx服务进程是否已启用
    root        26  0.0  0.0  10884  1132 ?        Ss   Aug25   0:00 nginx: master process nginx
    nginx       27  0.1  0.0  41288 34140 ?        S    Aug25   0:44 nginx: worker process
    # 否则启用nginx
    nginx
    # 如果提示配置错误，是因为没有外网域名
    vim /etc/nginx/conf.d/default.conf
    # 删除第5行的servername
    # 再次启用nginx
    nginx
    ```

