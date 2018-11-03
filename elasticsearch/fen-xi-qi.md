# 分析器



* 清除类似 `´` ， `^` ， `¨` 的变音符号，这样在搜索 `rôle` 的时候也会匹配 `role`，反之亦然。请见 [_归一化词元_](https://www.elastic.co/guide/cn/elasticsearch/guide/current/token-normalization.html)。
* 通过提取单词的词干，清除单数和复数之间的差异—`fox` 与 `foxes`—以及时态上的差异—`jumping` 、 `jumped` 与 `jumps` 。请见 [_将单词还原为词根_](https://www.elastic.co/guide/cn/elasticsearch/guide/current/stemming.html)。
* 清除常用词或者 _停用词_ ，如 `the` ， `and` ， 和 `or` ，从而提升搜索性能。请见 [_停用词: 性能与精度_](https://www.elastic.co/guide/cn/elasticsearch/guide/current/stopwords.html)。
* 包含同义词，这样在搜索 `quick` 时也可以匹配 `fast` ，或者在搜索 `UK` 时匹配 `United Kingdom` 。 请见 [_同义词_](https://www.elastic.co/guide/cn/elasticsearch/guide/current/synonyms.html)。
* 检查拼写错误和替代拼写方式，或者 _同音异型词_ —发音一致的不同单词，例如 `their` 与 `there` ， `meat` 、 `meet` 与 `mete` 。 请见 [_拼写错误_](https://www.elastic.co/guide/cn/elasticsearch/guide/current/fuzzy-matching.html)。



分析器承担了以下四种角色：

* 文本拆分为单词：

  `The quick brown foxes` → \[ `The`, `quick`, `brown`, `foxes`\]

* 大写转小写：

  `The` → `the`

* 移除常用的 \_停用词\_：

  \[ `The`, `quick`, `brown`, `foxes`\] → \[ `quick`, `brown`, `foxes`\]

* 将变型词（例如复数词，过去式）转化为词根：

  `foxes` → `fox`

\`\`

\`\`

