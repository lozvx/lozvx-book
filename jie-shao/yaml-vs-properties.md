# yaml vs properties

{% embed data="{\"url\":\"https://meetsnehal.wordpress.com/2015/09/12/yaml-an-alternative-to-properties-file-with-spring-boot/\",\"type\":\"link\",\"title\":\"YAML an alternative to Properties file … with Spring Boot\",\"description\":\"From many years Java developers relies on properties file or xml files for specifying application configurations. In an enterprise application peoples have  creating separate files for each environ…\",\"icon\":{\"type\":\"icon\",\"url\":\"https://s1.wp.com/i/favicon.ico\",\"aspectRatio\":0},\"thumbnail\":{\"type\":\"thumbnail\",\"url\":\"https://meetsnehal.files.wordpress.com/2015/09/source-code-583537\_640.jpg\",\"width\":640,\"height\":362,\"aspectRatio\":0.565625}}" %}



From many years Java developers relies on properties file or xml files for specifying application configurations. In an enterprise application peoples have  creating separate files for each environment like Development, Staging and Production to define the properties for respective environment .But Spring boot gives us an option to configure all profiles within a single “yml” file.

**What is YAML ?**

[YAML](https://wordpress.com/post/84008038/58/redir.aspx?C=iIVoi758YUam3BoU91hAwS5KRK38t9JIG_sSS9Z9so3j2B5_qM_kbAAt1AwGIWBDvAXGWaEX0IQ.&URL=http%3a%2f%2fyaml.org%2f) is a superset of JSON, and as such is a very convenient format for specifying hierarchical configuration data.

From [YAML site](http://yaml.org/YAML) : It is a human friendly data serialization standard for all programming languages.

YAML is more readable and it is good for the developers for read/write configuration files.

**Design goals for YAML** :

From [YAML offcial specification](http://www.yaml.org/spec/1.2/spec.html) :

1. YAML is easily readable by humans.
2. YAML data is portable between programming languages.
3. YAML matches the [native data structures](https://wordpress.com/post/84008038/58/redir.aspx?C=iIVoi758YUam3BoU91hAwS5KRK38t9JIG_sSS9Z9so3j2B5_qM_kbAAt1AwGIWBDvAXGWaEX0IQ.&URL=http%3a%2f%2fwww.yaml.org%2fspec%2f1.2%2fspec.html%23native+data+structure%2f%2f) of agile languages.
4. YAML has a consistent model to support generic tools.
5. YAML supports one-pass processing.
6. YAML is expressive and extensible.
7. YAML is easy to implement and use.

**For what should I consider  it?**

Even if your YAML file is incomplete there is no way to detect it , but XML parser always check for well formed document.

**Dont consider :**

YAML files dont consider for good serialization rather think for JSON because it is based on the objects.

So , _What about XML ?_ 

Xml is mostly prefer for machine to machine communication.

YAML document example:

```text
environment:
    profiles: dev
    name: Developer App 
    url: http://dev.abc.com
    
    profiles: qa
    name: QA App 
    url: http://qa.abc.com

```

**Which  Java YAML parsers available ?**

There are following YAML parsers available for Java,

1. [SnakeYAML](https://bitbucket.org/asomov/snakeyaml)
2. [JYaml](http://jyaml.sourceforge.net/)
3. [YamlBeans](http://yamlbeans.sourceforge.net/)
4. [JvYaml](https://www.openhub.net/p/jvyaml)

Spring Boot uses SnakeYAML library for yaml support.

**SnakeYAML**

Snakeyaml is a YAML parser and emitter for the Java Virtual Machine.

Official site: [https://bitbucket.org/asomov/snakeyaml](https://bitbucket.org/asomov/snakeyaml)

**SnakeYAML features:**

* a complete [YAML 1.1 parser](https://wordpress.com/post/84008038/58/redir.aspx?C=iIVoi758YUam3BoU91hAwS5KRK38t9JIG_sSS9Z9so3j2B5_qM_kbAAt1AwGIWBDvAXGWaEX0IQ.&URL=http%3a%2f%2fyaml.org%2fspec%2f1.1%2fcurrent.html). In particular, SnakeYAML can parse all examples from the specification.
* Unicode support including UTF-8/UTF-16 input/output.
* high-level API for serializing and deserializing native Java objects.
* support for all types from the [YAML types repository](https://wordpress.com/post/84008038/58/redir.aspx?C=iIVoi758YUam3BoU91hAwS5KRK38t9JIG_sSS9Z9so3j2B5_qM_kbAAt1AwGIWBDvAXGWaEX0IQ.&URL=http%3a%2f%2fyaml.org%2ftype%2findex.html).
* relatively sensible error messages.

So ….

Its all about YAML, but what about its support in **Spring Boot** framework ?

Yes, If **Snakeyaml** library is included in your classpath, then **SpringApplication**class will automatically supports the YAML as an alternative to properties file.

> if you use starter pom then **spring-boot-starter** automaticaly loads yml file \(application.yml\)

You can check [spring-boot-starter](https://github.com/spring-projects/spring-boot/blob/master/spring-boot-starters/spring-boot-starter/pom.xml) pom here.

**How it is loaded in Spring Boot ?**

The **YamlPropertiesFactoryBean** will load YAML as Properties and the  **YamlMapFactoryBean** will load YAML as a Map.

Read more on : [Spring Boot Doc](http://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-external-config.html#boot-features-external-config-yaml)

**How to use YamlPropertiesFactoryBean to load YAML files using Spring Framework ?**

From Spring framework 4.1.0 has added support for YAML , Spring framework 4.1.0 maven POM has [Snakeyaml dependency](http://central.maven.org/maven2/org/springframework/spring-beans/4.1.0.RELEASE/spring-beans-4.1.0.RELEASE.pom) .  
You can load YAML using two ways in spring application,

1. Using Java Configuration class

| 12345678 | `@Bean  public` `static` `PropertySourcesPlaceholderConfigurer properties() {      PropertySourcesPlaceholderConfigurer propertySourcesPlaceholderConfigurer =` `new` `PropertySourcesPlaceholderConfigurer();      YamlPropertiesFactoryBean yaml =` `new` `YamlPropertiesFactoryBean();      yaml.setResources(new` `ClassPathResource("appConfig.yml");      propertySourcesPlaceholderConfigurer.setProperties(yaml.getObject());      return` `propertySourcesPlaceholderConfigurer;  }` |
| --- |


2. Using XML bean configuration

| 1234567 | `<context:annotation-config/>` `<bean` `id="yamlProperties"` `class="org.springframework.beans.factory.config.YamlPropertiesFactoryBean">    <property` `name="resources"` `value="classpath:appConfig.yml"/></bean>` `<context:property-placeholder` `properties-ref="yamlProperties"/>` |
| --- |


**Is there any YAML editor available ?**

Yes, Now Spring **STS 3.7.0** has [Spring Boot YAML editor](https://spring.io/blog/2015/05/11/new-in-sts-3-7-0-spring-boot-yaml-editor) which has  boot-specific content-assist, validation, hover-infos and hyperlink detectors. It understands Spring Boot’s configuration metadata .

Check This link for more info : [STS  3.7.0 YAML editor](http://docs.spring.io/sts/nan/latest/NewAndNoteworthy.html).

**Any example for Spring Boot YAML  demo?**

If you want to understand how spring boot yaml works refer this example [Spring-boot-yaml](https://github.com/spring-projects/spring-boot/tree/master/spring-boot-samples/spring-boot-sample-profile) from GitHub.

**References :**

[https://en.wikipedia.org/wiki/YAML](https://en.wikipedia.org/wiki/YAML)  
[http://yaml.org/](http://yaml.org/)  
[http://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-external-config.html](http://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-external-config.html)  


