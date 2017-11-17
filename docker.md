Reference:[ https://zouyapeng.gitbooks.io/docker/content/DockerInstallation/ubuntu\_16\_04.html](https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/)

* 更新源

```
sudo apt-get update 
```

* Install packages to allow `apt` to use a repository over HTTPS:

```
sudo apt-get install \
 spt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common
```

* 安装

```
sudo apt-get install docker-engine
```

* 启动服务

```
sudo service docker start
```

* 测试

```
sudo docker run hello-world
```



