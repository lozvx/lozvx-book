# 跨域访问

{% embed data="{\"url\":\"http://www.ruanyifeng.com/blog/2016/04/same-origin-policy.html\",\"type\":\"link\",\"title\":\"浏览器同源政策及其规避方法 - 阮一峰的网络日志\",\"icon\":{\"type\":\"icon\",\"url\":\"http://www.ruanyifeng.com/favicon.ico\",\"aspectRatio\":0}}" %}

{% embed data="{\"url\":\"http://www.ruanyifeng.com/blog/2016/04/cors.html\",\"type\":\"link\",\"title\":\"跨域资源共享 CORS 详解 - 阮一峰的网络日志\",\"icon\":{\"type\":\"icon\",\"url\":\"http://www.ruanyifeng.com/favicon.ico\",\"aspectRatio\":0}}" %}



Nginx CORS 支持

```text
    server {
        listen       80;
        server_name  local;

        location / {

    # CORS支持
    set $cors "local";
    if ( $http_origin  ~* \.test\.com) {
        set $cors "allow";
    }
    if ($request_method = "OPTIONS") {
        set $cors "${cors}options";
    }

   # options请求不转给后端，直接返回204
   if ($cors = "allowoptions") {
        add_header Access-Control-Allow-Origin $http_origin;
        add_header Access-Control-Allow-Methods GET,PUT,POST,OPTIONS;
        add_header Access-Control-Allow-Credentials true;
        add_header Access-Control-Allow-Headers *;
        add_header Access-Control-Max-Age 1728000;
        return 204;
   }

   # 第二个if会导致上面的add_header无效，这是nginx的问题，这里直接重复执行下
   if ($cors = "allow") {
        add_header Access-Control-Allow-Origin $http_origin;
        add_header Access-Control-Allow-Credentials true;
   } 
       # CORS支持 End

        proxy_pass http://local;
            #proxy_redirect http:// https://;
        }
    }
```

