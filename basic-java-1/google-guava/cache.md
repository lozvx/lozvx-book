# Cache

集成到SpringCache：[http://www.cnblogs.com/woshimrf/p/5923501.html](http://www.cnblogs.com/woshimrf/p/5923501.html)

```text
LoadingCache<Key, Graph> graphs = CacheBuilder.newBuilder()
       .maximumSize(1000)
       .expireAfterWrite(10, TimeUnit.MINUTES)
       .removalListener(MY_LISTENER)
       .build(
           new CacheLoader<Key, Graph>() {
             public Graph load(Key key) throws AnyException {
               return createExpensiveGraph(key);
             }
           });
```

采用guava cache作为 本地缓存。将guava cache注册到cacheManager里就可以调用了。

* 配置cacheManager

首先 针对要缓存的类型，配置缓存策略。设置最大缓存数量和缓存过期时间。

```text
public static final String HOTEL_POSTION = "hotel_position";  //cache key
@Value("${cache.guavaCache.hotelPosition.maxSize}")
private long hotelPositionMaxSize;
@Value("${cache.guavaCache.hotelPosition.duration}")
private long hotelPositionDuration;

private GuavaCache buildHotelPositionCache() {
        return new GuavaCache(HOTEL_POSTION,
                CacheBuilder.newBuilder()
                        .recordStats()
                        .maximumSize(hotelPositionMaxSize)
                        .expireAfterWrite(hotelPositionDuration, TimeUnit.DAYS)
                        .build());
}
```

将刚才创建的缓存策略添加到cacheManager中

```text
    @Bean
    public CacheManager cacheManager() {
        SimpleCacheManager manager = new SimpleCacheManager();
        List list = new ArrayList();
        list.add(buildHotelPositionCache());
        manager.setCaches(  list );

        return manager;
    }
```

* 配置要缓存的方法

在需要使用这个缓存的地方，使用注解。

```text
@Cacheable(value = CacheManagementConfig.HOTEL_POSTION, key = "{#hotelId}", condition = "", unless = "!#result.isSuccessful()")
    public BaseDomainResponse<HotelPosition> getHotelPosition(int hotelId, String apiToken) {
        //......
    }
```

