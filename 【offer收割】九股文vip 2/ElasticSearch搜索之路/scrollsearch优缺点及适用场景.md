# 👌scroll search 优缺点及适用场景

### 优点
1. **高效处理大数据量**：

`scroll`搜索专为处理大量数据设计，可以高效地从索引中提取大量文档，而不会因为深度分页而导致性能问题。

1. **稳定的一致性**：

`scroll`搜索会在首次查询时创建一个快照，这样即使在后续的滚动查询过程中索引发生变化（如新增或删除文档），查询结果也不会受到影响。

1. **适用于批量数据处理**：

适合需要一次性处理大量数据的场景，例如数据迁移、批量数据导出、数据分析等。

### 缺点
1. **资源占用**：

`scroll`搜索会在整个查询期间占用集群资源，包括内存和 CPU。长时间保持一个`scroll`会话会占用大量资源，可能影响集群的整体性能。

1. **不适合实时性要求高的应用**：

由于`scroll`搜索基于快照进行查询，后续的滚动查询结果不会反映索引的最新状态，因此不适合需要实时数据更新的应用场景。

1. **复杂性增加**：

实现`scroll`搜索需要管理滚动上下文（scroll context），包括处理滚动 ID（scroll ID）和滚动窗口（scroll window），增加了实现的复杂性。

### 适用场景
1. **批量数据导出**：当需要导出大量数据时，`scroll`搜索可以高效地提取数据，避免传统分页带来的性能问题。
2. **数据迁移**：在进行索引数据迁移时，`scroll`搜索可以确保一致性和高效性。
3. **大规模数据分析**：适用于需要一次性处理大量数据的分析任务，例如日志分析、数据挖掘等。

### 示例
使用`scroll`搜索进行批量数据提取的示例代码：

```plain
import org.elasticsearch.action.search.SearchRequest;
import org.elasticsearch.action.search.SearchResponse;
import org.elasticsearch.action.search.SearchScrollRequest;
import org.elasticsearch.client.RequestOptions;
import org.elasticsearch.client.RestHighLevelClient;
import org.elasticsearch.common.unit.TimeValue;
import org.elasticsearch.index.query.QueryBuilders;
import org.elasticsearch.search.Scroll;
import org.elasticsearch.search.SearchHit;
import org.elasticsearch.search.builder.SearchSourceBuilder;

RestHighLevelClient client = new RestHighLevelClient(
    RestClient.builder(new HttpHost("localhost", 9200, "http"))
);

final Scroll scroll = new Scroll(TimeValue.timeValueMinutes(1L));
SearchSourceBuilder searchSourceBuilder = new SearchSourceBuilder()
    .query(QueryBuilders.matchAllQuery())
    .size(100);  // 每页大小

SearchRequest searchRequest = new SearchRequest("index_name")
    .scroll(scroll)
    .source(searchSourceBuilder);

SearchResponse searchResponse = client.search(searchRequest, RequestOptions.DEFAULT);
String scrollId = searchResponse.getScrollId();
SearchHit[] hits = searchResponse.getHits().getHits();

while (hits.length > 0) {
    // 处理当前页的数据
    for (SearchHit hit : hits) {
        System.out.println(hit.getSourceAsString());
    }

    // 创建新的滚动请求
    SearchScrollRequest scrollRequest = new SearchScrollRequest(scrollId);
    scrollRequest.scroll(scroll);
    searchResponse = client.scroll(scrollRequest, RequestOptions.DEFAULT);
    scrollId = searchResponse.getScrollId();
    hits = searchResponse.getHits().getHits();
}

// 清除滚动上下文
ClearScrollRequest clearScrollRequest = new ClearScrollRequest();
clearScrollRequest.addScrollId(scrollId);
client.clearScroll(clearScrollRequest, RequestOptions.DEFAULT);

// 关闭客户端
client.close();
```

在这个示例中，`scroll`搜索用于批量提取数据，每次查询都会使用滚动 ID（scroll ID）来获取下一批数据，直到没有更多数据为止。最后，通过`clearScroll`请求清除滚动上下文，释放资源。

`scroll`搜索是一种高效的批量数据提取机制，特别适合处理大数据量和需要一次性获取大量数据的场景。尽管它在实时性和资源占用方面有一定的限制，但在数据迁移、批量导出和大规模数据分析等场景中，它能显著提升查询性能和一致性。



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/ngo4mmw8nguigno0>