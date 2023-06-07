---
title: 云本机应用程序中的 Elasticsearch
description: 了解如何将弹性搜索功能添加到云本机应用程序。
author: robvet
ms.date: 05/13/2020
ms.openlocfilehash: fa46f3387eecb3fccd63fdea10c11e92923ae862
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91155375"
---
# <a name="elasticsearch-in-a-cloud-native-app"></a>云本机应用中的 Elasticsearch

Elasticsearch 是一种分布式搜索和分析系统，可跨不同类型的数据实现复杂的搜索功能。 它是开源的，广泛流行。 考虑以下公司如何将 Elasticsearch 集成到其应用程序中：

- 当你键入) 搜索时，适用于全文和增量 (搜索的[维基百科](https://blog.wikimedia.org/2014/01/06/wikimedia-moving-to-elasticsearch/)。
- [GitHub](https://www.elastic.co/customers/github) 用于索引和公开超过8000000的代码存储库。  
- 用于使容器库可发现的[Docker](https://www.elastic.co/customers/docker) 。

Elasticsearch 基于 [Apache Lucene](https://lucene.apache.org/core/) 全文搜索引擎。 Lucene 提供高性能的文档索引和查询。 它使用反转索引方案对数据进行索引，而不是将页映射到关键字，而是将关键字映射到页面，就像本书末尾的术语表。 Lucene 具有强大的查询语法功能，可以通过以下方式查询数据：

- 术语 (完整的单词) 
- 前缀 (开始-带有 word) 
- 使用 " \* " 或 "？" 筛选器) 通配符 (
- 短语 (文档中的一系列文本) 
- 布尔值 (复杂搜索合并查询) 

虽然 Lucene 为搜索提供低级别的管道，但 Elasticsearch 提供了位于 Lucene 顶层的服务器。 Elasticsearch 添加了更高级别的功能以简化工作 Lucene，其中包括用于访问 Lucene 的索引和搜索功能的 RESTful API。 它还提供了一个能够提供巨大的可伸缩性、容错和高可用性的分布式基础结构。

对于更大的具有复杂搜索要求的云本机应用程序，Elasticsearch 可用作 Azure 中的托管服务。 Microsoft Azure 市场提供了预先配置的模板，开发人员可以使用这些模板在 Azure 上部署 Elasticsearch 群集。

在 Microsoft Azure 市场中，开发人员可以使用生成的预配置模板在 Azure 上快速部署 Elasticsearch 群集。 使用 Azure 托管产品，最多可部署50个数据节点、20个协调节点和三个专用主节点。

## <a name="summary"></a>总结

本章详细介绍了云本机系统中的数据。 我们在云本机系统中的数据存储模式与整体应用程序中的数据存储进行了对比。 我们查看了在云本机系统中实现的数据模式，其中包括跨服务查询、分布式事务和用于处理大容量系统的模式。 我们将 SQL 与 NoSQL 数据相对应。 我们查看了 Azure 中的可用数据存储选项，其中包括 Microsoft 中心和开源选项。 最后，我们在云本机应用程序中讨论了缓存和 Elasticsearch。

### <a name="references"></a>参考

- [命令和查询责任分离 (CQRS) 模式](/azure/architecture/patterns/cqrs)

- [事件来源模式](/azure/architecture/patterns/event-sourcing)

- [为什么在 CAP 定理中不能承受 RDBMS 分区，为什么它可用？](https://stackoverflow.com/questions/36404765/why-isnt-rdbms-partition-tolerant-in-cap-theorem-and-why-is-it-available)

- [具体化视图](/azure/architecture/patterns/materialized-view)

- [你确实需要知道开源数据库](https://www.ibm.com/blogs/systems/all-you-really-need-to-know-about-open-source-databases/)

- [补偿事务模式](/azure/architecture/patterns/compensating-transaction)

- [Saga 模式](https://microservices.io/patterns/data/saga.html)

- [Saga 模式 |如何使用微服务实现业务事务](https://blog.couchbase.com/saga-pattern-implement-business-transactions-using-microservices-part/)

- [补偿事务模式](/azure/architecture/patterns/compensating-transaction)

- [在9球后面：说明 Cosmos DB 一致性级别](https://blog.jeremylikness.com/blog/2018-03-23_getting-behind-the-9ball-cosmosdb-consistency-levels/)

- [探索不同类型的 NoSQL 数据库第 II 部分](https://www.3pillarglobal.com/insights/exploring-the-different-types-of-nosql-databases)

- [在 RDBMS、NoSQL 和 NewSQL 数据库上。与 John Ryan 访谈](http://www.odbms.org/blog/2018/03/on-rdbms-nosql-and-newsql-databases-interview-with-john-ryan/)
  
- [SQL vs NoSQL vs NewSQL：完全比较](https://www.xenonstack.com/blog/sql-vs-nosql-vs-newsql/)

- [破折号： Kubernetes 的四个属性-本机数据库](https://thenewstack.io/dash-four-properties-of-kubernetes-native-databases/)

- [CockroachDB](https://www.cockroachlabs.com/)

- [TiDB](https://pingcap.com/en/)

- [YugabyteDB](https://www.yugabyte.com/)

- [Vitess](https://vitess.io/)

- [Elasticsearch：权威指南](https://shop.oreilly.com/product/0636920028505.do)
  
- [Apache Lucene 简介](https://www.baeldung.com/lucene)

>[!div class="step-by-step"]
>[上一页](azure-caching.md)
>[下一页](resiliency.md) <!-- Next Chapter -->
