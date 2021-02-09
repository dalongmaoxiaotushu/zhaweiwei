
### vi 串替换
> :12,22s/abc/123/g  将12行到22行的字符串’abc‘替换成’123‘ >
> （s代表字符串的意思），不加g只替换每行的一个要替换的字符串，后面的不会替换）

### sed 串替换
> sed -ri '1,10 s/134/136/g' etcd.conf   














---
#
#
<meta http-equiv="refresh" content="5">