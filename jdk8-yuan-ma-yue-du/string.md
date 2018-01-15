```
    /** The value is used for character storage. */
    private final char value[];

    /** Cache the hash code for the string */
    private int hash; // Default to 0
```

value保存值，hash缓存hashcode

Java中的数组很多用System.arraycopy\(\)方法进行数组拷贝，String中的substring，ArrayList中的add，remove等。

[http://xuyuanshuaaa.iteye.com/blog/1046621](http://xuyuanshuaaa.iteye.com/blog/1046621)

下面的方法是，将toIndex后的元素 往前copy，然后将后面的元素 =null，让GC回收。

```
    protected void removeRange(int fromIndex, int toIndex) {
        modCount++;
        int numMoved = size - toIndex;
        System.arraycopy(elementData, toIndex, elementData, fromIndex,
                         numMoved);

        // clear to let GC do its work
        int newSize = size - (toIndex-fromIndex);
        for (int i = newSize; i < size; i++) {
            elementData[i] = null;
        }
        size = newSize;
    }
```



