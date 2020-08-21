715演示项测试结果：
-------------------------------------------------------------------------------------------------

1. 小规模异构集群环境信息展示（1*x86-GPU+1*Atlas），包含用户登录、账户管理、集群监控信息查看；
   * 测试结果： PASS

2. 集群扩容，增加一台x86-GPU服务器、一台Atlas服务器，查看扩容后的集群信息是否匹配、监控信息是否正常；
   * 测试结果： N/A
   * 测试详情：
        1. 在已部署的集群中缩容1台x86-GPU服务器，在扩容该服务器时在node join过程和启用labelservice/worker过程都存在配置冲突；
        2. 在已部署的集群中缩容1台x86-GPU服务器，然后执行kubeadm reset，并删除配置 `rm -rf /etc/cni/net.d/&&rm -rf $HOME/.kube`,在node join过程和启用labelservice/worker过程都存在配置冲突；
        3. 可以缩容一台Atlas服务器，缩容后，还可以将该Atlas服务器扩容进集群

3. MindSpore多机多卡分布式训练，查看训练过程中对资源的使用情况、训练结束资源释放情况；
   * 测试结果： N/A
   * 测试详情： 2 node * 8NPU Job可以running，但是MindSpore进程出错 `TDT pushing data failed!`;NPU资源监控没有波动！

4. GPU任务，展示notebook可调试、编辑任务；     
   * 测试结果： PASS
   * 测试详情： 仅验证了单节点GPU上情况

5. Atlas节点缩容，形成2*x86-GPU+1Atlas异构集群后，运行TensorFlow单机8卡NPU训练任务，查看训练过程中的资源使用情况、训练结束资源释放情况；
   * 测试结果： PASS
   * 测试详情： 缩容atlas01，在扩容atlas01都可以；当前集群状态为 2 atlas-1*8x86GPU，
             TensorFlow在atlas01 上单机8卡NPU 训练任务可以到running状态，能够执行finished，Job 的grafana NPU资源监控到使用状态的波动。

6. X86-GPU节点缩容，形成1*x86-GPU+1Atlas异构集群，演示多GPU任务根据优先级，分配资源（抢占）。
   * 测试结果： PASS
   * 测试详情： 缩容x86GPU02,形成集群状态为 1atlas-1*8x86GPU，（测试时可能在2atlas-1*8x86GPU情况下）
             提交多个Job,配置不同优先级，优先级高的Job会先被分配资源执行；需要注意Preemptible, 优先于Priority!

**需要紧急处理的问题：**

1. GPU01 在反复重启，不能正常加入集群；
2. atlas01,atlas02,GPU01,GPU02重启后DNS丢失，不能正常同步或更新外网资源；
3. GPU01,GPU02节点重启后，.3网段失效，通过外网段不能正常访问；
4. 新加入的FD窗口Edge Inference缺少配置或合入，窗口空白！
5. 用户管理和权限管理的版本与平台版本没有同步，平台版本在发布时必须事先沟通确认，在代码中将其dockerfile中版本分支更新；避免环境部署之后才发现用户管理没有更新，再次编辑代码更新前端的操作！
6. GPU节点扩容Fail

**尚需注意或决策的情况:**

1. 集群环境组合为 2*x86-GPU+2Atlas；其中 atlas02 配置时作为master，当集群部署完成后，atlas02的NPU资源没有展示（不可用）

    后经排查发现，需要将atlas再设置为worker（或充当为worker），此时atlas02的NPU资源才展示。

2. `sudo -E bash -c 'cd /data/resnet50_cifar10/ && mkdir -p /var/log/npu/conf/slog/ && cp slog.conf /var/log/npu/conf/slog/ && ./run_8p.sh &&sleep 10m'`
    
    模型已执行完，该 cmd 在 30min 后都没结束！