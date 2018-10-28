# 多词查询

match查询支持多词查询



```text
GET /my_index/my_type/_search
{
    "query": {
        "match": {
            "title": "BROWN DOG!"
        }
    }
}
```

上面查询返回



```text
{
  "hits": [
     {
        "_id":      "4",
        "_score":   0.73185337, 
        "_source": {
           "title": "Brown fox brown dog"
        }
     },
     {
        "_id":      "2",
        "_score":   0.47486103, 
        "_source": {
           "title": "The quick brown fox jumps over the lazy dog"
        }
     },
     {
        "_id":      "3",
        "_score":   0.47486103, 
        "_source": {
           "title": "The quick brown fox jumps over the quick dog"
        }
     },
     {
        "_id":      "1",
        "_score":   0.11914785, 
        "_source": {
           "title": "The quick brown fox"
        }
     }
  ]
}
```



因为 `match` 查询必须查找两个词（ `["brown","dog"]` ），它在内部实际上先执行两次 `term` 查询，然后将两次查询的结果合并作为最终结果输出。为了做到这点，它将两个 `term` 查询包入一个 `bool` 查询中，详细信息见 [布尔查询](https://elasticsearch.cn/book/elasticsearch_definitive_guide_2.x/bool-query.html)。



### 控制精度

match查询支持 `minimum_should_match` 最小匹配参数， 这让我们可以指定必须匹配的词项数用来表示一个文档是否相关

```text
GET /my_index/my_type/_search
{
  "query": {
    "match": {
      "title": {
        "query":                "quick brown dog",
        "minimum_should_match": "75%"
      }
    }
  }
}
```

  


