# MySQL

{% embed url="https://github.com/threerocks/studyFiles/tree/master/%E6%95%B0%E6%8D%AE%E5%BA%93" %}

{% embed url="https://blog.csdn.net/wyl9527/article/details/79111467" %}

推荐： [高性能MySQL 第3版 中文 .pdf](https://github.com/threerocks/studyFiles/blob/master/%E6%95%B0%E6%8D%AE%E5%BA%93/%E9%AB%98%E6%80%A7%E8%83%BDMySQL%20%E7%AC%AC3%E7%89%88%20%E4%B8%AD%E6%96%87%20.pdf)

Docker

[https://store.docker.com/images/mysql/](https://store.docker.com/images/mysql/)

```text
#未挂载volume
sudo docker run --name mysql -e MYSQL_ROOT_PASSWORD=password -d mysql:5.7

#挂载volume，持久化数据
sudo docker run --name mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=password -d -v /home/lozvx/mysqldata:/var/lib/mysql mysql:5.7
```

连接：

```text
sudo docker exec -it mysql mysql -uroot -ppassword

# 进入之后，要对用户进行授权，否则用navicat连接不上。
GRANT ALL PRIVILEGES ON *.* TO 'root'@'localhost' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON *.* TO 'root'@'172.21.40.209' IDENTIFIED BY 'password';
flush privileges;
```

建库:

```text
create database lozvx;
```

客户端

 [DBeaver](https://dbeaver.io/)



