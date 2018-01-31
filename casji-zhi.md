参考：

[http://mp.weixin.qq.com/s/f9PYMnpAgS1gAQYPDuCq-w](http://mp.weixin.qq.com/s/f9PYMnpAgS1gAQYPDuCq-w)

http://blog.csdn.net/javazejian/article/details/72772470

http://www.cnblogs.com/pkufork/p/java\_unsafe.html





**什么是CAS？**

CAS是英文单词**Compare And Swap**的缩写，翻译过来就是比较并替换。

CAS机制当中使用了3个基本操作数：内存地址V，旧的预期值A，要修改的新值B。

更新一个变量的时候，只有当变量的预期值A和内存地址V当中的实际值相同时，才会将内存地址V对应的值修改为B。



**CPU指令对CAS的支持**

或许我们可能会有这样的疑问，假设存在多个线程执行CAS操作并且CAS的步骤很多，有没有可能在判断V和E相同后，正要赋值时，切换了线程，更改了值。造成了数据不一致呢？答案是否定的，因为CAS是一种系统原语，原语属于操作系统用语范畴，是由若干条指令组成的，用于完成某个功能的一个过程，并且原语的执行必须是连续的，在执行过程中不允许被中断，也就是说CAS是一条CPU的原子指令，不会造成所谓的数据不一致问题。



  


