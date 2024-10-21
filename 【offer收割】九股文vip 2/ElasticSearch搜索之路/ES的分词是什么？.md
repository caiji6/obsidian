# 👌ES的分词是什么？

Elasticsearch（ES）的分词（Tokenization）是全文搜索引擎中一个关键的过程，它将文本拆分成更小的单元（称为“tokens”或“词元”），这些单元是搜索和索引的基本单位。分词是文本分析的一部分，通常在索引文档和查询时进行。

### 分词的作用
1. **索引创建**：当文档被索引时，Elasticsearch 会将文本字段的内容分解为词元，并将这些词元存储在倒排索引中。这使得搜索引擎可以快速查找到包含特定词元的文档。
2. **查询处理**：当用户提交查询时，Elasticsearch 会将查询字符串分解为词元，并在索引中查找这些词元，从而找到匹配的文档。

### 分词器（Analyzers）
Elasticsearch 使用分词器（Analyzers）来执行分词过程。一个分词器通常由以下几个部分组成：

1. **字符过滤器（Character Filters）**：在分词之前对文本进行预处理，例如去除 HTML 标签或转换字符。
2. **分词器（Tokenizer）**：将文本拆分成词元。
3. **词元过滤器（Token Filters）**：对词元进行进一步处理，例如去除停用词（常见的无意义词，如“the”、“and”）、小写转换、词干提取等。

### 内置分词器
Elasticsearch 提供了多种内置分词器，常见的包括：

1. **Standard Analyzer**：默认分词器，适用于大多数西方语言。它使用 Unicode 文本分割算法，并进行小写转换和去除停用词。
2. **Simple Analyzer**：将文本按非字母字符分割，并将所有字符转换为小写。
3. **Whitespace Analyzer**：按空白字符分割文本，不进行小写转换或去除停用词。
4. **Keyword Analyzer**：不进行分词，直接将整个输入作为一个词元。适用于精确匹配的字段。
5. **Pattern Analyzer**：使用正则表达式进行分词。
6. **Custom Analyzer**：用户可以自定义分词器，组合不同的字符过滤器、分词器和词元过滤器。

### 示例
```plain
{
  "analyzer": "standard",
  "text": "The quick brown fox jumps over the lazy dog"
}
```

输出结果：

```plain
{
  "tokens": [
    {"token": "the", "start_offset": 0, "end_offset": 3, "type": "<ALPHANUM>", "position": 0},
    {"token": "quick", "start_offset": 4, "end_offset": 9, "type": "<ALPHANUM>", "position": 1},
    {"token": "brown", "start_offset": 10, "end_offset": 15, "type": "<ALPHANUM>", "position": 2},
    {"token": "fox", "start_offset": 16, "end_offset": 19, "type": "<ALPHANUM>", "position": 3},
    {"token": "jumps", "start_offset": 20, "end_offset": 25, "type": "<ALPHANUM>", "position": 4},
    {"token": "over", "start_offset": 26, "end_offset": 30, "type": "<ALPHANUM>", "position": 5},
    {"token": "lazy", "start_offset": 31, "end_offset": 35, "type": "<ALPHANUM>", "position": 6},
    {"token": "dog", "start_offset": 36, "end_offset": 39, "type": "<ALPHANUM>", "position": 7}
  ]
}
```

### 自定义分词器
以下是自定义分词器的示例，它使用标准分词器，但添加了一个小写过滤器和一个停用词过滤器：

```plain
PUT /my_index
{
  "settings": {
    "analysis": {
      "analyzer": {
        "my_custom_analyzer": {
          "type": "custom",
          "tokenizer": "standard",
          "filter": [
            "lowercase",
            "stop"
          ]
        }
      }
    }
  }
}
```



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/nghumwycg8d5spn8>