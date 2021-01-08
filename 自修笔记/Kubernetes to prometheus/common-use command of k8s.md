## 集群管理
1. kubeadm reset 可以撤销kube init的操作，也可以移除node节点
2. kubedam create token --print-join-command   创建加入节点的新token并打出加入命令

## deploy app
1. kubectl get nodes

2. kubectl create deployment kubernetes-bootcamp --image=gcr.io/google-samples/kubernetes-bootcamp:v1

3. kubectl get deployments

4. kubectl proxy
> Starting Proxy. After starting it will not output a response. Please click the first Terminal Tab\n"; 
>The kubectl command can create a proxy that will forward communications into the cluster-wide, private network. The proxy can be terminated by pressing control-C and won't show any output while its running.

We will open a second terminal window to run the proxy.

5. kubectl logs $POD_NAME

6. kubectl exec $POD_NAME env
> We can execute commands directly on the container once the Pod is up and running. For this, we use the exec command and use the name of the Pod as a parameter. Let’s list the environment variables:

7. kubectl exec -ti $POD_NAME bash
> Next let’s start a bash session in the Pod’s container <br/>
> cat server.js <br/>
> curl localhost:8080


## service expose app
1. kubectl get services

2. kubectl expose deployment kubernetes-bootcamp --type="NodePort" --port 8080


3. kubectl describe svc kubernetes-bootcamp

4. kubectl describe deployment 

5. kubectl get pods -l run=kubernetes-bootcamp

6. kubectl get services -l run=kubernetes-bootcamp

7. kubectl label pod $POD_NAME app=v1
> kubectl describe pods $POD_NAME <br/>
> kubectl get pods -l app=v1 <br/>
> Services match a set of Pods using labels and selectors, a grouping primitive that allows logical operation on objects in Kubernetes. Labels are key/value pairs attached to objects and can be used in any number of ways:

Designate objects for development, test, and production
Embed version tags
Classify an object using tags






## scale app
1. kubectl get deployments

2. kubectl get rs     To see the ReplicaSet created by the Deployment

3. kubectl scale deployment kubernetes-bootcamp --replicas=4

4. kubectl get pods -o wide

5. kubectl describe deployments kubernetes-bootcamp 

6. kubectl describe services/kubernetes-bootcamp

7. kubectl get svc



## update and rollback

1. kubectl set image deployment kubernetes-bootcamp kubernetes-bootcamp=jocatalin/kubernetes-bootcamp:v2


2. kubectl rollout status deployment kubernetes-bootcamp


3. kubectl rollout undo deployment kubernetes-bootcamp






# 部署应用过程

## 编译应用
1. mvn package -pl system
2. mvn package -pl inventory   

> 用maven编译 system 和inventory两个微服务



## 运行应用
1. kubectl apply -f kubernetes.yaml
2. kubectl delete -f kubernetes.yaml  上述命令的逆向


### 查看某个资源的字段
1. kubectl explain service --recursive
### 查看具体字段的解释
1. kubectl explain svc.spec.ports


## 导出当前运行pod的yaml
kubectl get pods kube-flannel-ds-amd64-2jtkv -n kube-system  -o yaml > pods.yaml

## 查看k8s api_server 对外的ip地址和端口号，用于prometheus监控
1. kubectl get ep 










---
#
#
<meta http-equiv="refresh" content="5">