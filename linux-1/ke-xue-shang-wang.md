# 科学上网

## 服务端

```text
docker pull oddrationale/docker-shadowsocks
docker run -d -p 666:666 oddrationale/docker-shadowsocks -s 0.0.0.0 -p 666 -k $SSPASSWORD -m aes-256-cfb
```

or

```text
sudo ssserver -c /etc/ss_conf.json -d start
```

## 客户端

```text
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

