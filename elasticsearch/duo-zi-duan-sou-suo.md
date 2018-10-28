# 多字段搜索

查询很少是简单一句话的 `match` 匹配查询。通常我们需要用相同或不同的字符串查询一个或多个字段，也就是说，需要对多个查询语句以及它们相关度评分进行合理的合并。  


 最简单的多字段查询可以将搜索项映射到具体的字段。 如果我们知道 _War and Peace_ 是标题，Leo Tolstoy 是作者，很容易就能把两个条件用 `match` 语句表示， 并将它们用 [`bool` 查询](https://elasticsearch.cn/book/elasticsearch_definitive_guide_2.x/bool-query.html) 组合起来：

```text
GET /_search
{
  "query": {
    "bool": {
      "should": [
        { "match": { "title":  "War and Peace" }},
        { "match": { "author": "Leo Tolstoy"   }}
      ]
    }
  }
}
```

语句优先级： 

`boost` 参数。为了提升 `title` 和 `author` 字段的权重， 为它们分配的 `boost` 值大于 `1`

```text
GET /_search
{
  "query": {
    "bool": {
      "should": [
        { "match": { 
            "title":  {
              "query": "War and Peace",
              "boost": 2
        }}},
        { "match": { 
            "author":  {
              "query": "Leo Tolstoy",
              "boost": 2
        }}},
        { "bool":  { 
            "should": [
              { "match": { "translator": "Constance Garnett" }},
              { "match": { "translator": "Louise Maude"      }}
            ]
        }}
      ]
    }
  }
}
```

