1. 在 [DigitalOcean](https://cloud.digitalocean.com/kubernetes/clusters) 创建一个 Kubernetes 集群
2. 下载集群 Config 文件到 `~/.kube` 目录
3. 通过环境变量 `KUBECONFIG` 设置本地 `kubectl` 工具使用下载的配置文件

   ```sh
   export KUBECONFIG=$HOME/.kube/xxx-kubeconfig.yaml
   ```

4. 创建一个部署

   ```sh
   kubectl create deployment hello-node --image=registry.k8s.io/e2e-test-images/agnhost:2.39 -- /agnhost netexec --http-port=8080
   ```

5. 将 Pod 暴露给公网

   ```sh
   kubectl expose deployment hello-node --type=LoadBalancer --port=8080
   ```

6. 查看集群外部入口

   ```sh
   $ kubectl get services
   NAME         TYPE           CLUSTER-IP     EXTERNAL-IP      PORT(S)          AGE
   hello-node   LoadBalancer   10.245.28.38   139.59.216.184   8080:30770/TCP   14m
   kubernetes   ClusterIP      10.245.0.1     <none>           443/TCP          50m
   ```

   在这里 `hello-node` 的 `EXTERNAL-IP` 就是外部入口 IP，`PORT` 是其端口号。

7. 访问集群服务

   ```sh
   $ curl 139.59.216.184:8080
   NOW: 2024-05-05 12:44:23.149263952 +0000 UTC m=+728.843856665
   ```

   Pod 服务返回了当前时间。

8. 清理

   现在我们已经进行完了我们的第一次实验。运行下面的命令清理我们在集群中创建的资源：

   ```sh
   kubectl delete service hello-node
   kubectl delete deployment hello-node
   ```

参考：[Hello Minikube | Kubernetes Tutorial](https://kubernetes.io/docs/tutorials/hello-minikube/)