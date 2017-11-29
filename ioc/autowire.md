Advantages：

* Autowiring can significantly reduce the need to specify properties or constructor arguments. \(Other mechanisms such as a bean template [discussed elsewhere in this chapter](https://docs.spring.io/spring/docs/5.0.2.RELEASE/spring-framework-reference/core.html#beans-child-bean-definitions) are also valuable in this regard.\)

  减少指定属性或者构造函数参数的需要

* Autowiring can update a configuration as your objects evolve. For example, if you need to add a dependency to a class, that dependency can be satisfied automatically without you needing to modify the configuration. Thus autowiring can be especially useful during development, without negating the option of switching to explicit wiring when the code base becomes more stable.

    如果后续需要增加依赖，不需要修改配置文件

Disadvantages：

* Explicit dependencies in `property` and `constructor-arg` settings always override autowiring. You cannot autowire so-called _simple_ properties such as primitives, `Strings`, and `Classes` \(and arrays of such simple properties\). This limitation is by-design.

 属性或构造函数中的显式依赖会覆盖自动装配。

* Autowiring is less exact than explicit wiring. Although, as noted in the above table, Spring is careful to avoid guessing in case of ambiguity that might have unexpected results, the relationships between your Spring-managed objects are no longer documented explicitly.

* Wiring information may not be available to tools that may generate documentation from a Spring container.

    织入的信息可能不能生成文档。

* Multiple bean definitions within the container may match the type specified by the setter method or constructor argument to be autowired. For arrays, collections, or Maps, this is not necessarily a problem. However for dependencies that expect a single value, this ambiguity is not arbitrarily resolved. If no unique bean definition is available, an exception is thrown.



