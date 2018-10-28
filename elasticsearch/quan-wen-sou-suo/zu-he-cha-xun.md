# 组合查询

 与过滤器一样， `bool` 查询也可以接受 `must` 、 `must_not` 和 `should` 参数下的多个查询语句



```text
GET /my_index/my_type/_search
{
  "query": {
    "bool": {
      "must":     { "match": { "title": "quick" }},
      "must_not": { "match": { "title": "lazy"  }},
      "should": [
                  { "match": { "title": "brown" }},
                  { "match": { "title": "dog"   }}
      ]
    }
  }
}
```

