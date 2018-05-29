# 简单工厂模式-Simple Factory Pattern

简单工厂模式（Simple Factory Pattern）：定义一个工厂类，它可以根据参数的不同返回不同类的实例，被创建的实例通常具有共同的父类。因为在简单工厂模式中用于创建实例的方法是静态（static）方法，因此简单工厂模式又被称为静态工厂方法（Static Factory Method）模式，它属于类创建模式。

实例的创建交由一个工厂类。

```java
package designpattern.simpleFactory;

public class Factory {
    public static Product getProduct(String productName) {
        if (productName == null) {
            return null;
        }
        switch (productName) {
            case "A":
                return new ProductA();
            case "B":
                return new ProductB();
            default:
                return null;
        }
    }
}
```

