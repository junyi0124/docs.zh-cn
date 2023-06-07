---
title: Windows Communication Foundation 事务概述
ms.date: 03/30/2017
helpviewer_keywords:
- transactions [WCF]
- WCF, transactions
- Windows Communication Foundation, transactions
ms.assetid: c7757854-1207-4019-8b31-552578b7d570
ms.openlocfilehash: 1486290241fdb40d415278c4a01738aa711e2182
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96261470"
---
# <a name="windows-communication-foundation-transactions-overview"></a>Windows Communication Foundation 事务概述

事务可提供一种分组方法，将一组操作分为单个不可分的执行单元。 事务是指具有下列属性的操作集合：  
  
- 原子性。 此属性可确保特定事务下完成的所有更新都已提交并保持持久，或所有这些更新都已中止并回滚到其先前状态。  
  
- 一致性。 此属性可保证某一事务下所做的更改表示从一种一致状态转换到另一种一致状态。 例如，将钱从支票帐户转移到存款帐户的事务并不改变整个银行帐户中的钱的总额。  
  
- 隔离。 此属性可防止事务遵循属于其他并发事务的未提交的更改。 隔离在确保一种事务不能对另一事务的执行产生意外的影响的同时，还提供一个抽象的并发。  
  
- 持续性。 这意味着一旦提交对托管资源（如数据库记录）的更新，即使出现失败这些更新也会保持持久。  
  
 Windows Communication Foundation (WCF) 提供了一组丰富的功能，使您能够在 Web 服务应用程序中创建分布式事务。  
  
 WCF 实现 WS-AtomicTransaction (WS-AT) 协议的支持，该协议使 WCF 应用程序能够将事务流式传输到可互操作的应用程序，如使用第三方技术生成的可互操作的 Web 服务。 WCF 还实现对 OLE 事务协议的支持，该协议可用于不需要互操作功能来启用事务流的情况。  
  
 你可以使用应用程序配置文件来配置绑定以启用或禁用事务流，以及设置有关绑定的所需事务协定。 此外，你可以使用配置文件在服务级别设置事务超时值。 有关详细信息，请参阅 [启用事务流](enabling-transaction-flow.md)。  
  
 <xref:System.ServiceModel> 命名空间中的事务属性允许你进行以下操作：  
  
- 使用 <xref:System.ServiceModel.ServiceBehaviorAttribute> 属性配置事务超时值以及隔离级别的筛选。  
  
- 启用事务功能并使用 <xref:System.ServiceModel.OperationBehaviorAttribute> 属性配置事务完成行为。  
  
- 使用协定方法上的 <xref:System.ServiceModel.ServiceContractAttribute> 和 <xref:System.ServiceModel.OperationContractAttribute> 属性来要求、允许或拒绝事务流。  
  
 有关详细信息，请 [参阅 "](servicemodel-transaction-attributes.md)"。  
  
## <a name="see-also"></a>另请参阅

- [ServiceModel 事务属性](servicemodel-transaction-attributes.md)
- [启用事务流](enabling-transaction-flow.md)
