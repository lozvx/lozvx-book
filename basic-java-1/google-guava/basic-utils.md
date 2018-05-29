# Basic Utils

Optional:

```java
    public static void main(String[] args) {
        Optional<Integer> possible = Optional.of(5);
        System.out.println(possible.isPresent()); // returns true
        System.out.println(possible.get()); // returns 5
    }
```

Precondition:

```java
    public static void main(String[] args) {
        int i = -1, j = 3;
        Preconditions.checkArgument(i >= 0, "Argument was %s but expected nonnegative", i);
        Preconditions.checkArgument(i < j, "Expected i < j, but %s > %s", i, j);
    }
```

Ording:

Objects:

equals and hashcode:

```java
    public static void main(String[] args) {
        // equals
        System.out.println(Objects.equal("a", "a")); // returns true
        System.out.println(Objects.equal(null, "a")); // returns false
        System.out.println(Objects.equal("a", null)); // returns false
        System.out.println(Objects.equal(null, null)); // returns true
        //hashcode
        System.out.println(Objects.hashCode(new A("fdsf")));
    }
```

toString:

```java
   // Returns "ClassName{x=1}"
   MoreObjects.toStringHelper(this)
       .add("x", 1)
       .toString();

   // Returns "MyObject{x=1}"
   MoreObjects.toStringHelper("MyObject")
       .add("x", 1)
       .toString();
```

CompareTo:

```java
   public int compareTo(Foo that) {
     return ComparisonChain.start()
         .compare(this.aString, that.aString)
         .compare(this.anInt, that.anInt)
         .compare(this.anEnum, that.anEnum, Ordering.natural().nullsLast())
         .result();
   }
```

StopWatch:

```java
 Stopwatch stopwatch = Stopwatch.createStarted();
 doSomething();
 stopwatch.stop(); // optional

 long millis = stopwatch.elapsed(MILLISECONDS);

log.info("time: " + stopwatch); // formatted string like "12.3 ms"
```

