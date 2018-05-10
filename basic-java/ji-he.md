# 集合

for each 循环可以与任何实现了iterable接口的对象一起工作，这个接口只包含一个方法。

```java
public interface Iterable<E>
{
Iterator<E> iterator();
}
```

所有map都是实现你Map接口，而不是Collection接口。![](../.gitbook/assets/collectionvscollections.jpeg)![](../.gitbook/assets/java-collection-hierarchy.jpeg)

