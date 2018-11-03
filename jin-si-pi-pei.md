# 近似匹配

match 查询可以告知我们这大袋子中是否包含查询的词条，但却无法告知词语之间的关系。



### 短语匹配

 类似 `match` 查询， `match_phrase` 查询首先将查询字符串解析成一个词项列表，然后对这些词项进行搜索，但只保留那些包含 _全部_ 搜索词项，且 _位置_ 与搜索词项相同的文档。 比如对于 `quick fox` 的短语搜索可能不会匹配到任何文档，因为没有文档包含的 `quick` 词之后紧跟着 `fox` 。

```text
"match": {
    "title": {
        "query": "quick brown fox",
        "type":  "phrase"
    }
}
```

### 多值字段



### 性能优化

短语查询和邻近查询都比简单的 `query` 查询代价更高 。 一个 `match` 查询仅仅是看词条是否存在于倒排索引中，而一个 `match_phrase` 查询是必须计算并比较多个可能重复词项的位置。

[Lucene nightly benchmarks](http://people.apache.org/~mikemccand/lucenebench/) 表明一个简单的 `term` 查询比一个短语查询大约快 10 倍，比邻近查询\(有 `slop` 的短语 查询\)大约快 20 倍。当然，这个代价指的是在搜索时而不是索引时。

