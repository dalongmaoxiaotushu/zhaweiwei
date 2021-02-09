 ### nginx 配置反向代理
> nginx 安装在192.168.154.130:80主机上<br/>
> tomcat 安装在192.168.154.131:8080主机上<br/>
> 通过访问nginx的地址代理到tomcat<br/>
> 反向代理是指client端不需要在浏览器上做任何配置，nginx代理服务器屏蔽了后面的
> web服务端真正的地址。从外面看，nginx和后面的web服务端是一体的。


```
  418 wget  http://downloads.sourceforge.net/project/pcre/pcre/8.37/pcre-8.37.tar.gz
  419  mkdir nginx
  420  tar -xzvf pcre-8.37.tar.gz -C ./nginx      #安装pcre  正则
  421  ls
  422  cd nginx
  425  cd pcre-8.37/  
  427  ./configure  出错，缺少C++编译器

  434  yum install -y gcc gcc-c++    安装c++编译器
  
  436  ./configure              # 编译pcre并安装
  437  make && make install     

  439  yum -y install make zlib zlib-devel gcc-c++ libtool openssl openssl-devel                  #安装nginx依赖的软件
 
  443  wget http://nginx.org/download/nginx-1.18.0.tar.gz
  445  tar -xzvf nginx-1.18.0.tar.gz
  447  cd nginx-1.18.0/                                     #安装nginx
  449  ./configure
  450  make && make install


 
  454  cd /usr/local         #运行nginx 开放端口
  456  cd nginx
  459  cd sbin
  461  ./nginx                        
  462  ps -ef |grep nginx
  463  netstat -ntlp |grep nginx
  464  man firewall-cmd
  465  firewall-cmd --permanent --add-port=80/tcp

  
 
  471  ./nginx -?    #查看nginx帮助
  472  ./nginx -h
 
  
 
  
  485  java -version   #安装tomcat
  488  wget https://mirrors.tuna.tsinghua.edu.cn/apache/tomcat/  tomcat-8/v8.5.61/bin/apache-tomcat-8.5.61.tar.gz
  490  tar -xzvf apache-tomcat-8.5.61.tar.gz

  
  493  cd apache-tomcat-8.5.61/         #启动tomcat
  495  cd bin
  497  ./startup.sh
  498  netstat -ntlp|grep java


  504  cd webapps           #修改tomcat主页
  505  ls
  506  cd ROOT/
  507  ls
  508  cat index.jsp
  509  vi index.jsp
  510  
  519  ./shutdown.sh
  520  ./startup.sh

  526  scp apache-tomcat-8.5.61.tar.gz root@192.168.154.131:/root/nginx
  527  ls
  528  cd /usr/local/nginx/
  530  cd conf

  533  vi nginx.conf
  534  ./nginx -s reload
```

### nginx.conf 配置
```
server{
        listen       80;
        server_name  192.168.154.130;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location / {
            root   html;
            proxy_pass  http://192.168.154.131:8080;
            index  index.html index.htm;
        }
}
```

### nginx 配置反向代理2 根据路径分发
> 在192.168.154.131的tomcat的webapps目录下创建edu/a.html  写入"edu.tomcat"
> 在192.168.154.131的tomcat的webapps目录下创建edu/a.html  写入"vod.tomcat"
```
server {
       listen 9001;
       server_name  192.168.154.130;

       location ~ /edu/ {
           root html;
           proxy_pass http://192.168.154.131:8080;
           index   index.html index.htm;
       }

       location ~ /vod/ {
           root html;
           proxy_pass http://192.168.154.130:8080;
           index   index.html index.htm;
       }
    }

```


---
#
#
<meta http-equiv="refresh" content="5">



