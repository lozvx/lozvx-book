# Bean scopes

Bean scopes:

| Scope | Description |
| :--- | :--- |
| [singleton](https://docs.spring.io/spring/docs/5.0.2.RELEASE/spring-framework-reference/core.html#beans-factory-scopes-singleton) | \(Default\) Scopes a single bean definition to a single object instance per Spring IoC container. |
| [prototype](https://docs.spring.io/spring/docs/5.0.2.RELEASE/spring-framework-reference/core.html#beans-factory-scopes-prototype) | Scopes a single bean definition to any number of object instances. |
| [request](https://docs.spring.io/spring/docs/5.0.2.RELEASE/spring-framework-reference/core.html#beans-factory-scopes-request) | Scopes a single bean definition to the lifecycle of a single HTTP request; that is, each HTTP request has its own instance of a bean created off the back of a single bean definition. Only valid in the context of a web-aware Spring `ApplicationContext`. |
| [session](https://docs.spring.io/spring/docs/5.0.2.RELEASE/spring-framework-reference/core.html#beans-factory-scopes-session) | Scopes a single bean definition to the lifecycle of an HTTP `Session`. Only valid in the context of a web-aware Spring `ApplicationContext`. |
| [application](https://docs.spring.io/spring/docs/5.0.2.RELEASE/spring-framework-reference/core.html#beans-factory-scopes-application) | Scopes a single bean definition to the lifecycle of a `ServletContext`. Only valid in the context of a web-aware Spring `ApplicationContext`. |
| [websocket](https://docs.spring.io/spring/docs/5.0.2.RELEASE/spring-framework-reference/web.html#websocket-stomp-websocket-scope) | Scopes a single bean definition to the lifecycle of a `WebSocket`. Only valid in the context of a web-aware Spring `ApplicationContext`. |

* singleton

```text
<bean id="accountService" class="com.foo.DefaultAccountService"/>

<!-- the following is equivalent, though redundant (singleton scope is the default) -->
<bean id="accountService" class="com.foo.DefaultAccountService" scope="singleton"/>
```

* prototype

```text
<bean id="accountService" class="com.foo.DefaultAccountService" scope="prototype"/>
```

* Request, session, application, and WebSocket scopes

The request, session, application, and websocket scopes are only available if you use a web-aware Spring ApplicationContext implementation \(such as XmlWebApplicationContext\). If you use these scopes with regular Spring IoC containers such as the ClassPathXmlApplicationContext, an IllegalStateException will be thrown complaining about an unknown bean scope.

这几个Bean scope只有在web的spring项目中。

* request

```text
<bean id="loginAction" class="com.foo.LoginAction" scope="request"/>
```

or

```text
@RequestScope
@Component
public class LoginAction {
    // ...
}
```

* session

```text
<bean id="userPreferences" class="com.foo.UserPreferences" scope="session"/>
```

or

```text
@SessionScope
@Component
public class UserPreferences {
    // ...
}
```

* application

```text
<bean id="appPreferences" class="com.foo.AppPreferences" scope="application"/>
```

or

```text
@ApplicationScope
@Component
public class AppPreferences {
    // ...
}
```

