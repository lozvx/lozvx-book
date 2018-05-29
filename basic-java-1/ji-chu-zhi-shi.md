# 基础知识

参考：[https://github.com/francistao/LearningNotes/blob/master/Part2/JavaSE/Java基础知识.md](https://github.com/francistao/LearningNotes/blob/master/Part2/JavaSE/Java基础知识.md)

八种基本数据类型：int ,double ,long ,float, short,byte,char,boolean

对应的封装类型是：Integer ,Double ,Long ,Float, Short,Byte,Character,Boolean

this关键字有两个用途

* 引用隐式参数
* 调用该类其他的构造器

super关键字也有两个用途

* 调用超类的方法
* 调用超类的构造器

一个对象变量可以引用多种实际类型的现象被称为多态。在运行时能够自动选择调用哪个方法的现象被称为动态绑定。

类即使不含抽象方法，也可以将类声明为抽象类。

抽象类不能被实例化。

在Java中，只有基本类型不是对象，例如，数值，字符和布尔类型到值都不是对象。所有数组类型，不管是对象数组还是基本类型到数组都扩展于Object类。

所有枚举类型都是Enum类的子类。他们继承你这个类到许多方法。

public enum Size {SMALL,MEDIUM,LAGRE,EXTRA\_LARGE};

这个声明定义的类型是一个类，它刚好有4个实例。

接口与抽象类

[http://www.importnew.com/18780.html](http://www.importnew.com/18780.html)

[/www.importnew.com/12399.html](https://github.com/lozvx/lozvx-book/tree/d8fcb28607b2fe17707d1cc1946caf37bd1ccf5f/www.importnew.com/12399.html)

内部类

* 内部类方法可以访问该类定义所在到作用域中的数据，包括私有数据。
* 内部类可以对同一个包中到其他类隐藏起来。
* 当想要定义一个回调函数且不想编写大量代码时，可以使用匿名（anonymous）内部类实现。

回调函数

www.importnew.com/19301.html

[https://www.bysocket.com/?p=636](https://www.bysocket.com/?p=636)

