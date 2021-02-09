## google 访问 dashboard 证书问题不能弹出token输入页面

### 二进制部署
####  删除默认的secret，用自签证书创建新的secret
```
kubectl delete secret kubernetes-dashboard-certs -n kubernetes-dashboard

kubectl create secret generic kubernetes-dashboard-certs \
--from-file=/opt/kubernetes/ssl/server-key.pem --from-file=/opt/kubernetes/ssl/server.pem -n kubernetes-dashboard

```

####  修改 dashboard.yaml 文件，在args下面增加证书两行
```
args:
       # PLATFORM-SPECIFIC ARGS HERE
       - --auto-generate-certificates
       - --tls-key-file=server-key.pem
       - --tls-cert-file=server.pem

kubectl apply -f kubernetes-dashboard.yaml


```

[参考文章](https://blog.csdn.net/xujiamin0022016/article/details/107676229)



### windows 系统添加受信任的证书
> win+R mmc->文件->添加删除管理单元->证书->添加->我的用户账户->确定->受信任的根证书颁发机构右键->所有任务->导入->server.crt (不行把ca.crt也导入进去，然后关闭所有浏览器页面，重启浏览器)

### 浏览器设置跳过tls验证参数
> 在快捷方式上右键，修改目标为
```
C:\Users\Administrator\AppData\Local\Google\Chrome\Application\chrome.exe --test-type --ignore-certificate-errors
```





















---
#
#
<meta http-equiv="refresh" content="5">