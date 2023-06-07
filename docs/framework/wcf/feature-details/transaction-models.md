---
title: 事务模型
ms.date: 03/30/2017
ms.assetid: 48a8bc1b-128b-4cf1-a421-8cc73223c340
ms.openlocfilehash: ce7eb74a3e06df8db2e6d8261faca16650e4e4f1
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96242261"
---
# <a name="transaction-models"></a>事务模型

本主题描述 Microsoft 提供的事务编程模型和基础结构组件之间的关系。  
  
 使用 (WCF) Windows Communication Foundation 中的事务时，务必要了解不要在不同的事务模型之间进行选择，而是在集成且一致的模型的不同层上操作。  
  
 以下各部分描述了三个主要事务组件。  
  
## <a name="windows-communication-foundation-transactions"></a>Windows Communication Foundation 事务  

 WCF 中的事务支持可让你编写事务性服务。 此外，通过其对 WS-AtomicTransaction (WS-AT) 协议的支持，应用程序可以将事务流式传输到使用 WCF 或第三方技术生成的 Web 服务。  
  
 在 WCF 服务或应用程序中，WCF 事务功能提供了属性和配置，用于以声明方式指定基础结构创建、流动和同步事务的方式和时间。  
  
## <a name="systemtransactions-transactions"></a>System.Transactions 事务  

 <xref:System.Transactions> 命名空间同时提供了一个基于 <xref:System.Transactions.Transaction> 类的显式编程模型和一个使用 <xref:System.Transactions.TransactionScope> 类的隐式编程模型（在此模型中，基础结构自动管理事务）。  
  
 有关如何使用这两种模型创建事务应用程序的详细信息，请参阅 [编写事务应用程序](https://go.microsoft.com/fwlink/?LinkId=94947)。  
  
 在 WCF 服务或应用程序中， <xref:System.Transactions> 提供用于在客户端应用程序中创建事务的编程模型，并在需要时，在服务中显式与事务进行交互。  
  
## <a name="msdtc-transactions"></a>MSDTC 事务  

 Microsoft Distributed Transaction Coordinator (MSDTC) 是一个事务管理器，它为分布式事务提供支持。  
  
 有关详细信息，请参阅 [DTC 程序员参考](/previous-versions/windows/desktop/ms686108(v=vs.85))。  
  
 在 WCF 服务或应用程序中，MSDTC 提供基础结构，用于协调客户端或服务中创建的事务。
