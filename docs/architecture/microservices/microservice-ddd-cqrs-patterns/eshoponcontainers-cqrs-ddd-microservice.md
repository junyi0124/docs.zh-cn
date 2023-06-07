---
title: 在 eShopOnContainers 的 DDD 微服务中应用 CQRS 和 CQS 方法
description: 适用于容器化 .NET 应用程序的 .NET 微服务体系结构 | 了解在 eShopOnContainers 中的订购微服务中实现 CQRS 的方式。
ms.date: 03/03/2020
ms.openlocfilehash: 2916df596a6d0f887411f3ef0074aed395ef58ba
ms.sourcegitcommit: 5280b2aef60a1ed99002dba44e4b9e7f6c830604
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/03/2020
ms.locfileid: "84306937"
---
# <a name="apply-cqrs-and-cqs-approaches-in-a-ddd-microservice-in-eshoponcontainers"></a>在 eShopOnContainers 的 DDD 微服务中应用 CQRS 和 CQS 方法

eShopOnContainers 引用应用程序处的订单微服务设计基于 CQRS 原则。 但是，它使用最简单的方法：只需将查询与命令分离，且执行这两种操作时使用相同的数据库。

这些模式的本质，以及这里的要点是，查询是幂等的：无论查询一个系统多少次，该系统的状态都不会改变。 换而言之，查询没有副作用。

因此，可以使用不同的“读取”数据模型而不是事务逻辑的“写入”域模型，尽管排序微服务使用的是相同的数据库。 因此，这是简化的 CQRS 方法。

另一方面，用于触发事务和数据更新的命令在系统中会更改状态。 在处理复杂问题和不断变化的业务规则的过程中，使用命令时需小心谨慎。 这正是应用 DDD 技术来实现更好的模型化系统的时机。

本指南中介绍的 DDD 模式并不是普遍适用的。 它们会在设计中引入约束。 这些约束具有一些优点，比如可随时间推移不断提升质量，特别是命令和用于修改系统状态的其他代码的质量。 但是这些约束在数据读取和查询方面的没有太多用处，反而会增加复杂性。

这种模式中的一种是聚合模式，会在后面几部分中进行详细介绍。 简单来说，在聚合模式中，因多个域对象在域中的关系，会将这些域对象作为一个单位来对待。 在查询中，此模式也可能产生不便，它可能会增加查询逻辑的复杂性。 对于只读查询，无法获得将多个对象视为单个聚合的优势。 而只会增加复杂性。

如前一部分中的图 7-2 所示，本指南建议仅在微服务的事务性/更新区域中使用 DDD 模式（也即由命令触发时）。 查询可以遵循更简单的方法，如果遵循 CQRS 方法，应将查询与命令分离开来。

要实现“查询端”，成熟的 ORM 中有许多方法可供选择：EF Core、AutoMapper 投影、存储过程、视图、具体化视图或微型 ORM。

在本指南和 eShopOnContainers （尤其是订单微服务）中，使用 [Dapper](https://github.com/StackExchange/dapper-dot-net) 等微型 ORM 实现直接查询。 得益于低开销的轻量级框架，这样可基于 SQL 语句实现任何查询以获得最佳性能。

使用此方法时，如果对模型进行任何更新，且该更新会影响实体持久保存到 SQL 数据库的方式，那么同时需要 对 Dapper 使用的或任何其他单独的查询方法（非 EF）使用的 SQL 查询进行单独更新。

## <a name="cqrs-and-ddd-patterns-are-not-top-level-architectures"></a>CQRS 和 DDD 模式不是顶层体系结构

须知 CQRS 和大多数 DDD 模式（如 DDD 层或带有聚合的域模型）不是体系结构样式，而只是体系结构模式。 体系结构样式的例子包括微服务、SOA 和事件驱动的体系结构 (EDA)。 它们描述由许多组件（如许多微服务）构成的系统。 而 CQRS 和 DDD 模式描述单个系统或组件内的某项内容，在本例中，也即微服务内的某项内容。

不同的界定上下文 (BC) 采用不同的模式。 它们具有不同的功能，会形成不同的解决方案。 这里需强调的是，在任何情况下都使用相同模式会导致失败。 请勿滥用 CQRS 和 DDD 模式。 有许多更简单的子系统、BC 或微服务，可使用简单的 CRUD 服务或其他方法更轻松地实现。

只有一个应用程序体系结构：你设计的系统或端到端应用程序的体系结构（如微服务体系结构）。 但该应用程序中的每个界定的上下文或微服务的设计反映有关其自身的折衷方案和体系结构模式级别的内部设计决策。 请勿尝试将相同的体系结构模式（如 CQRS 或 DDD）用于所有情况。

### <a name="additional-resources"></a>其他资源

- **Martin Fowler。CQRS** \
  <https://martinfowler.com/bliki/CQRS.html>

- **Greg Young.CQRS 文档** \
  <https://cqrs.files.wordpress.com/2010/11/cqrs_documents.pdf>

- **Udi Dahan.明确的 CQRS** \
  <https://udidahan.com/2009/12/09/clarified-cqrs/>

>[!div class="step-by-step"]
>[上一页](apply-simplified-microservice-cqrs-ddd-patterns.md)
>[下一页](cqrs-microservice-reads.md)
