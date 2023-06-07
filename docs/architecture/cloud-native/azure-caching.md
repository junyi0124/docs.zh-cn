---
title: 缓存在云本机应用程序中
description: 了解云本机应用程序中的缓存策略。
author: robvet
ms.date: 05/17/2020
ms.openlocfilehash: 84860ad4583e4b45d5ca9490d9f0167e7439d3d4
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91161082"
---
# <a name="caching-in-a-cloud-native-app"></a>在云本机应用中缓存

缓存的优点非常好理解。 此方法的工作方式是将经常访问的数据从后端数据存储区暂时复制到靠近应用程序的 *快速存储* 。 缓存通常是在 .。。

- 数据保持相对静态。
- 数据访问速度较慢，尤其与缓存速度相比。
- 数据受到高级别争用的限制。

## <a name="why"></a>为什么？

如 [Microsoft 缓存指南](/azure/architecture/best-practices/caching)中所述，缓存可提高单个微服务和整个系统的性能、可伸缩性和可用性。 它减少了处理对数据存储的大量并发请求的延迟和争用。 随着数据量和用户数量的增加，缓存的优点也就越大。

当客户端重复读取不可变或不经常更改的数据时，缓存最有效。 示例包括引用信息，如产品和定价信息，或构建成本高昂的共享静态资源。

尽管微服务应是无状态的，但当绝对需要时，分布式缓存可支持对会话状态数据进行并发访问。

还应考虑进行缓存以避免重复计算。 如果操作会转换数据或执行复杂计算，请缓存后续请求的结果。

## <a name="caching-architecture"></a>缓存体系结构

云本机应用程序通常实现分布式缓存结构。 缓存是作为基于云的 [后备服务](./definition.md#backing-services)托管的，独立于微服务。 图5-15 显示了此体系结构。

![云本机应用中的缓存](media/caching-in-a-cloud-native-app.png)

**图 5-15**：云本机应用中的缓存

在上图中，请注意缓存如何独立于微服务并由其共享。 在此方案中，通过 [API 网关](./front-end-communication.md)调用缓存。 如第4章所述，该网关充当所有传入请求的前端。 分布式缓存会尽可能返回缓存的数据，从而提高系统的响应能力。 此外，从服务中分离缓存允许缓存单独进行扩展或缩减以满足增加的流量需求。

上图显示了称为 [缓存端模式](/azure/architecture/patterns/cache-aside)的常见缓存模式。 对于传入请求，先查询缓存 (步骤 \# 1) 以获取响应。 如果找到，则立即返回数据。 如果缓存中不存在数据 (称为 [缓存未命中](https://www.techopedia.com/definition/6308/cache-miss)) ，则会从下游服务 (步骤 2) 中的本地数据库检索数据 \# 。 然后，将其写入缓存，以便以后请求 (第 \# 3 步) ，并将其返回给调用方。 必须小心，以便定期逐出缓存的数据，以便系统保持及时和一致。

随着共享缓存的增长，在多个节点上对数据进行分区可能会有好处。 这样做可帮助最大程度地减少争用并提高可伸缩性。 许多缓存服务支持动态添加和删除节点，以及跨分区重新平衡数据。 此方法通常涉及群集。 聚类分析将联合节点集合作为无缝单一缓存公开。 但是，在内部，数据按预定义的分发策略分散到各个节点，以均衡负载。

## <a name="azure-cache-for-redis"></a>用于 Redis 的 Azure 缓存

[适用于 Redis 的 Azure 缓存](https://azure.microsoft.com/services/cache/) 是一种安全的数据缓存和消息代理服务，由 Microsoft 全面管理。 用作一种平台即服务 (PaaS) 产品/服务，提供高吞吐量和低延迟的数据访问。 Azure 内部或外部的任何应用程序都可以访问该服务。

Azure Cache for Redis 服务管理对跨 Azure 数据中心托管的开放源代码 Redis 服务器的访问。 该服务充当提供管理、访问控制和安全性的外观。 服务本身支持一组丰富的数据结构，包括字符串、哈希、列表和集。 如果你的应用程序已使用 Redis，则它将按原样使用 Azure Cache for Redis。

适用于 Redis 的 Azure 缓存大于简单缓存服务器。 它可以支持多种方案来增强微服务的体系结构：

- 内存中数据存储区
- 分布式非关系数据库
- 消息代理
- 配置或发现服务器
  
对于高级方案，可以将缓存数据的副本 [持久保存到磁盘](/azure/azure-cache-for-redis/cache-how-to-premium-persistence)。 如果灾难性事件同时禁用主缓存和副本缓存，则会从最新快照重新构造缓存。

Azure Redis 缓存可用于许多预定义的配置和定价层。 [高级层](/azure/azure-cache-for-redis/cache-overview#service-tiers)的功能包括多种企业级功能，如群集、数据持久性、异地复制和虚拟网络隔离。

>[!div class="step-by-step"]
>[上一页](relational-vs-nosql-data.md)
>[下一页](elastic-search-in-azure.md)
