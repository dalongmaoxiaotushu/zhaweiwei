


## prometheus 安装
1. ./prometheus --help
> 查看prometheus运行命令帮助
2. ./promtool --help
> 这个工具可以用来对编辑好的yml文件进行语法检测<br/>
> ./promtool check config  ./prometheus.yml

1. target : 被监控目标
2. instance： 被监控目标服务器本身实例
3. job ： 多个实例的一个分组


1. 自己写metrics接口，遵循数据模型
    1. 先知道怎么收集你的监控指标
    2. 集成官方的客户端或者自己的数据格式
2. 使用社区维护的exporter
[exporter 列表](https://prometheus.io/docs/instrumenting/exporters)



## 监控系统服务的运行状态
> systemctl 管理的服务
> ./node_exporter --help  |grep systemd





















---
#
#
<meta http-equiv="refresh" content="5">