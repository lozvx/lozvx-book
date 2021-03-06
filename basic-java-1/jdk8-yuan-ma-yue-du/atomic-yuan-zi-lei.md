# Atomic原子类

Atomic原子类，内部使用unsafe类进行原子级的操作。如incrementAndGet等。

```text
// setup to use Unsafe.compareAndSwapInt for updates
// 使用unsafe类进行CAS操作
private static final Unsafe unsafe = Unsafe.getUnsafe();
private static final long valueOffset;

static {
    try {
        valueOffset = unsafe.objectFieldOffset
            (AtomicInteger.class.getDeclaredField("value"));
    } catch (Exception ex) { throw new Error(ex); }
}

//内部存储，使用volatile内存可见
private volatile int value;

    /**
     * Atomically increments by one the current value.
     * unsafe原子操作
     * @return the updated value
     */
    public final int incrementAndGet() {
        return unsafe.getAndAddInt(this, valueOffset, 1) + 1;
    }

```

 

