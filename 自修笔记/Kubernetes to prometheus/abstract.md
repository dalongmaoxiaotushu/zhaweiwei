


### Helm 客户端
> Helm 是一个命令行下的客户端工具。主要用于Kubernetes应用程序
> Chart的创建、打包、发布及创建和管理本地和远程的Chart仓库。


### Tiller 服务器
> Tiller 是 Helm 的服务端，部署在Kubernetes集群中，Tiller用于
> 接受Helm的请求，并根据Chart生成Kubernetes的部署文件（Helm称
> 之为 Release）。然后提交给Kubernetes创建应用。


### chart 和 release
>  chart是创建一个应用的信息集合，可以将chart想象成apt、yum中的
>  软件安装包。
>  release是chart的运行实例，代表一个正在运行的应用。



1. 安装Helm
2. 安装Tiller
3. 部署Prometheus Operator
4. 安装Prometheus Operator Deployment
5. 安装Prometheus
6. 安装AlertManager
7. 安装kube-prometheus
> kube-prometheus 是一个Helm Chart 打包了监控Kubernetes需要的所有Exporter和
> Service Monitor，会创建几个Service。每个Exporter会对应一个Service，为Prometheus
> 提供Kubernetes集群的各类监控数据。每个Service对应一个ServiceMonitor，组成
> Prometheus的Target列表。














---
#
#
<meta http-equiv="refresh" content="5">
