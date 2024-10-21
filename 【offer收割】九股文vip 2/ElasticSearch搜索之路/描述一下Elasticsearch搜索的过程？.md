# 👌描述一下Elasticsearch搜索的过程？

### 1. 用户提交查询
用户通过 REST API 或客户端库向 Elasticsearch 提交查询请求。查询请求包含查询字符串、查询条件、过滤条件等。

```plain
{
  "query": {
    "match": {
      "content": "Elasticsearch"
    }
  }
}
```

### 2. 查询解析
Elasticsearch 接收到查询请求后，会进行解析和预处理，将查询转换成内部可理解的查询结构。这包括对查询字符串进行分词、分析和标准化。

### 3. 路由查询
Elasticsearch 集群中的数据分布在多个分片（shard）上。每个索引被分成多个主分片（primary shard），每个主分片又可以有多个副本分片（replica shard）。查询请求会被路由到相关的分片上进行处理。

### 4. 分片级查询
每个相关的分片会独立执行查询操作。

#### a. 倒排索引查找
分片使用倒排索引查找包含查询词语的文档列表。例如，查询 "Elasticsearch" 会查找倒排索引中包含 "Elasticsearch" 的文档。

#### b. 过滤和评分
对于查找到的文档，分片会应用查询条件和过滤条件，并计算每个文档的相关性评分。评分通常基于 TF-IDF 或 BM25 算法。

#### c. 排序和限制
分片对符合条件的文档进行排序，并根据请求中的限制（如分页参数）截取部分结果。

### 5. 聚合结果
所有相关分片的查询结果会被汇总到协调节点（coordinating node），协调节点负责合并和排序来自不同分片的结果。

#### a. 合并结果
协调节点将各个分片返回的结果合并成一个统一的结果集。

#### b. 重新排序
如果查询请求中包含排序条件，协调节点会对合并后的结果集进行重新排序。

### 6. 返回结果
协调节点将最终的搜索结果返回给用户。返回结果通常包含匹配文档的详细信息、相关性评分、总匹配数等。

```plain
{
  "took": 10,
  "timed_out": false,
  "_shards": {
    "total": 5,
    "successful": 5,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 2,
      "relation": "eq"
    },
    "max_score": 1.0,
    "hits": [
      {
        "_index": "my_index",
        "_id": "1",
        "_score": 1.0,
        "_source": {
          "content": "Elasticsearch is a search engine"
        }
      },
      {
        "_index": "my_index",
        "_id": "2",
        "_score": 0.9,
        "_source": {
          "content": "Elasticsearch is powerful"
        }
      }
    ]
  }
}
```

### 7. 高级功能
Elasticsearch 提供了多种高级搜索功能，如聚合（aggregations）、高亮（highlighting）、建议（suggestions）等，这些功能可以在查询过程中被应用，进一步丰富搜索结果。

#### a. 聚合
聚合用于对数据进行统计分析和分组。例如，计算某个字段的平均值、最大值、最小值等。

```plain
{
  "aggs": {
    "avg_price": {
      "avg": {
        "field": "price"
      }
    }
  }
}
```

#### b. 高亮
高亮用于在搜索结果中标记匹配的词语，方便用户查看。

```plain
{
  "query": {
    "match": {
      "content": "Elasticsearch"
    }
  },
  "highlight": {
    "fields": {
      "content": {}
    }
  }
}
```



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/xiwrpp4c0g2t9gga>