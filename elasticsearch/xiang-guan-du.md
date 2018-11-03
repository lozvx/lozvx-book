# 相关度

[https://www.elastic.co/guide/cn/elasticsearch/guide/current/scoring-theory.html](https://www.elastic.co/guide/cn/elasticsearch/guide/current/scoring-theory.html)



### 相关度评分背后的理论

Lucene（或 Elasticsearch）使用 [_布尔模型（Boolean model）_](http://en.wikipedia.org/wiki/Standard_Boolean_model) 查找匹配文档， 并用一个名为 [_实用评分函数（practical scoring function）_](https://www.elastic.co/guide/cn/elasticsearch/guide/current/practical-scoring-function.html) 的公式来计算相关度。这个公式借鉴了 [_词频/逆向文档频率（term frequency/inverse document frequency）_](http://en.wikipedia.org/wiki/Tfidf)和 [_向量空间模型（vector space model）_](http://en.wikipedia.org/wiki/Vector_space_model)，同时也加入了一些现代的新特性，如协调因子（coordination factor），字段长度归一化（field length normalization），以及词或查询语句权重提升。



#### 布尔模型

_布尔模型（Boolean Model）_ 只是在查询中使用 `AND` 、 `OR` 和 `NOT` （与、或和非）这样的条件来查找匹配的文档，以下查询：

```text
full AND text AND search AND (elasticsearch OR lucene)
```

会将所有包括词 `full` 、 `text` 和 `search` ，以及 `elasticsearch` 或 `lucene` 的文档作为结果集。

这个过程简单且快速，它将所有可能不匹配的文档排除在外。



#### 词频/逆向文档频率（TF/IDF）

当匹配到一组文档后，需要根据相关度排序这些文档，不是所有的文档都包含所有词，有些词比其他的词更重要。一个文档的相关度评分部分取决于每个查询词在文档中的 _权重_ 。

**词频**

词在文档中出现的频度是多少？ 频度越高，权重 _越高_ 

**逆向文档频率**

词在集合所有文档里出现的频率是多少？频次越高，权重 _越低_

**字段长度归一值**

字段的长度是多少？ 字段越短，字段的权重 _越高_ 。如果词出现在类似标题 `title` 这样的字段，要比它出现在内容 `body` 这样的字段中的相关度更高。





#### 向量空间模型

_向量空间模型（vector space model）_ 提供一种比较多词查询的方式，单个评分代表文档与查询的匹配程度，为了做到这点，这个模型将文档和查询都以 _向量（vectors）_ 的形式表示：

向量实际上就是包含多个数的一维数组，例如：

```text
[1,2,5,22,3,8]
```

