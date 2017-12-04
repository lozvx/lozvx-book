Aspject：

开启@AspectJ，需要引入aspectjweaver.jar并且开启配置。

注解

```
@Configuration
@EnableAspectJAutoProxy
public class AppConfig {

}
```

或XML

```
<aop:aspectj-autoproxy/>
```



