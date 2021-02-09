## oracle启动
[oracel 开机自启动服务配置](https://www.cnblogs.com/wucongzhou/p/12612951.html)


## shm空间不够
```
mount -t tmpfs shmfs -o size=12288M /dev/shm

# 已经挂在过的使用

mount -o remount,size=12288M /dev/shm

# 为了确保重启后生效，需要修改 /etc/fstab

tmpfs /dev/shm tmpfs  defaults,size=12288M      0 0

# 修改成功后在SQL中查看

SQL> show parameter target;

```


## 启动数据库
[启动数据库方式](https://jingyan.baidu.com/article/5552ef47c73eef518ffbc908.html)


## 解决数据库sqlplus删除键乱码

```
       yum install readline*
   
   17  wget https://github.com/hanslub42/rlwrap/releases/download/7c1e432/rlwrap-0.44.tar.gz

   23  tar -zxvf rlwrap-0.44.tar.gz -C ./
   24  ls
   25  cd rlwrap-0.44
   26  ls
   27  ./configure
   28  make && make install
   29  rlwrap
   30  su - oracle
       vi .bash_profile
          alias sqlplus='rlwrap sqlplus'
          alias rman='rlwrap rman'
       source .bash_profile

```

## 安装help
[SQL当中的help](https://www.xuebuyuan.com/1858667.html)

## 常用命令


```

sqlplus /nolog

start

conn / as sysdba

conn system/123456 

conn system/123456@orcl

alter user scott account unlock

help index



```





---
#
#
<meta http-equiv="refresh" content="5">