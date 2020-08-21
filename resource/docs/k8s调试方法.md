1. 等待pod状态
   

   1. Wait for the pod "busybox1" to contain the status condition of type "Ready".

        `kubectl wait --for=condition=Ready pod/grafana`

   2. Wait for the pod "busybox1" to be deleted, with a timeout of 60s, after having issued the "delete" command.

       ```
       kubectl delete pod/busybox1
       kubectl wait --for=delete pod/busybox1 --timeout=60s
       ```

   * 示例，重启监控组件如grafana,或openstry,如果立即stop/start，pod的服务会因主机端口占用启用失败；此时可以增加wait选项等待stop操作完成

      `kubectl wait --for=delete pod/openstry* --timeout=60s -n kube-system`

   **参考：**
   [kubectl_wait](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#wait)