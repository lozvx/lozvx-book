Java语言规范将派生于Error类或RuntimeException类的所有异常称为未检查异常，所有其它异常称为已检查异常。

Red colored are checked exceptions which must either be caught or declared in the method’s throws clause.



* 如果在子类覆盖了超类的一个方法，子类方法中声明的已检查异常不能超过超类方法中声明的异常范围。也就是说子类方法抛出的异常范围更小，或者可以根本不抛出异常。



![](/assets/Exception-Hierarchy-Diagram.jpeg)



