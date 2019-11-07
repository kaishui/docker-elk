# 1. ubuntu configuration setting
 open files & max vm area

# 2. setting secure 

```
cd $ELASTICSEARCH_PATH
bin/elasticsearch

```

## 2.1 add security setting
```
xpack.security.audit.enabled: true
xpack.license.self_generated.type: trial
xpack.security.enabled: true
xpack.monitoring.collection.enabled: true

```

## 2.2 set password
```
#manual setting
bin/elasticsearch-setup-passwords interactive 

password: changeme

# auto setting
bin/elasticsearch-setup-passwords auto

```
## 2.3 证书转换
p12生成证书 
openssl pkcs12 -clcerts -nokeys -out apple_cert.pem -in apple_cert.p12

p12导出key，这时需要输入p12的导出密码 
openssl pkcs12 -nocerts -nodes -out apple.key -in apple_cert.p12

利用key导出公私钥（存在rsa和ecc算法两种）：

导出rsa公私钥 
openssl rsa -in apple.key -out apple_rsa_pri.pem 
openssl rsa -in apple.key -pubout -out apple_rsa_pub.pem
导出ecc公私钥 
openssl ec -in apple.key -out apple_ec_pri.pem 
openssl ec -in apple.key -pubout -out apple_ec_pub.pem
一般服务端只需要证书和私钥，所以拼接即可： 
cat apple_cert.pem apple_rsa_pri.pem >magic-dev.pem 
或者 
cat apple_cert.pem apple_ec_pri.pem >magic-dev.pem

原文链接：https://blog.csdn.net/ququ_123/article/details/79863800