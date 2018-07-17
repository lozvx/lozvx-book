# Ubuntu

Install List:

Jdk: open jdk or oracle jdk

[https://www.digitalocean.com/community/tutorials/how-to-install-java-with-apt-get-on-ubuntu-16-04](https://www.digitalocean.com/community/tutorials/how-to-install-java-with-apt-get-on-ubuntu-16-04)

openJDK

```text
sudo apt-get update
sudo apt-get install default-jre
sudo apt-get install default-jdkOracle JDK
```

Oracle JDK

```text
sudo add-apt-repository ppa:webupd8team/java
sudo apt-get update
sudo apt-get install oracle-java8-installer
sudo apt-get install oracle-java9-installer
```

[https://www.digitalocean.com/community/tutorials/how-to-install-java-with-apt-get-on-ubuntu-16-04](https://www.digitalocean.com/community/tutorials/how-to-install-java-with-apt-get-on-ubuntu-16-04)

```bash
# 选择java版本
sudo update-alternatives --config java
```

Git

Maven

IDEA

google pinyin

ss-local

chrome

redis-server

TreeSize

Everywhere



rabbitmq：

docker启动

```text
sudo docker pull rabbitmq:management
sudo docker run -d --name rabbitmq -p 5671:5671 -p 5672:5672 -p 4369:4369 -p 25672:25672 -p 15671:15671 -p 15672:15672 rabbitmq:management

```

ssh：

开启ssh服务

```text
 sudo apt-get install openssh-server
```

