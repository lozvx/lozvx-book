Install Maven

[https://www.vultr.com/docs/how-to-install-apache-maven-on-ubuntu-16-04](https://www.vultr.com/docs/how-to-install-apache-maven-on-ubuntu-16-04)

```
cd /opt/
sudo wget http://www-eu.apache.org/dist/maven/maven-3/3.3.9/binaries/apache-maven-3.3.9-bin.tar.gz
sudo tar -xvzf apache-maven-3.3.9-bin.tar.gz
sudo mv apache-maven-3.3.9 maven
```

Setup environment 

```
sudo vi /etc/profile.d/mavenenv.sh

```

mavenenv.sh

```
export M2_HOME=/opt/maven
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64/jre
export PATH=${M2_HOME}/bin:${PATH}
```



Verify

```
mvn -v
```



