# String utilities

[https://github.com/google/guava/wiki/StringsExplained](https://github.com/google/guava/wiki/StringsExplained)

```java
    public static void main(String[] args) {
        // Join
        Joiner joiner = Joiner.on("; ").skipNulls();
        System.out.println(joiner.join("Harry", null, "Ron", "Hermione"));
        System.out.println(Joiner.on(",").join(Arrays.asList(1, 5, 7)));// returns "1,5,7"

        //Split
        System.out.println(Splitter.on(',')
                .trimResults()
                .omitEmptyStrings()
                .split("foo,bar,,   qux"));
    }
```

