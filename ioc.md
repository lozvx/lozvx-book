### 什么是IOC的功能？

#### 概念

> IoC -- Inverse of Control，控制反转，将对象的创建权反转给Spring！！ 使用IOC可以解决的程序耦合性高的问题！！

#### 控制反转

> 假设我需要做一个功能，在这个功能当中我需要调用servic层，然后再调用dao层，去取数据。在传统的javaEE开发中我就直接去new一个service 然后再new一个dao。但是在spring框架中，我们吧new service和new dao的权利交个spring框架，假设我需要使用我就直接去spring框架中寻找。等于说我的资源创建的权利交给了spring框架，这就叫做控制反转。

#### 解耦

> 刚刚我们说资源创建交给了sring，我们需要什么就找spring。这过程就像是工厂模式。但是在spring框架中它需要创建哪些对象，它需要一个配置文件。这个配置文件告诉spring，需要创建哪些资源。

例如：假设我需要去数据库查询数据显示页面

> 程序启动，spring框架去找配置文件创建资源，把资源放置再一个容器中，开始运行，前端请求数据，在spring中找controller层，再找service层，再找dao层要数据，最后数据原路返回controller，再显示到页面上。其中service被spring注入到controlller层，dao层被spring注入到service层。这个过程分工明确。每一层各司其职。传统的一个开发，在servlet中直接new然后去查数据，然后数据返回到界面上。万一操作一多所有的判断，查询不同的表，这个servlet的代码变得十分的臃肿。不说开发慢，你开发完了看代码也费劲。 所以说控制反转可以用来解耦

 

---

The BeanFactory interface provides an advanced configuration mechanism capable of managing any type of object. ApplicationContext is a sub-interface of BeanFactory

