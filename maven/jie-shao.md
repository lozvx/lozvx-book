# clean: 清理输出目录 target

compile： 编译项目主代码

# mvn clean install -DskipTests

mvn dependency:tree

scope为依赖范围，若依赖范围为test，则表示该依赖只对测试有效

```
<dependency>
<groupId>junit</groupId>
<artifactId>junit</artifactId>
<version>4.12</version>
<scope>test</scope>
</dependency>
```

由于历史原因，maven核心插件compiler默认只支持编译java1.3，因此需要配置更高java版本

```Xml
<plugin>
<groupId>org.apache.maven.plugins</groupId>
<artifactId>maven-compiler-plugin</artifactId>
<version>3.5.1</version>
<configuration>
<source>1.8</source>
<target>1.8</target>
</configuration>
</plugin>
```



```
生成可执行的jar文件
```

```
<plugin>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-maven-plugin</artifactId>
<version>1.3.3.RELEASE</version>
<configuration>
<mainClass>${start-class}</mainClass>
</configuration>
</plugin>

maven坐标： groupId,artifactId,version,packaging,classifier


maven是的插件机制：


编译的插件 
maven
-compiler-plugin


测试的插件 
maven
-surefire-plugin 支持Junit3，Junit4和testNG




maven
 的三套生命周期


http://blog.csdn.net/crave_shy/article/details/40919169



```

```
下载源代码 mvn dependency:sources
```

```
mvn dependency:sources 
```

dependency management

jetty-

maven

-plugin可以发现编译后的文件变化后，自动将其更新到jetty容器。

mvn

dependency:copy-dependencies

