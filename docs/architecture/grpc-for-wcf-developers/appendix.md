---
title: 附录-适用于 WCF 开发人员的 gRPC
description: 在新式微服务体系结构中讨论分布式事务及其实现。
ms.date: 09/02/2019
ms.openlocfilehash: f60899463a13e9f740f6ae63150d18eab3069124
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91165853"
---
# <a name="appendix-a---transactions"></a>附录 A-事务

Windows Communication Foundation (WCF) 支持分布式事务，使你能够跨多个服务执行原子操作。 此功能基于 [Microsoft 分布式事务处理协调器](/previous-versions/windows/desktop/ms684146(v=vs.85))。

在较新的微服务环境中，这种类型的自动分布式事务处理是不可能的。 涉及到许多不同的技术，其中包括关系数据库、NoSQL 数据存储和消息传送系统。 在单一环境中，可能还会混合使用多种操作系统、编程语言和框架。

WCF 分布式事务实现了所谓的 [两阶段提交 (2pc) ](https://en.wikipedia.org/wiki/Two-phase_commit_protocol)。 你可以通过协调跨服务的消息、在每个服务中创建打开的事务以及发送提交或回滚消息（具体取决于成功或失败）来手动实现2PC 事务。 但是，随着系统的发展，管理2PC 所涉及的复杂性可能会呈指数级增长。 打开事务保存可能对性能产生负面影响的数据库锁，更糟的是导致跨服务死锁。

如果可能，最好避免分布式事务。 如果数据的两个项都需要进行原子更新，请考虑将它们处理为同一个服务。 使用单个请求或消息向该服务应用这些原子更改。

如果无法做到这一点，一种替代方法是使用 [Saga 模式](https://microservices.io/patterns/data/saga.html)。 在 saga 中，更新按顺序处理;每个更新成功后，将触发下一个更新。 这些触发器可以从服务传播到服务，或由 saga 协调器或 orchestrator 进行管理。 如果在此过程中的任何时间点更新失败，则已完成其更新的服务会应用特定的逻辑来反转它们。

另一种选择是将域驱动设计 (DDD) 和命令/查询责任分离 (CQRS) ，如 [.Net 微服务电子书](../microservices/microservice-ddd-cqrs-patterns/index.md)中所述。 特别是，使用域事件或 [事件来源](https://martinfowler.com/eaaDev/EventSourcing.html) 有助于确保更新的一致性（如果不能立即应用）。

>[!div class="step-by-step"]
>[上一页](application-performance-management.md)
