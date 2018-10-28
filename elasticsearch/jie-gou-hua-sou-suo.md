# 结构化搜索



结构化搜索  


### 精确值查找

当进行精确值查找时， 我们会使用过滤器（filters）。过滤器很重要，因为它们执行速度非常快，不会计算相关度（直接跳过了整个评分阶段）而且很容易被缓存。我们会在本章后面的 [过滤器缓存](https://elasticsearch.cn/book/elasticsearch_definitive_guide_2.x/filter-caching.html) 中讨论过滤器的性能优势，不过现在只要记住：请尽可能多的使用过滤式查询。

1.Term查询

```text
{
    "term" : {
        "price" : 20
    }
}
```

然而，当我们查找一个精确值的时候，我们不希望对查询进行评分计算。所以我们会使用constant\_score查询以非评分模式来执行term查询并以1作为统一评分。

```text
{
    "query" : {
        "constant_score" : { 
            "filter" : {
                "term" : { 
                    "price" : 20
                }
            }
        }
    }
}
```

  
当我们 使用term查询文本时。会出现查询不到期望结果的情况。因为如果我们使用analyze API时，我们可以看到这里的文本会被拆分成多个更小的token。

```text
GET /my_store/_analyze
{
  "field": "productID",
  "text": "XHDK-A-1293-#fJ3"
}
```



```text
{
  "tokens" : [ {
    "token" :        "xhdk",
    "start_offset" : 0,
    "end_offset" :   4,
    "type" :         "<ALPHANUM>",
    "position" :     1
  }, {
    "token" :        "a",
    "start_offset" : 5,
    "end_offset" :   6,
    "type" :         "<ALPHANUM>",
    "position" :     2
  }, {
    "token" :        "1293",
    "start_offset" : 7,
    "end_offset" :   11,
    "type" :         "<NUM>",
    "position" :     3
  }, {
    "token" :        "fj3",
    "start_offset" : 13,
    "end_offset" :   16,
    "type" :         "<ALPHANUM>",
    "position" :     4
  } ]
}
```

显然这种处理方式是我们不希望的，为了避免这种情况，我们需要告诉ES该字段具有精确值，要将其设置成not\_analyzed无需分析的。我们需要删除旧索引，然后创建一个正确映射的新索引。

```text
DELETE /my_store 

PUT /my_store 
{
    "mappings" : {
        "products" : {
            "properties" : {
                "productID" : {
                    "type" : "string",
                    "index" : "not_analyzed" 
                }
            }
        }
    }

}
```

