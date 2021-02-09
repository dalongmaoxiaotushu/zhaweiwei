# openssl颁发证书

## 基本概念
1. 双方通信，需要一个中间权威机构帮助双方交换彼此的public.key;不然二者直接交换存在中间串改或者假装对方发送黑客自己的public.key的情况
2. 中间机构称之为CA
3. 通信双方将自身的公钥发送请求给CA，CA用自己的私钥为之加密（签名），生成证书文件（.crt），拷贝回通信双方。通信双方用该证书传递给对方。
4. 通信双方接收到证书以后，可以用CA的公钥对证书进行解密，核对得到的公钥是否和对方发送过来的公钥一致。从而达到安全交换公钥的目的。
5. CA本身也是分等级的，下级CA（subCA）可以向上级CA申请自身的证书，上级证书同样是用自己的私钥加密下级CA的公钥。颁发证书给下级CA。
6. 最顶级的CA（rootCA）给自己颁发证书。
7. windows中可以在Internet选项中添加CA或者是安装CA证书。


[CA 结构图](pic/CA.png)

## openssl 证书申请和颁发
1. 建立RootCA
   1. 生成私钥
   2. 自签名证书
```
 yum install tree
  384  vim /etc/pki/tls/openssl.cnf
  385  cd /etc/pki/CA/
  386  ls
  387  tree
  388  genrsa
  389  man genrsa
  390  openssl genrsa -out private/cakey.pem 4096
  391  cat private/cakey.pem
  392  man req
  393  openssl req -new -x509 -key private/cakey.pem -out cacert.pem -days 3650
  394  ls
  395  cat cacert.pem

  397  openssl x509 -in cacert.pem -noout -text   消除base64编码，查看证书文件
  398  openssl x509 -in cacert.pem -noout -issuer
  399  openssl x509 -in cacert.pem -noout -subject
  400  openssl x509 -in cacert.pem -noout -dates
```

>生成证书时候需要填写的信息，同一机构的不同证书要一致，不然需要在配置文件中修改配置

```
-----
Country Name (2 letter code) [XX]:CN
State or Province Name (full name) []:anhui
Locality Name (eg, city) [Default City]:hefei
Organization Name (eg, company) [Default Company Ltd]:StateGrid
Organizational Unit Name (eg, section) []:xintong
Common Name (eg, your name or your server's hostname) []:kubernetes
Email Address []:

```

2. 用户或服务器
   1. 生成私钥
   2. 生成证书申请文件
   3. 将申请文件发送给CA
```
 mkdir app
  383  cd app
  384  ls

  386  (umask 077;openssl genrsa -out app.key 1024)
  387  cat app.key
  388  openssl req  -new -key app.key -out app.csr
  389  ls
  390  scp app.csr root@192.168.154.130:/etc/pki/CA
  
```

```
-----
Country Name (2 letter code) [XX]:CN
State or Province Name (full name) []:anhui
Locality Name (eg, city) [Default City]:hefei
Organization Name (eg, company) [Default Company Ltd]:StateGrid
Organizational Unit Name (eg, section) []:xintong
Common Name (eg, your name or your server's hostname) []:app
Email Address []:

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:123456
An optional company name []:

```

3. CA颁发证书
```
 openssl ca -in app.csr -out certs/app.cert -days 100
  403  touch index.txt
  404  openssl ca -in app.csr -out certs/app.cert -days 100
  
  406  echo 0F > serial
  407  tree
  
  409  cat serial
  410  openssl ca -in app.csr -out certs/app.cert -days 100
  411  tree
  412  cat certs/app.cert
  413  tree

```

> 颁发证书时候如果没有index文件会报错，创建一个空的index文件即可，openssl会自动写入。
> 还需要一个serial文件来存储下一个要颁发的证书编号，起始号码随便可以设置一个十六进制
> 值。
```
Using configuration from /etc/pki/tls/openssl.cnf
/etc/pki/CA/index.txt: No such file or directory
unable to open '/etc/pki/CA/index.txt'
139632263542688:error:02001002:system library:fopen:No such file or directory:bss_file.c:398:fopen('/etc/pki/CA/index.txt','r')
139632263542688:error:20074002:BIO routines:FILE_CTRL:system lib:bss_file.c:400:
[root@k8s-node1 CA]# touch index.txt
[root@k8s-node1 CA]# openssl ca -in app.csr -out certs/app.cert -days 100
Using configuration from /etc/pki/tls/openssl.cnf
/etc/pki/CA/serial: No such file or directory
error while loading serial number
139694892988320:error:02001002:system library:fopen:No such file or directory:bss_file.c:398:fopen('/etc/pki/CA/serial','r')
139694892988320:error:20074002:BIO routines:FILE_CTRL:system lib:bss_file.c:400:

```
4. 证书发送给客户端
```
scp certs/app.cert root@192.168.154.131:/root/app/
```
5. 应用软件使用证书

## openssl配置文件
```
cat /etc/pki/tls/openssl.cnf
```

```
default_ca      = CA_default            # The default ca section <br/>

---------------------------------------------------------<br/>
[ CA_default ]

dir             = /etc/pki/CA           # Where everything is kept<br/>
certs           = $dir/certs            # Where the issued certs are kept<br/>
crl_dir         = $dir/crl              # Where the issued crl are kept<br/>
database        = $dir/index.txt        # database index file.<br/>
unique_subject = no                    # Set to 'no' to allow creation of<br/>
                                        # several ctificates with same subject.<br/>
new_certs_dir   = $dir/newcerts         # default place for new certs.<br/>

certificate     = $dir/cacert.pem       # The CA certificate<br/>
serial          = $dir/serial           # The current serial <br/>
crlnumber       = $dir/crlnumber        # the current crl number<br/>
                                        # must be commented out to leave a V1 CRL<br/>
crl             = $dir/crl.pem          # The current CRL<br/>
private_key     = $dir/private/cakey.pem # The private key<br/>
RANDFILE        = $dir/private/.rand    # private random number file<br/>

x509_extensions = usr_cert              # The extentions to add to the cert<br/>
```



openssl x509 -outform der -in ca.pem -out ca.crt   pem转换成crt





























---
#
#
<meta http-equiv="refresh" content="5">