# Container overview

![](../../.gitbook/assets/ioccontainer.png)

1.Configuration：XML ,Annotation, Java-configuration

2.instantiating a container

```java
ApplicationContext context = new ClassPathXmlApplicationContext("services.xml", "daos.xml");

or

ApplicationContext ctx = new AnnotationConfigApplicationContext("com.desmond.demo*");
```

3.Using the container

```java
// create and configure beans
ApplicationContext context = new ClassPathXmlApplicationContext("services.xml", "daos.xml");

// retrieve configured instance
PetStoreService service = context.getBean("petStore", PetStoreService.class);

// use configured instance
List<String> userList = service.getUsernameList();
```

