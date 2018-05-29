# git 设置代理

[https://www.chenyudong.com/archives/use-git-or-github-in-company-local-net.html](https://www.chenyudong.com/archives/use-git-or-github-in-company-local-net.html)

```text
$ git config --global http.proxy 127.0.0.1:1080
```

Windows下通过socks5代理进行ssh连接：

[https://blessing.studio/ssh-over-proxy/](https://blessing.studio/ssh-over-proxy/)

1. 下载connect二进制文件放到C:/Windows中
2. 配置ssh代理

```text
ProxyCommand connect -H 127.0.0.1:1080 %h %p
```

