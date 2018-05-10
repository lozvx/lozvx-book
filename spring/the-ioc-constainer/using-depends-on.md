# Using Depends-on

The depends-on attribute can explicitly force one or more beans to be initialized before the bean using this element is initialized.

```text
<bean id="beanOne" class="ExampleBean" depends-on="manager"/>
<bean id="manager" class="ManagerBean" />
```

```text
<bean id="beanOne" class="ExampleBean" depends-on="manager,accountDao">
    <property name="manager" ref="manager" />
</bean>

<bean id="manager" class="ManagerBean" />
<bean id="accountDao" class="x.y.jdbc.JdbcAccountDao" />
```

延迟加载

```text
<bean id="lazy" class="com.foo.ExpensiveToCreateBean" lazy-init="true"/>
<bean name="not.lazy" class="com.foo.AnotherBean"/>
```

或者全局设置为延迟加载

```text
<beans default-lazy-init="true">
    <!-- no beans will be pre-instantiated... -->
</beans>
```

