* Install packages to allow `apt` to use a repository over HTTPS:

```
sudo apt-get install apt-transport-https ca-certificates curl software-properties-common
```

* Add Docker’s official GPG key:

```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

# Verify
sudo apt-key fingerprint 0EBFCD88
```

* Add Repo

```
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com linux/ubuntu $(lsb_release -cs) stable"
```



* 更新源

```
sudo apt-get update
```

* 安装

```
 apt-get install docker-ce
```

* 启动

```
sudo service docker start
```

* 测试

```
sudo docker run hello-world
```



