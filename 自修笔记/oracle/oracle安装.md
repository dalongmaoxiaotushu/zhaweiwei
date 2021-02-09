# red hat 6.5 安装 oracle11g

## 解决red hat yum 源未注册问题
[替换yum源](https://blog.csdn.net/allthebetter/article/details/105561487)
> 其中由于redhat6.5已经停止维护，所以想要下载的话要转到网址vault.centos.org对应目录去下载<br/>
> 修改后，当替换repo文件的时候，要编辑其中的url都替换成vault.centos.org<br/>
> 在vim最后行模式中，:1,$s/\/centos/     <br/>
> :1,$s/mirror.centos.org/vault.centos.org <br/>
> :1,$s/mirror.163.org/vault.centos.org   <br/>
> 执行替换yum源后，任然会显示未注册，但是这时候可以用yum安装依赖了。

## [安装oracle 11g](https://www.cnblogs.com/eastonliu/p/10019954.html)
## 环境变量配置是先赋值，后export
```
export PATH
export ORACLE_BASE=/u01/oracle/;
export ORACLE_HOME=$ORACLE_BASE/product/;
export ORACLE_SID=orcl
ORACLE_TERM=xterm; export ORACLE_TERM
PATH=/usr/sbin:$PATH; export PATH
PATH=$ORACLE_HOME/bin:$PATH; export PATH
LD_LIBRARY_PATH=$ORACLE_HOME/lib:/lib:/usr/lib;export LD_LIBRARY_PATH
CLASSPATH=$ORACLE_HOME/JRE:$ORACLE_HOME/jlib:$ORACLE_HOME/rdbms/jlib; export CLASSPATH
NLS_LANG=AMERICAN_AMERICA.AL32UTF8; export NLS_LANG
umask 022

```
## 使用图形界面前一定要在root状态下xhost + 再切换到oracle用户运行图形界面程序。

## 如果还不行，就需要在oracle用户和root用户中都 export 本机ip:0.0    注意本机ip是指宿主机ip并不是虚拟机ip






---
#
#
<meta http-equiv="refresh" content="5">