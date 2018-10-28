# 匹配查询

 匹配查询 `match` 是个 _核心_ 查询。无论需要查询什么字段， `match` 查询都应该会是首选的查询方式。 它是一个高级 _全文查询_ ，这表示它既能处理全文字段，又能处理精确字段。



```text
GET /my_index/my_type/_search
{
    "query": {
        "match": {
            "title": "QUICK!"
        }
    }
}
```

Elasticsearch 执行上面这个 `match` 查询的步骤是：

1. _检查字段类型_ 。

   标题 `title` 字段是一个 `string` 类型（ `analyzed` ）已分析的全文字段，这意味着查询字符串本身也应该被分析。

2. _分析查询字符串_ 。

   将查询的字符串 `QUICK!` 传入标准分析器中，输出的结果是单个项 `quick` 。因为只有一个单词项，所以 `match` 查询执行的是单个底层 `term` 查询。

3. _查找匹配文档_ 。

   用 `term` 查询在倒排索引中查找 `quick` 然后获取一组包含该项的文档，本例的结果是文档：1、2 和 3 。

4. _为每个文档评分_ 。

   用 `term` 查询计算每个文档相关度评分 `_score` ，这是种将 词频（term frequency，即词 `quick` 在相关文档的 `title` 字段中出现的频率）和反向文档频率（inverse document frequency，即词 `quick` 在所有文档的 `title` 字段中出现的频率），以及字段的长度（即字段越短相关度越高）相结合的计算方式。参见 [相关性的介绍](https://elasticsearch.cn/book/elasticsearch_definitive_guide_2.x/relevance-intro.html) 。

