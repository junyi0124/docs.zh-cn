---
title: 事务基础知识
description: 查看 .NET 中的事务基础知识。 所有事务都必须拥有基本 ACID 属性， (原子、一致、隔离和持久) 。
ms.date: 03/30/2017
ms.assetid: 353f4ee2-e6bf-4b1c-b1c8-385fc8a486c0
ms.openlocfilehash: 5efc778d6c50c16ac7335ed4d624dbd03d26a2e1
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91147380"
---
# <a name="transaction-fundamentals"></a>事务基础知识

事务将多个任务绑定在一起。 例如，假设应用程序执行两个任务。 首先，它在数据库中创建一个新表。 然后，它调用一个专用对象，来收集数据、设置数据格式以及将数据插入新表中。 这两个任务是相关的，甚至是相互依赖的，以便只有在可以用数据填充表时才创建新表。 在单个事务范围内执行这两个任务时，会在它们之间强制建立连接。 如果第二个任务失败，则第一个任务会回滚到创建新表之前的点。  
  
 若要确保发生可预测的行为，所有事务都必须具有基本的 ACID 属性（原子性、一致性、隔离性和持久性）。 这些属性可以确保关键任务事务能够做出全是或全非的主张。 有关 ACID 的详细信息，请参阅 [Acid 属性](/windows/win32/cossdk/acid-properties)。 总之，ACID 可保证一组相关任务作为一个整体成功或失败。 用事务处理术语来说，就是事务不是提交就是中止。 对于要提交的事务，所有参与者都必须保证对数据的任何更改都是永久的。 即使发生系统崩溃或其他不可预见的事件，更改也必须是永久的。 只要一个参与者无法做出这项保证，整个事务就会失败， 并且对事务范围内的数据的所有更改都会回滚到特定的设定点。  
  
 事务操作可限定于单个数据资源，如数据库或消息队列。 在这种情况下，本地事务由 <xref:System.Transactions> 所提供的可提升性能的事务管理器管理。 当这些事务由数据资源控制时，它们具有高效性并易于管理。  
  
 事务也可跨多个数据资源。 使用分布式事务可以将在不同系统上执行的多种不同的操作合并到一个通过或失败的操作中。 在这种情况下，事务由位于每个系统中的 Microsoft 分布式事务协调器 (MSDTC) 进行协调。  
  
 在使用 <xref:System.Transactions> 所提供的类开发事务应用程序时，不必考虑需要使用哪种事务，也不必考虑所涉及的事务管理器。 <xref:System.Transactions> 基础结构会为您自动管理这些事宜。  
  
 在创建事务时，可以指定应用于该事务的隔离级别。 由枚举定义的隔离级别 <xref:System.Transactions.IsolationLevel> 确定了其他事务对受事务影响的数据所拥有的访问级别。  
  
 您可以使用 ADO.NET、 <xref:System.EnterpriseServices> 或由命名空间提供的事务性编程模型来创建事务 <xref:System.Transactions> 。 [System.object 提供的功能](features-provided-by-system-transactions.md)主题讨论了可用于使用命名空间编写事务应用程序的功能 <xref:System.Transactions> 。  
  
## <a name="see-also"></a>请参阅

- [由 System.Transactions 提供的功能](features-provided-by-system-transactions.md)
