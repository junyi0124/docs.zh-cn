---
title: 每个微服务的数据主权
description: 每个微服务的数据主权是微服务的核心内容之一。 每个微服务都必须是其数据库的唯一所有者，不能与任何其他微服务共享该数据库。 当然，某个微服务的所有实例都连接到同一个高可用性数据库。
ms.date: 09/20/2018
ms.openlocfilehash: f606d6314f38bf3e2c163871af432806dddc7446
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/14/2020
ms.locfileid: "73191919"
---
# <a name="data-sovereignty-per-microservice"></a>每个微服务的数据主权

微服务体系结构的重要规则是每个微服务必须拥有其域数据和逻辑。 就像一个完整的应用程序拥有其逻辑和数据一样，每个微服务也须在其自治生命周期内拥有其逻辑和数据，且每个微服务都必须独立部署。

这意味着，域的概念模型因子系统或微服务而异。 以企业应用程序为例，其中客户关系管理 (CRM) 应用程序、交易购买子系统和客户支持子系统各自调用唯一客户实体属性和数据，并采用不同的界定上下文 (BC)。

此原则类似于[域驱动设计 (DDD)](https://en.wikipedia.org/wiki/Domain-driven_design)，其中每个[界定上下文](https://martinfowler.com/bliki/BoundedContext.html)或者自治子系统或服务必须拥有其域模型（数据加上逻辑和行为）。 每个 DDD 界定上下文对应于一个业务微服务（一个或多个服务）。 关于界定上下文模式的这一点将在下一部分展开讨论。

另一方面，在许多应用程序中使用的传统（整体式数据）方法具有单个集中式的数据库或只需少量的数据库。 这通常是用于整个应用程序及其所有内部子系统的规范化 SQL 数据库，如图 4-7 中所示。

![显示两种数据库方法的示意图。](./media/data-sovereignty-per-microservice/data-sovereignty-comparison.png)

图 4-7  。 数据主权比较：整体式数据库与微服务

在传统方法中，所有服务都共享一个数据库，并且通常采用分层体系结构。 在微服务方法中，每个微服务都拥有自己的模型/数据。 集中式数据库方法初看来比较简单，似乎能够在不同的子系统中重复使用实体，从而保持所有对象的一致性。 但实际上，你最终会得到为许多不同的子系统提供服务的大型表格，其中包括大多数情况下不需要的属性和列。 这相当于在进行短途徒步旅行、一天自驾游和学习地理知识时使用同一张自然地图。

通常具有一个关系数据库的单片式应用程序可提供两个重要的优势：[ACID 事务](https://en.wikipedia.org/wiki/ACID)和 SQL 语言，它们均可以用于所有与应用程序相关的表和数据。 此方法可轻松编写从多张表格合并数据的查询。

但是，当转到微服务体系结构时，数据访问会变得更为复杂。 即使在微服务或绑定上下文中使用 ACID 事务时，也务必要考虑到，每个微服务拥有的数据都专用于该微服务，并且只应通过其 API 终结点（REST、gRPC、SOAP 等）进行同步访问，或通过消息传递（AMQP 或类似方法）进行异步访问。

封装该数据可确保微服务松散耦合，且可彼此独立地进行演变。 如果多个服务访问相同的数据，架构更新将需要针对所有服务的协调更新。 这会破坏微服务生命周期自治。 但分布式的数据结构意味着无法跨微服务进行单个 ACID 事务。 这反过来意味着当业务流程跨多个微服务时，必须使用最终一致性。 这比简单的 SQL 联接更难实现，因为无法在不同的数据库之间创建完整性约束或使用分布式事务，我们稍后会对此进行解释。 同样，许多其他关系数据库功能在多个微服务间将不可用。

进一步来说，不同的微服务通常使用不同类型的数据库  。 现代应用程序存储和处理不同类型的数据，并且关系数据库并不总是最佳选择。 对于某些用例，相较于 SQL 数据库（如 SQL Server 或 Azure SQL 数据库），NoSQL 数据库（如 Azure CosmosDB 或 MongoDB）可能具有更为方便的数据模型，并且提供更好的性能和可伸缩性。 其他情况下，关系数据库仍是最佳方法。 因此，基于微服务的应用程序通常混合使用 SQL 和 NoSQL 数据库，这有时称为[多语言持久性](https://martinfowler.com/bliki/PolyglotPersistence.html)方法。

数据存储的分区式多语言持久性体系结构带来诸多好处。 其中包括松散耦合的服务和更好的性能、可伸缩性、可管理性及更低的成本。 但是，它也会带来一些分布式数据管理难题，如本章后面的“[标识域模型边界](identify-microservice-domain-model-boundaries.md)”中所述。

## <a name="the-relationship-between-microservices-and-the-bounded-context-pattern"></a>微服务和界定上下文模式之间的关系

微服务的概念源自[域驱动设计 (DDD)](https://en.wikipedia.org/wiki/Domain-driven_design) 中的[界定上下文 (BC) 模式](https://martinfowler.com/bliki/BoundedContext.html)。 DDD 将大模型划分为多个 BC 并对其边界十分明确，以此处理大模型。 每个 BC 必须具有其自己的模型和数据库；同样，每个微服务拥有其相关的数据。 此外，每个 BC 通常具有其自己的[通用语言](https://martinfowler.com/bliki/UbiquitousLanguage.html)，从而帮助软件开发人员和域专业人员之间进行通信。

通用语言中的这些术语（主要是域实体）在不同的界定上下文中具有不同的名称，即使不同域实体共享相同的标识（即，用于从存储读取实体的唯一 ID）也是如此。 例如，在用户配置文件界定上下文中，用户域实体可能与买家域实体在订购界定上下文中共享标识。

因此微服务类似于界定上下文，但它还指定它是一个分布式服务。 它针对每个界定上下文作为单独进程而构建，且必须使用前文所述的分布式协议，如 HTTP/HTTPS、WebSocket 或 [AMQP](https://en.wikipedia.org/wiki/Advanced_Message_Queuing_Protocol)。 但是，界定上下文模式不指定界定上下文是分布式服务，还是只是整体式部署应用程序内的一个逻辑边界（如常规子系统）。

务必强调，定义每个界定上下文的服务是一个不错的开端。 但你无需将设计限定于此。 有时必须设计含多个物理服务的界定上下文或业务微服务。 但从根本上讲，界定上下文和微服务这两个模式密切相关。

DDD 通过获取分布式微服务形式的实际边界而从微服务中获益。 但是，不共享微服务间的模型这类想法也适用于界定上下文。

### <a name="additional-resources"></a>其他资源

- **Chris Richardson.模式：每个服务一个数据库** \
  <https://microservices.io/patterns/data/database-per-service.html>

- **Martin Fowler。BoundedContext** \
  <https://martinfowler.com/bliki/BoundedContext.html>

- **Martin Fowler。PolyglotPersistence** \
  <https://martinfowler.com/bliki/PolyglotPersistence.html>

- **Alberto Brandolini。含上下文映射的策略域驱动设计** \
  <https://www.infoq.com/articles/ddd-contextmapping>

>[!div class="step-by-step"]
>[上一页](microservices-architecture.md)
>[下一页](logical-versus-physical-architecture.md)
