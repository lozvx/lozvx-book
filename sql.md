### 服务端

```

```

or

```
sudo ssserver -c /etc/ss_conf.json -d start
```

### 客户端

```

{
    "server":"server ip",
    "server_port":666,
    "local_address": "127.0.0.1",
    "local_port":1080,
    "password":"password",
    "timeout":300,
    "method":"aes-256-cfb"
}

```

  
配置全局代理PAC

network-&gt;network proxy-&gt;Configuration URL

file:////home/ss/autoproxy.pac



