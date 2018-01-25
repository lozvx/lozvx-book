http://blog.csdn.net/jiangnan2014/article/details/53735429

http://blog.csdn.net/cpfeed/article/details/7342480



内部维护的是一个long的数组

```
    /**
     * The internal field corresponding to the serialField "bits".
     */
    private long[] words;

    /**
     * The number of words in the logical size of this BitSet.
     */
    private transient int wordsInUse = 0;

    /**
     * Whether the size of "words" is user-specified.  If so, we assume
     * the user knows what he's doing and try harder to preserve it.
     */
    private transient boolean sizeIsSticky = false;
```



