# Docker

* s udo add-apt-repository "deb \[arch=amd64\] [https://download.docker.com/linux/ubuntu](https://download.docker.com/linux/ubuntu) $\(lsb\_release -cs\) stable"1sudo add-apt-repository "deb \[arch=amd64\] [https://download.docker.com/linux/ubuntu](https://download.docker.com/linux/ubuntu) $\(lsb\_release -cs\) stable"Install packages to allow `apt` to use a repository over HTTPS:

```text
sudo apt-get install apt-transport-https ca-certificates curl software-properties-common
```

* Add Docker’s official GPG key:

```text
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

# Verify
sudo apt-key fingerprint 0EBFCD88
```

* Add Repo

```text
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"更新源
```

```text
sudo apt-get update
```

* 安装

```text
sudo apt-get install docker-ce
```

* 启动

```text
sudo service docker start
```

* 测试

```text
sudo docker run hello-world
```

