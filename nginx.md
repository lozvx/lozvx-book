# nginx

 参考：[https://www.zybuluo.com/phper/note/89391](https://www.zybuluo.com/phper/note/89391)

nginx 命令：

启动： nginx

查看进程：ps -ef \| grep nginx

停止：kill进程

平滑重启：nginx -s reload

配置文件：/user/local/etc/nginx/nginx.conf



参考：

[https://www.liaoxuefeng.com/article/0014189023237367e8d42829de24b6eaf893ca47df4fb5e000](https://www.liaoxuefeng.com/article/0014189023237367e8d42829de24b6eaf893ca47df4fb5e000)

#### 给Nginx配置一个自签名的SSL证书

{% embed data="{\"url\":\"https://github.com/michaelliao/itranswarp.js/blob/master/conf/ssl/gencert.sh\",\"type\":\"link\",\"title\":\"michaelliao/itranswarp.js\",\"description\":\"itranswarp.js - Full-featured CMS including blog, wiki, discussion, etc. powered by Nodejs.\",\"icon\":{\"type\":\"icon\",\"url\":\"https://github.com/fluidicon.png\",\"aspectRatio\":0},\"thumbnail\":{\"type\":\"thumbnail\",\"url\":\"https://avatars0.githubusercontent.com/u/470058?s=400&v=4\",\"width\":400,\"height\":400,\"aspectRatio\":1}}" %}

```bash

#!/bin/sh# create self-signed server certificate:read -p "Enter your domain [www.example.com]: " DOMAINecho "Create server key..."openssl genrsa -des3 -out $DOMAIN.key 1024echo "Create server certificate signing request..."SUBJECT="/C=US/ST=Mars/L=iTranswarp/O=iTranswarp/OU=iTranswarp/CN=$DOMAIN"openssl req -new -subj $SUBJECT -key $DOMAIN.key -out $DOMAIN.csrecho "Remove password..."mv $DOMAIN.key $DOMAIN.origin.keyopenssl rsa -in $DOMAIN.origin.key -out $DOMAIN.keyecho "Sign SSL certificate..."openssl x509 -req -days 3650 -in $DOMAIN.csr -signkey $DOMAIN.key -out $DOMAIN.crtecho "TODO:"echo "Copy $DOMAIN.crt to /etc/nginx/ssl/$DOMAIN.crt"echo "Copy $DOMAIN.key to /etc/nginx/ssl/$DOMAIN.key"echo "Add configuration in nginx:"echo "server {"echo "    ..."echo "    listen 443 ssl;"echo "    ssl_certificate     /etc/nginx/ssl/$DOMAIN.crt;"echo "    ssl_certificate_key /etc/nginx/ssl/$DOMAIN.key;"echo "}"
```

{% code-tabs %}
{% code-tabs-item title="Sample" %}
```bash

#user  nobody;worker_processes  1;#error_log  logs/error.log;#error_log  logs/error.log  notice;#error_log  logs/error.log  info;#pid        logs/nginx.pid;events {    worker_connections  1024;}http {    include       mime.types;    default_type  application/octet-stream;    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '    #                  '$status $body_bytes_sent "$http_referer" '    #                  '"$http_user_agent" "$http_x_forwarded_for"';    #access_log  logs/access.log  main;    sendfile        on;    #tcp_nopush     on;    #keepalive_timeout  0;    keepalive_timeout  65;    #gzip  on;    upstream pttrade_local {        server 127.0.0.1:8887  weight=10 max_fails=2 fail_timeout=30s;    }     server {        listen       8080;        server_name  localhost;        #charset koi8-r;        #access_log  logs/host.access.log  main;        location / {            root   html;            index  index.html index.htm;        }        #error_page  404              /404.html;        # redirect server error pages to the static page /50x.html        #        error_page   500 502 503 504  /50x.html;        location = /50x.html {            root   html;        }        # proxy the PHP scripts to Apache listening on 127.0.0.1:80        #        #location ~ \.php$ {        #    proxy_pass   http://127.0.0.1;        #}        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000        #        #location ~ \.php$ {        #    root           html;        #    fastcgi_pass   127.0.0.1:9000;        #    fastcgi_index  index.php;        #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;        #    include        fastcgi_params;        #}        # deny access to .htaccess files, if Apache's document root        # concurs with nginx's one        #        #location ~ /\.ht {        #    deny  all;        #}    }    # another virtual host using mix of IP-, name-, and port-based configuration    #    server {        listen       80;        server_name  trade;        location / {            proxy_pass http://pttrade_local;        }    }    # HTTPS server    #    server {        listen       443 ssl;        server_name  trade_https;        ssl_certificate     ssl/www.pttrade.jd.com.crt;        ssl_certificate_key ssl/www.pttrade.jd.com.key;        ssl_session_cache    shared:SSL:1m;        ssl_session_timeout  5m;        ssl_ciphers  HIGH:!aNULL:!MD5;        ssl_prefer_server_ciphers  on;        location / {            proxy_pass http://pttrade_local;        }    }    include servers/*;}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

