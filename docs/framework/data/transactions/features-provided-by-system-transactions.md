---
title: 由 System.Transactions 提供的功能
description: 查看 .NET 中的 System. transaction 命名空间提供的功能，以编写您自己的事务应用程序和资源管理器。
ms.date: 03/30/2017
ms.assetid: e458cef9-63b5-4401-b448-1536dcd9d9e5
ms.openlocfilehash: 27c6224530b4faa1ada93db8147f9334f38b93ab
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91182917"
---
# <a name="features-provided-by-systemtransactions"></a>由 System.Transactions 提供的功能

本节描述如何使用 <xref:System.Transactions> 命名空间所提供的功能编写您自己的事务应用程序和资源管理器。 具体来说，本节介绍如何创建事务（本地或分布式事务）并与一个或多个参与者一起参与该事务。  
  
## <a name="overview-of-systemtransactions"></a>System.Transactions 概述  

 <xref:System.Transactions> 命名空间中的类所提供的基础结构通过支持在 SQL Server、ADO.NET、消息队列 (MSMQ) 和 Microsoft 分布式事务协调器 (MSDTC) 中启动的事务，使事务编程变得简单和高效。 <xref:System.Transactions> 命名空间提供基于 <xref:System.Transactions.Transaction> 类的显式编程模型和使用 <xref:System.Transactions.TransactionScope> 类的隐式编程模型，在后一种模型中，事务由该基础结构自动管理。 有关如何使用这两种模型创建事务应用程序的详细信息，请参阅 [编写事务应用程序](writing-a-transactional-application.md)。  
  
 此外，<xref:System.Transactions> 命名空间还提供了用于实现资源管理器的类型。 资源管理器管理事务中使用的持久或可变数据，并与事务管理器协调工作，共同为应用程序提供了原子性和隔离性的保证。 由 <xref:System.Transactions> 基础结构提供的事务管理器支持的事务可涉及到多个可变资源或单个持久资源。 有关实现资源管理器的详细信息，请参阅 [实现资源管理器](implementing-a-resource-manager.md)。  
  
 此外，事务管理器还通过与基于磁盘的事务管理器（如 DTC）进行协调，透明地将本地事务升级为分布式事务，此时另一个持久资源管理器会将自身登记到事务中。 <xref:System.Transactions> 基础结构提供增强性能的关键方式有两种。  
  
- 动态升级，它可确保事务跨多个分布式资源时 <xref:System.Transactions> 基础结构仅使用 MSDTC。 有关动态升级的更多信息。 请参阅 [事务管理升级](transaction-management-escalation.md) 主题。  
  
- 可提升的登记，如果某个资源是参与事务的唯一实体，则允许该资源（如数据库）取得事务的所有权。 以后在需要时，<xref:System.Transactions> 基础结构仍然可以将事务管理升级到 MSDTC。 这样进一步减少了使用 MSDTC 的机会。 [使用单阶段提交和可提升的单阶段通知](optimization-spc-and-promotable-spn.md)中的主题优化深入介绍了可提升的登记。  
  
 <xref:System.Transactions> 命名空间定义了三种限制访问它所公开的资源类型的信任级别：AllowPartiallyTrustedCallers (APTCA)、DistributedTransactionPermission(DTP) 和完全信任。 有关各种信任级别的详细信息，请参阅 [访问资源中的安全信任级别](security-trust-levels-in-accessing-resources.md)。  
  
## <a name="in-this-section"></a>在本节中  
  
### <a name="writing-a-transactional-application"></a>编写事务应用程序  

 <xref:System.Transactions> 命名空间提供了两种用于创建事务应用程序的模型。 [使用事务范围实现隐式事务](implementing-an-implicit-transaction-using-transaction-scope.md) 描述 <xref:System.Transactions> 命名空间如何支持使用类创建隐式事务 <xref:System.Transactions.TransactionScope> 。  
  
 [使用 CommittableTransaction 实现显式事务](implementing-an-explicit-transaction-using-committabletransaction.md) 描述 <xref:System.Transactions> 命名空间如何支持使用类创建显式事务 <xref:System.Transactions.CommittableTransaction> 。  
  
 有关涉及编写事务应用程序的其他主题，请参阅 [编写事务应用程序](writing-a-transactional-application.md)。  
  
### <a name="implementing-a-resource-manager"></a>实现资源管理器  

 若要实现可参与事务的资源管理器，请参阅 [实现资源管理器](implementing-a-resource-manager.md)。 该节将介绍资源登记、事务提交、故障后恢复以及最佳优化实践。
