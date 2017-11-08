#ElasticSearch文档

##参考资源
- 权威指南中文版(版本旧,写这个文档时候的文字基本都是本书的复制粘贴,读英文较慢，只在编写代码时候，去找到特定章节仔细读)
[链接](https://www.elastic.co/guide/cn/elasticsearch/guide/current/getting-started.html)
- 英文Reference(最主要编程参考资料)
[链接](https://www.elastic.co/guide/en/elasticsearch/reference/current/getting-started.html)
- 客户端Reference(类似jdbc,jedis作用,现在用的是lowlevel)
[链接](https://www.elastic.co/guide/en/elasticsearch/client/index.html)
----------------------
##基础概念
####[面向文档](https://www.elastic.co/guide/cn/elasticsearch/guide/current/_document_oriented.html)<br>
Elasticsearch是面向文档的，意味着它存储整个对象或文档<br>
在应用程序中对象很少只是一个简单的键和值的列表。通常，它们拥有更复杂的数据结构，可能包括日期、地理信息、其他对象或者数组等
也许有一天你想把这些对象存储在数据库中。使用关系型数据库的行和列存储，这相当于是把一个表现力丰富的对象挤压到一个非常大的电子表格中：你必须将这个对象扁平化来适应表结构--通常一个字段>对应一列--而且又不得不在每次查询时重新构造对象<br>
####[分布式](https://www.elastic.co/guide/cn/elasticsearch/guide/current/_an-empty-cluster.html)
一个运行中的 Elasticsearch实例称为一个节点(一个elasticsearch进程,在命令行运行起来的一个程序)，而集群是由一个或者多个拥有相同 cluster.name 配置的节点组成， 它们共同承担数据和负载的压力。当有节点加入集群中或者从集群中移除节点时，集群将会重新平均分布所有的数据
####[索引和分片](https://www.elastic.co/guide/cn/elasticsearch/guide/current/_add-an-index.html)
我们往Elasticsearch添加数据时需要用到索引——保存相关数据的地方。索引实际上是指向一个或者多个物理分片的逻辑命名空间<br>
一个分片是一个底层的工作单元，它仅保存了全部数据中的一部分，分片是一个 Lucene 的实例，以及它本身就是一个完整的搜索引擎,我们的文档被存储和索引到分片内，但是应用程序是直接与索引而不是与分片进行交互。

分片以及单点故障.水平扩容.负载均衡 强烈建议点开链接自己看,很清晰,有图有真相 结点->索引->分片 [链接](https://www.elastic.co/guide/cn/elasticsearch/guide/current/_add-an-index.html),点链接自己看把,中文的而且是简要说明，看了可以解决一些问题:
- 为什么要分片,主分片副分片的含义
- 关闭集群中的几个结点有什么影响
- 如何避免单点故障以及提高吞吐量
- master结点和slave结点的区别和关系
- [文档如何决定保存在哪个分片以及为何主分片数创建后不能变](https://www.elastic.co/guide/cn/elasticsearch/guide/current/routing-value.html)
---
##输入和输出

####[文档元数据](https://www.elastic.co/guide/cn/elasticsearch/guide/current/_Document_Metadata.html)
一个文档不仅仅包含它的数据 ，也包含元数据 —— 有文档的描述信息。<br>
三个必须的元数据元素如下：
_index
文档在哪存放
_type
文档表示的对象类别
_id
文档唯一标识  <br>
一个索引应该是因共同的特性被分组到一起的文档集合。例如，你可能存储所有的产品在索引products中，而存储所有销售的交易到索引sales中。虽然也允许存储不相关的数据到一个索引中，但这通常看作是一个反模式的做法<br>
(这里点链接自己看一下把,应该是比较重要的,虽然可以把es的index大致当做对应mysql的database,es的type当做mysql的表,但是实际在设计es结构的时候应该是不推荐的,比如，产品和分类在mysql中应该位于一个database的俩个table,但是在ES中不应该让这俩个种类型在同一个索引中，因为结构相差很大([这里只是举例说一下,实际中就算用es存,也是在把分类作为产品文档对象的一个属性。](https://www.elastic.co/guide/cn/elasticsearch/guide/current/document.html)),详细看[链接1](https://www.elastic.co/guide/cn/elasticsearch/guide/current/_Document_Metadata.html),[链接2](https://www.elastic.co/guide/cn/elasticsearch/guide/current/mapping.html))
---
##搜索
这个部分点开链接自己仔细看把,如果只是简单使用其他章节不用看,只把下面4个链接看了就可以<br>
1.[映射和分析，精确值VS全文](https://www.elastic.co/guide/cn/elasticsearch/guide/current/mapping-analysis.html)<br>
2.[倒排索引例子](https://www.elastic.co/guide/cn/elasticsearch/guide/current/inverted-index.html)<br>
3.[分析与分析器!!!](https://www.elastic.co/guide/cn/elasticsearch/guide/current/analysis-intro.html) <br>
4.[映射](https://www.elastic.co/guide/cn/elasticsearch/guide/current/analysis-intro.html)（理解就可以,api在新版本有部分替换了,string那个变为keyword和text）<br>
- [测试分析器](https://www.elastic.co/guide/cn/elasticsearch/guide/current/analysis-intro.html)
- [测试域的分析效果](https://www.elastic.co/guide/cn/elasticsearch/guide/current/mapping-intro.html)
- [自定义分析器](https://www.elastic.co/guide/cn/elasticsearch/guide/current/custom-analyzers.html)
- [创建符合需求的自定义索引库(自己决定分片数和映射（相当于db的schema)](https://www.elastic.co/guide/cn/elasticsearch/guide/current/_creating_an_index.html)<br>
- [ES的类型实际上是一个用于过滤的普通字段？](https://www.elastic.co/guide/cn/elasticsearch/guide/current/mapping.html)

5.分布式
- [分布式存储-为何主分片数量无法变](https://www.elastic.co/guide/cn/elasticsearch/guide/current/routing-value.html)
- [分布式搜索-为何不能深度分页](https://www.elastic.co/guide/cn/elasticsearch/guide/current/_query_phase.html)
##有几个常见的问题肯定会有问到,我直接整理好了中文链接,在下面
>1.1问：能否直接改变某个文档的某个的字段(不想重新索引整个文档)？<br>
>[答:不能改变，要重新索引该文档,只是重新保存一下这个文档，没什么代价,点击详情](https://www.elastic.co/guide/cn/elasticsearch/guide/current/update-doc.html)<br>
>1.2问：能否改变某个映射（比如要在某个字段上换分词器,实际上是在改变映射也就是schema,代价很大）<br>
>[要重新建立索引库](https://www.elastic.co/guide/cn/elasticsearch/guide/current/mapping-intro.html)
>2.问：项目为何数据在mysql和elastic中各一份？<br>
>[答：这个官网指南自己也建议把关系数据库作为主存,es用于搜索(这个权威指南是ES官方的,只是版本旧一点,最新的api参考得看特定版本的reference)](https://www.elastic.co/guide/cn/elasticsearch/guide/current/version-control.html)<br>
>3.问：如何批量操作数据,以避免多次请求的开销以及批处理的size多大合适<br>
>[答：链接大概指导,具体要根据硬件](https://www.elastic.co/guide/cn/elasticsearch/guide/current/bulk.html)<br>
>4.问：建索引为什么要有多个主分片？又为啥要给每个主分片有一个副本分片<br>
>[答：]()[①决定了索引保存文档的最大数量](https://www.elastic.co/guide/cn/elasticsearch/guide/current/_add-an-index.html)[②当添加结点(新增加一台主机)时候能通过移动分片增加处理能力](https://www.elastic.co/guide/cn/elasticsearch/guide/current/shard-scale.html)<br>
5.问：一个分片是一个lucene索引,当对elasticsearch发出写请求,到底请求会被哪个结点的哪个分片所处理？副本分片除了保存数据外,可以处理何种请求?<br>
[答：①当发送请求到集群中的任一节点。 每个节点都有能力处理任意请求。 每个节点都知道集群中任一文档位置，所以可以直接将请求转发到需要的节点上。②
新建、索引和删除请求都是写!!!操作，必须在主分片上面完成之后才能被复制到相关的副本分片③可以从主分片或者从其它任意副本分片检索文档（副本分片可用来检索数据）④在处理读！！！请求时，协调结点在每次请求的时候都会通过轮询所有的副本分片来达到负载均衡](https://www.elastic.co/guide/cn/elasticsearch/guide/current/distrib-write.html)<br>
6.IK分词器是否在一个结点中配置就可以？<br>
答：不可以,必须在所有结点上都配置，如果只在一个结点上配置了特定的分词器,则只有这个结点有能力处理关于这个带有分词器映射的索引库的请求,在图中(无图)就会看到分片只能存在于本结点，其他结点无法负载需要用特定分词器的索引库的分片
7.如果开启了一个集群,关闭掉一个结点是否会使得集群丢失数据?<br>
[答：在创建索引库时候,可以设置分片数,默认主分片5个,并且每个主分片对应一个副本分片,如果不使用默认配置而自己指定副本分片为0,则会导致在关闭一个结点的时候,丢失特定的主分片从而导致数据丢失,举例：假如索引没有副本分片,只有2个主分片,那么在开启一个结点的时候，俩个分片都在同一个结点上,此时又开启了一个结点,为了负载均衡,其中一个分片会跑到新结点,从而让俩台机器都有能力处理请求,此时关闭一个结点,就会导致一个分片丢失，数据功能丢失,但是假如俩个除了俩个初始的主分片，每个分片还有一个副本分片,则会在创建了一个新结点的时候，每个结点各有俩个分片，虽然此时数据冗余（特意的）但是此时关闭任意一个结点数据都不会丢失,因为数据有俩分](https://www.elastic.co/guide/cn/elasticsearch/guide/current/_scale_horizontally.html)<br>
[副本分片不光是容灾的备份，最主要是可以帮助整体性能,主分片不能变,副分片可以增加（搜索的性能）](https://www.elastic.co/guide/cn/elasticsearch/guide/current/_scale_horizontally.html)

###db->index table->type只是简单方便理解下,实际上建立类型不能按照这个关系,（我自己理解，应该是mysql一个table对应一个索引库,以后再看。。）
[类型问题1-冲突和效率](https://www.elastic.co/guide/cn/elasticsearch/guide/current/mapping.html)
这个不太理解,但是现在照着避免就好(lucene我只是简单了解,所以具体为什么效率低不很清楚,下面的文档有说道什么压缩效率之类的)<br>
[类型问题2-将来取消,避免问题](https://www.elastic.co/guide/en/elasticsearch/reference/current/removal-of-types.html)
