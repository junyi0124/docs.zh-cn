---
title: 使用 WS-AtomicTransaction
ms.date: 03/30/2017
helpviewer_keywords:
- WS-AT protocol [WCF]
ms.assetid: 04a4c200-0af0-4c5d-a3d9-87cb7339e054
ms.openlocfilehash: 22b84dc49ab723953ce36402ac14221f410dda11
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96281594"
---
# <a name="using-ws-atomictransaction"></a>使用 WS-AtomicTransaction

WS-AtomicTransaction (WS-AT) 是一种可互操作的事务协议。 它使您能够使用 Web 服务消息对分布式事务进行流处理并以可互操作的方式在异类事务基础结构之间进行协调。 WS-AT 使用两阶段提交协议在分布式应用程序、事务管理器和资源管理器之间驱动原子结果的生成。  
  
  (WCF) 提供的 WS-AT 实现 Windows Communication Foundation 提供的协议服务内置于 Microsoft 分布式事务处理协调器 (MSDTC) 事务管理器中。 使用 WS-AT，WCF 应用程序可将事务流式传输到其他应用程序，包括使用第三方技术生成的可互操作的 Web 服务。  
  
 在客户端应用程序和服务器应用程序之间流动事务时，使用的事务协议由服务器在终结点上公开的绑定确定，而该终结点由客户端选择。 某些 WCF 系统提供的绑定默认为将 `OleTransactions` 协议指定为事务传播格式，而另一些则默认为指定 ws-at。 您也可以以编程方式修改给定绑定内所选的事务协议。  
  
 协议的选择可影响以下内容：  
  
- 使事务从客户端流动到服务器所使用的消息头的格式。  
  
- 用于在客户端的事务管理器和服务器的事务之间运行两阶段提交协议以便解析事务结果的网络协议。  
  
 如果使用 WCF 编写服务器和客户端，则无需使用 WS-AT。 可以改为使用 `NetTcpBinding` 的默认设置并启用 `TransactionFlow` 属性，此设置将使用 `OleTransactions` 协议。 有关详细信息，请参阅 [\<netTcpBinding>](../../configure-apps/file-schema/wcf/nettcpbinding.md)。 否则，如果你要使事务流动到使用第三方技术生成的 Web 服务，则必须使用 WS-AT。  
  
## <a name="see-also"></a>另请参阅

- [配置 WS-Atomic 事务支持](configuring-ws-atomic-transaction-support.md)
