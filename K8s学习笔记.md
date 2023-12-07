# K8s学习笔记

Kubernetes 是谷歌用go语言翻写的borg（也是google家的）架构

特点：轻量级:消耗资源少    开源    弹性伸缩		负载均衡：IPVS 

 服务分类：有状态服务： DBMS ，集群化管理服务Zookeeper、etcd等

​					无状态服务：LVS（高性能负载均衡），APACHE，docker





高可用集群副本数据最好是>=3 的奇数个（如3.5.7.9）





1. **Kube-apiserver（API Server）**:
   - 作用：提供了Kubernetes API的访问点，允许用户、管理员和其他组件与集群进行交互。
2. **etcd**:
   - 作用：分布式键值存储，用于保存集群的配置数据和状态信息，是 Kubernetes 集群的主要数据存储。
3. **Kube-scheduler**:（调度器）
   - 作用：负责将新创建的 Pod 分配给节点，根据节点的资源和约束条件来选择合适的节点。
4. **Kube-controller-manager**:(工作负载控制器)
   - 作用：运行多个控制器，用于监控和维护集群中的不同资源，例如 Replication Controller、Service、Namespace 等。
5. **Kubelet**:（生命周期管理）
   - 作用：在每个节点上运行，负责管理节点上的容器，包括容器的创建、销毁、监控和报告节点的资源使用情况。
6. **Kube-proxy**:
   - 作用：维护网络规则，负责将集群内的流量路由到正确的 Pod，提供了服务发现和负载均衡功能。
7. **Container Runtime**:
   - 作用：Kubernetes 支持多种容器运行时，例如 Docker、containerd 等。容器运行时负责管理容器的生命周期和资源隔离。
8. **CNI（Container Network Interface）**:
   - 作用：定义了容器网络规范，允许不同的网络插件在 Kubernetes 中创建和管理容器网络。
9. **Ingress Controller**:
   - 作用：处理入站网络流量，允许从集群外部访问集群内的服务，并提供路由、负载均衡和 SSL 终止等功能。
10. **DNS**:
    - 作用：提供内部服务发现，为 Pod 提供域名解析，使它们可以通过服务名称进行通信。
11. **Dashboard**:
    - 作用：提供了一个基于 Web 的用户界面，用于管理和监控集群，查看资源使用情况，部署应用程序等。
12. **Prometheus**:
    - 作用：用于集群监控和报警，帮助管理员和开发人员了解集群的性能和健康状况。
13. **Heapster**:
    - 作用：为集群提供性能和资源利用的监控数据，可以与其他监控和日志系统集成。

```
Controller 主要用于管理和维护 Pod 的复制和状态，以确保应用程序的高可用性和滚动更新。
Service 主要用于提供应用程序的网络访问，以确保应用程序可以被其他组件访问，并提供负载均衡和服务发现功能。
在K8S集群中，微服务的负载均衡是由kube-proxy实现的。kube-proxy是k8s集群内部的负载均衡器。
kube-proxy 是一个网络代理，主要负责维护集群内部的网络规则和负载均衡，确保流量可以正确路由到后端Pod。它工作在每个节点上。
Service 是一个资源对象，它定义了一组Pod并创建了一个虚拟网络入口。Service资源使用kube-proxy来实现负载均衡，确保从Service IP到后端Pod的流量负载均衡。
kube-proxy和Service通常协同工作，kube-proxy负责在节点上设置负载均衡规则，而Service定义负载均衡的行为。
总之，kube-proxy和Service是Kubernetes中用于负载均衡的不同组件，共同协作以确保应用程序的高可用性和负载均衡。kube-proxy处理底层网络规则，而Service定义了要负载均衡的应用程序服务。
Service是一种工作于传输层的负载均衡器，而Ingress是在应用层实现的HTTP(s)负载均衡机制。不过，Ingress资源自身不能进行“流量穿透”，它仅是一组路由规则的集合，这些规则需要通过Ingress控制器（Ingress Controller）发挥作用。目前，此类的可用项目有Nginx、Traefik、Envoy及HAProxy等。
```



##  Pod存在的意义

- 创建容器使用docker，一个docker对应一个容器，一个容器运行一个应用进程
- Pod是多进程设计，运用多个应用程序，也就是一个Pod里面有多个容器，而一个容器里面运行一个应用程序



## K8S的业务主要可以分为以下几种

- 长期伺服型：long-running      Deployment
- 批处理型：batch 			 Job
- 节点后台支撑型：node-daemon      DaemonSet
- 有状态应用型：stateful application   StatefulSet



## 命令合集

```kubectl describe pod <Pod名称>
kubectl create deployment nginx --image=nginx   # 下载nginx 【会联网拉取nginx镜像】 这里就会生成一个名字为nginx的pod
kubectl get pod  # 查看状态
kubectl expose deployment nginx --port=80 --type=NodePort # 暴露端口
kubectl get pod,svc/kubectl get service # 查看一下对外的端口 
kubectl get pods -o wide  查看所有pod分配在哪个node中（分配情况）
kubectl describe pod <Pod名称>  查看这个pod的详细内容
kubectl delete service <Service名称> 删除service
kubectl delete deployment <deployment/pod名称>删除一个pod
```

```shell
比如说我有一个已部署的deployment名字为web
[root@k8s-master ~]# kubectl get deploy
NAME   READY   UP-TO-DATE   AVAILABLE   AGE
web    1/1     1            1           19h
kubectl get deploy web -o=yaml --export > web.yaml
[root@k8s-master ~]# kubectl get deploy web -o=yaml --export > web.yaml  #会在当前目录下生成一个web.yaml文件
kubectl get pod nginx-67685f79b5-8rjk7 -o yaml    #获取该pod的配置清单
获取了yaml文件就可以对其修改
Kubectl apply -f [name.yaml] 就可以更新这个deploy或者pods
```

```shell
这个过程要注意生成的yaml里面的name能不能对的上
kubectrl create deployment web --image=nginx 传统方式创建一个pod
kubectl create deployment web --image=nginx --dry-run -o yaml > nginx.yaml 把创建过程生成一个yaml文件，方便复用
kubectl apply -f nginx.yaml 就能快速生成一个pod
kubectl expose deployment web --port=80 --type=NodePort --target-port=80 --name=web1 -o yaml > web1.yaml 把暴露端口的过程生成一个yaml文件
kubectl apply -f web1.yaml   就能快速曝光端口
kubectl exec -it <pod-name> -- /bin/bash 进入容器
kubectl exec -it <pod-name> -- /bin/sh -c "kill 1"	重启pod中的容器，容器内的 Nginx 主进程的进程号为 1
```

## 应用的升级与回滚

```shell
kubectl set image deployment web nginx=nginx:1.15 指定将nginx的镜像版本变成1.15 
kubectl get pod 可以看到升级过程（升级可以保证服务不中断）
Kubectl get pods -o wide或者kubectl describe pods web1自己查在哪个node上，然后docker images可以看到镜像版本
kubectl rollout status deployment web  查看状态
kubectl rollout undo deployment web    回滚到上一个版本
kubectl rollout history deployment web 查看版本变更历史
kubectl rollout undo deployment web --to-revision=2 #回滚到指定版本
kubectl get pods -o custom-columns=Name:metadata.name,Image:spec.containers[0].image #查看pod资源对象的name和image
```

- maxSurge：指定升级期间存在的总`Pod`对象数量最多可超出期望值的个数，其值可以是`0`或正整数，也可以是一个期望值的百分比；例如，如果期望值为`3`，当前的属性值为`1`，则表示`Pod`对象的总数不能超过`4`个。（如果我的pod设置的副本是3个，这里填写了maxsurge：1，那么升级时就会有4个pod存在）

- maxUnavailable：升级期间正常可用的`Pod`副本数（包括新旧版本）最多不能低于期望值的个数，其值可以是`0`或正整数，也可以是期望值的百分比；默认值为`1`，该值意味着如果期望值是`3`，则升级期间至少要有两个`Pod`对象处于正常提供服务的状态。

  ```
  [root@k8s-master ~]$kubectl get pods -o custom-columns=Name:metadata.name,Image:spec.containers[0].image
  Name                     Image
  myapp-55dcbf9fdc-8l4j9   ikubernetes/myapp:v1
  myapp-55dcbf9fdc-9fppf   ikubernetes/myapp:v1
  myapp-55dcbf9fdc-d2j88   ikubernetes/myapp:v1
  myapp-55dcbf9fdc-g8k4r   ikubernetes/myapp:v1
  myapp-55dcbf9fdc-xqmh5   ikubernetes/myapp:v1
  myapp-67dd64bf6f-q99c8   ikubernetes/myapp:v3 #这就是由于设置了maxsurge而多出来的一个pod，且image版本为v3
  
  ```

  

## service

##  Service常用类型

Service常用类型有三种

- ClusterIp：集群内部访问

- NodePort：对外访问应用使用

- LoadBalancer：对外访问应用使用，公有云

  一个deployment可以创建多个service访问，只需改名字就行

```shell
[root@k8s-master ~]# kubectl expose deployment webtest --port=80 --target-port=80 --dry-run -o yaml > service.yaml
将暴露窗口的操作生成一个yaml文件，以便复用。此操作并没有实际操作
```

这个yaml文件并没有TYPE这个字段，因为上面的命令没有指出到底是使用什么类型，所以需要自己增加，一定要注意缩进量，不然执行会默认为ClusterIP  

```shell
apiVersion: v1
kind: Service
metadata:
  creationTimestamp: null
  labels:
    app: web
  name: webtest4
spec:
  ports:
  - port: 80
    protocol: TCP
    targetPort: 80
  selector:
    app: web
  type: NodePort   //type 字段应该是 spec 下面的一个字段，而不是 selector 中的一部分（注意缩进量）
status:
  loadBalancer: {}

```

## 资源清单

```shell
kubectl get pods --show-labels     #查看pod信息时，并显示对象的标签信息
kubectl get pods -l app,tier    #过滤同时包含app，tier标签的pod
kubectl label pods/pod-demo env=production    #给pod资源pod-demo添加env标签值为production，标签可以任意写
kubectl label pods/pod-demo env=testing --overwrite   #修改已有标签信息，同上面添加标签一样，只是添加--overwrite参数
```

## initContainers

`initContainers` 是 Kubernetes 中的一个概念，用于在一个 Pod 中定义需要在应用容器运行之前执行的容器。它们是一种初始化容器，通常用于执行一些在主应用程序容器启动之前必须完成的初始化任务。

```bash
[root@k8s-master ~]# vim manfests/init-pod-demo.yaml
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
  labels:
    app: myapp
spec:
  containers: #这里是主任务
  - name: myapp
    image: busybox
    command: ['sh', '-c', 'echo The app is running! && sleep 3600']
  initContainers: #这里是初始化任务，初始化任务完成之前pod一直处于init状态，也就是初始化状态
  - name: init-myservice
    image: busybox
    command: ['sh', '-c', 'until nslookup myservice; do echo waiting for myservice; sleep 2; done;']
  - name: init-mydb
    image: busybox
    command: ['sh', '-c', 'until nslookup mydb; do echo waiting for mydb; sleep 2; done;']
```



## ReplicaSet

```shell
可以直接通过vim 编辑清单文件修改replicas字段，也可以通过kubect edit 命令去编辑。kubectl还提供了一个专用的子命令scale用于实现应用规模的伸缩，支持从资源清单文件中获取新的目标副本数量，也可以直接在命令行通过“--replicas”选项进行读取。
kubectl edit rs myapp  快速更新replicas数量 #进入文件找到下面的字段进行更改就行
replicas: 4
kubectl scale replicasets myapp --replicas=5    #将上面的Deployments控制器myapp的Pod副本数量提升为5个
也可以直接修改相关的yaml文件，如何apply
kubectl apply -f rs.yaml #就可以更变副本数量了
使用kubectl patch打补丁的方式进行扩容
[root@k8s-master ~]# kubectl patch deployment deploy-demo -p '{"spec":{"replicas":5}}'
```

使用`Kubectl delete`命令删除`ReplicaSet`对象时默认会一并删除其管控的各`Pod`对象，有时，考虑到这些`Pod`资源未必由其创建，或者即便由其创建也并非自身的组成部分，这时候可以添加`“--cascade=false”`选项，取消级联关系。

```shell
kubectl delete rs myapp #使用这个命令会把myapp的副本全部删除并且将其pod也删除掉
kubectl delete replicasets myapp --cascade=false #只删除replicas副本，不删除正在使用的pods
```

## StatefulSet

`StatefulSet`是`Pod`资源控制器的一种实现，用于部署和扩展有状态应用的`Pod`资源，确保它们的运行顺序及每个`Pod`资源的唯一性。其应用场景包括：

- 稳定的持久化存储，即`Pod`重新调度后还是能访问到相同的持久化数据，基于`PVC`来实现。
- 稳定的网络标识，即`Pod`重新调度后其`PodName`和`HostName`不变，基于`Headless Service`（即没有`Cluster IP`的`Service`）来实现
- 有序部署，有序扩展，即`Pod`是有顺序的，在部署或者扩展的时候要依据定义的顺序依次进行（即从0到N-1，在下一个`Pod`运行之前的所有之前的`Pod`必须都是`Running`和`Ready`状态），基于`init Containers`来实现
- 有序收缩，有序删除（即从N-1到0）

在创建`StatefulSet`之前需要准备的东西，创建顺序非常关键，如下

1、Volume

2、Persistent Volume

3、Persistent Volume Clain

4、Service

5、StatefulSet

Volume可以有很多中类型，比如nfs、gluster等，下面使用nfs

## Headless无头服务

Headless 服务的特点包括：

1. **无 Cluster IP**：Headless 服务没有 Cluster IP，这意味着它不提供负载均衡功能。每个 Pod 的 IP 地址将成为服务的 DNS 记录，客户端可以直接使用这些 IP 地址来访问 Pod。

2. **服务 DNS 记录**：Kubernetes 为 Headless 服务的每个 Pod 创建 DNS 记录。这些 DNS 记录的格式通常是 `pod-name.namespace.svc.cluster.local`。

3. **用途**：Headless 服务通常用于 StatefulSet 中，这是一种用于运行有状态应用程序（如数据库或消息队列）的控制器。每个 StatefulSet Pod 都需要一个唯一的标识，因此可以通过 Headless 服务的 DNS 记录来访问。

4. **服务发现**：Headless 服务在没有额外的负载均衡器的情况下，可以直接用于服务发现。这对于需要直接连接到某个特定 Pod 的应用程序非常有用。

   

   

   

   

   

## Ingress服务发现

`Kubernetes`提供了两种内建的云端负载均衡机制（`cloud load balancing`）用于发布公共应用：一种是工作于传输层的`Service`资源，它实现的是`“TCP负载均衡器”`（传输层），另一种是`Ingress`资源，它实现的是`“HTTP(S)负载均衡器”`。（应用层）
