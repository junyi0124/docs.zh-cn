---
title: 事务处理
description: 查看 .NET 中的事务处理。 事务确保了面向数据的资源不会永久更新，除非所有操作都成功完成。
ms.date: 03/30/2017
ms.assetid: effdc8e6-accf-41eb-98a5-431603ba218b
ms.openlocfilehash: bbf2448128da7df8af415ff5dea7e3cd8af24d1e
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91186804"
---
# <a name="transaction-processing"></a>事务处理

当您从网上书店购买书籍时，会用钱（以信贷方式）来交换书籍。 如果您的信用良好，则一系列相关操作可确保您和书店可以相应地获得书籍和钱。 但如果在交换期间该系列操作中的单个操作发生故障，则整个交换就会失败。 结果，您就得不到书籍，而书店也得不到钱。  
  
 负责使该交换取得平衡且可预测的技术称为事务处理。 事务可确保仅当事务单位中的所有操作都已成功完成时，才会永久更新面向数据的资源。 通过将一组相关操作合并到不是完全成功就是完全失败的单位中，可以简化错误恢复并使应用程序更加可靠。  
  
 事务处理系统包括承载一个面向事务的应用程序的计算机软硬件，该应用程序执行开展业务所需的例行事务。 例如，管理销售订单录入、机票预订、工资单、员工记录、生产和装运的系统。  
  
 本节提供有关事务处理的常见信息，以及有关如何使用 Microsoft .NET Framework 编写事务应用程序和资源管理器的特定信息。  
  
## <a name="in-this-section"></a>本节内容  

 [事务基础知识](transaction-fundamentals.md)  
 引入了基本的事务处理术语和概念。  
  
 [由 System.Transactions 提供的功能](features-provided-by-system-transactions.md)  
 讨论如何使用 System.Transactions 中的功能编写您自己的事务应用程序。  
  
## <a name="reference"></a>参考  

 <xref:System.Transactions>  
 提供用于使您的代码参与事务的类。 这些类支持的事务可使用多个分布式参与者、多阶段通知和持久登记。
