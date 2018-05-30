# Consul

安装：

```bash
brew install consul
```

查看版本

```bash
consul -v
```



配置 Consul（[官方资料](https://www.consul.io/docs/agent/options.html)）：

```text
$ consul agent -dev
$ consul agent -server -bootstrap-expect 1 -data-dir /tmp/consul -ui  -config-dir /etc/consul.d -bind=192.168.1.100
$ consul agent -data-dir /tmp/consul -node=ubuntu64 -bind=10.9.10.176
```

上面三种配置说明：

1. Sever 模式，快捷配置，一般用于调试模式，不建议使用
2. Sever 模式
3. Client 模式

配置参数说明：

* -server：- Serve 模式（不配置为 Client 模式），数量一般为 3-5 个
* -bootstrap-expect： - Server 数量
* -data-dir： - 数据目录
* -ui-dir： - UI目录
* -node： - Node名称
* -bind： - 集群通讯地址

Server 模式后台访问地址：[http://localhost:8500](http://localhost:8500/)

其他命令：

* consul members：查看集群成员
* consul info：查看当前服务器的状况
* consul leave：退出当前服务集群
* ctrl + c：停止服务

启动consul-example

