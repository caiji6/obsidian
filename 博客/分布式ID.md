分布式全局ID的解决方案有很多，比如说可以使用MySQL的全局表、Zookeeper的有序节点、MongoDB的object id、Redis的自增id以及UUID等等。这些方案只是解决基础的ID唯一性的问题，在实际生成环境里面，我们需要去构建一个全局唯一的id还需要考虑很多的因素，比如说：

1. <mark style="background: #FF5582A6;">有序性</mark>：有序的ID能够更好的确认数据的位置，以及B+树的存储结构中，范围查询的效率更高，并且可以提升B+树数据维护的效率。
   
2. <mark style="background: #FF5582A6;">安全性</mark>：避免恶意爬取数据造成数据泄露。
   
3. <mark style="background: #FF5582A6;">可用性</mark>：ID生成系统的可用性要求非常高，一旦出现故障就会造成业务不可用的问题。
   
4. <mark style="background: #FF5582A6;">高性能</mark>：全局ID生成系统需要满足整个公司的业务需求，涉及到亿级别的调用，对性能要求较高。

