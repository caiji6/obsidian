# 👌Es中的分析器是什么？

在 Elasticsearch（ES）中，分析器（Analyzer）是一个用于处理文本的组件，它将文本分解为更小的单元（词元，tokens），这些词元是索引和搜索的基本单位。

### 分析器的组成部分
一个分析器通常由以下几个部分组成：

1. **字符过滤器（Character Filters）**：在分词之前对文本进行预处理。例如，可以用字符过滤器来去除 HTML 标签或将特定字符转换为其他字符。
2. **分词器（Tokenizer）**：将文本拆分成词元。分词器是分析器的核心组件，它决定了文本如何被切分。
3. **词元过滤器（Token Filters）**：对词元进行进一步处理。例如，可以用词元过滤器来去除停用词（如“the”、“and”）、进行小写转换、词干提取等。

### 内置分析器
Elasticsearch 提供了多种内置分析器，常见的包括：

1. **Standard Analyzer**：默认分析器，适用于大多数西方语言。它使用 Unicode 文本分割算法，并进行小写转换和去除停用词。
2. **Simple Analyzer**：将文本按非字母字符分割，并将所有字符转换为小写。
3. **Whitespace Analyzer**：按空白字符分割文本，不进行小写转换或去除停用词。
4. **Keyword Analyzer**：不进行分词，直接将整个输入作为一个词元。适用于精确匹配的字段。
5. **Pattern Analyzer**：使用正则表达式进行分词。
6. **Language-specific Analyzers**：针对特定语言的分析器，例如`english`、`french`、`german`等。这些分析器通常会包含特定语言的停用词和词干提取规则。



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/cri9qc4ro12yg6as>