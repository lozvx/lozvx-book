当需要引入一个新产品时，由于简单工厂模式通过所传入的参数不同来创建不同的产品，这必定要修改工厂类的源代码，违背了开闭原则。因此产生了工厂方法模式

与简单工厂模式对比，工厂方法模式最重要的区别是引入了抽象工厂角色，抽象工厂可以是接口，也可以是抽象类或者具体类。可以通过替换具体的工厂类生产不同的产品。

SLF4J?

适用场景：

* 客户端不知道它所需要的对象的类。在工厂方法模式中,客户端不需要知道具体产品类的类名,只需要知道所对应的工厂即可,具体的产品对象由具体工厂类创建,可将具体工厂类的类名存储在配置文件或数据库中。
* 抽象工厂类通过其子类来指定创建哪个对象。在工厂方法模式中,对于抽象工厂类只需要提供一个创建产品的接口,而由其子类来确定具体要创建的对象,利用面向对象的多态性和里氏代换原则,在程序运行时,子类对象将覆盖父类对象,从而使得系统更容易扩展。



```java
package designpattern.factoryMethod;


public class FactoryMethodTest {
    public static void main(String[] args) {
        AbstractLoggerFactory factory = new DbLoggerFactory();// can be config by file
        AbstractLogger logger = factory.getLogger();
        logger.log("hello world!");//DbLogger: hello world!
    }
}

```



