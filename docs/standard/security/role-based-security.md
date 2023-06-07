---
title: 基于角色的安全性
ms.date: 07/15/2020
helpviewer_keywords:
- role-based security, about role-based security
- user authentication, principals
- principals [.NET]
- security [.NET], role-based
- permissions [.NET], principals
- authentication [.NET], principals
- role-based security, principals
ms.assetid: 578cc32b-5654-4d8b-9d8c-ebcbc5c75390
ms.openlocfilehash: a03cda3aac06cc247818ccea5c61c673225d7929
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94824130"
---
# <a name="role-based-security"></a>基于角色的安全性

在财务或业务应用程序中经常使用角色来强制执行策略。 例如，应用程序可能会根据发出请求的用户是否是指定角色的成员，以限制其所处理事务的大小。 职员处理事务的授权可能低于所指定阈值，主管可能具有更高的限制，而副总裁可能具有比主管更高的限制（或无限制）。 当应用程序需要多项审批来完成操作时，也可以使用基于角色的安全性。 这种情况可能发生在采购系统中，任何员工均可生成采购请求，但只有采购代理可以将此请求转换成可发送给供应商的采购订单。  
  
 基于 .NET 角色的安全性通过提供有关主体的信息来支持授权，该主体是从关联的标识构造的，可用于当前线程。 标识（和其帮助定义的主体）可以基于 Windows 帐户，或可以为与 Windows 帐户无关的自定义标识。 .NET 应用程序可以基于主体的标识或角色成员资格做出授权决策，也可以同时进行这两项决策。 角色是指在安全性方面具有相同特权的一组命名主体（如出纳或经理）。 主体可以是一个或多个角色的成员。 因此，应用程序可以使用角色成员身份来确定主体是否有权执行所请求的操作。  
  
 为了与代码访问安全性实现易用性和一致性，.NET 基于角色的安全性提供 <xref:System.Security.Permissions.PrincipalPermission?displayProperty=nameWithType> 使公共语言运行时能够以类似于代码访问安全检查的方式执行授权的对象。 <xref:System.Security.Permissions.PrincipalPermission> 类表示主体必须与之匹配，并且与两种声明性和命令性安全检查相兼容的标识和角色。 还可以直接访问主体的标识信息，并在需要时在代码中执行角色和标识检查。  
  
 .NET 提供了基于角色的安全性支持，它具有足够的灵活性和可扩展性，足以满足各种应用程序的需求。 你可以选择与现有身份验证基础结构进行交互操作（如 COM+ 1.0 服务）或创建自定义身份验证系统。 基于角色的安全性非常适合在主要在服务器上进行处理 ASP.NET Web 应用程序中使用。 但是，可以在客户端或服务器上使用基于 .NET 角色的安全性。  
  
 阅读本部分之前，请确保了解 [关键安全概念](key-security-concepts.md)中介绍的内容。  
  
## <a name="see-also"></a>另请参阅
  
- [主体和标识对象](principal-and-identity-objects.md)
- [安全性的基础概念](key-security-concepts.md)
- <xref:System.Security.Permissions.PrincipalPermission?displayProperty=nameWithType>
- [ASP.NET Core 安全性](/aspnet/core/security/)
