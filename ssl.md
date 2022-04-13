openssl 生成nginx 证书

这里说下Linux 系统怎么通过openssl命令生成 证书。
首先执行如下命令生成一个key
openssl genrsa -des3 -out ssl.key 1024
然后他会要求你输入这个key文件的密码。不推荐输入。因为以后要给nginx使用。每次reload nginx配置时候都要你验证这个PAM密码的。
由于生成时候必须输入密码。你可以输入后 再删掉。
mv ssl.key xxx.key

openssl rsa -in xxx.key -out ssl.key

rm xxx.key
然后根据这个key文件生成证书请求文件
openssl req -new -key ssl.key -out ssl.csr
以上命令生成时候要填很多东西 一个个看着写吧（可以随便，毕竟这是自己生成的证书）最后根据这2个文件生成crt证书文件
openssl x509 -req -days 3650 -in ssl.csr -signkey ssl.key -out ssl.crt
这里365是证书有效期 推荐3650哈哈。这个大家随意。最后使用到的文件是key和crt文件。如果需要用pfx 可以用以下命令生成
openssl pkcs12 -export -inkey ssl.key -in ssl.crt -out ssl.pfx
在需要使用证书的nginx配置文件的server节点里加入以下配置就可以了。
ssl on;
ssl_certificate /home/ssl.crt;
ssl_certificate_key /home/ssl.key;
ssl_session_timeout 5m;
ssl_protocols SSLv2 SSLv3 TLSv1;
ssl_ciphers ALL:!ADH:!EXPORT56:RC4+RSA:+HIGH:+MEDIUM:+LOW:+SSLv2:+EXP;
ssl_prefer_server_ciphers on;
然后重启nginx就大功告成了
最重要的是，访问是https进行访问，
server{
    listen 443;
    ssl on;
    ssl_certificate /usr/local/webserver/nginx/conf/vhost/ssl/server.crt;
    ssl_certificate_key /usr/local/webserver/nginx/conf/vhost/ssl/server.key;
}
端口一定是443端口
