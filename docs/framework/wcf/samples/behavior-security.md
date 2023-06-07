---
title: 行为安全
ms.date: 03/30/2017
ms.assetid: 19710ae3-f197-4d28-ba9d-52e465006819
ms.openlocfilehash: c06c615af773affe9b824c6a862afcbadfb16295
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96258733"
---
# <a name="behavior-security"></a>行为安全

本节包含演示服务行为的配置安全的示例。  
  
## <a name="in-this-section"></a>本节内容  

 [服务审核行为](service-auditing-behavior.md)  
 此示例演示如何在服务操作过程中使用 <xref:System.ServiceModel.Description.ServiceSecurityAuditBehavior> 来启用对安全事件的审核。  
  
 [成员资格和角色提供程序](membership-and-role-provider.md)  
 此示例演示服务如何使用 ASP.NET 成员资格和角色提供程序来对客户端进行身份验证和授权。  
  
 [授予对服务操作的访问权限](authorizing-access-to-service-operations.md)  
 此示例演示如何使用 [\<serviceAuthorization>](../../configure-apps/file-schema/wcf/serviceauthorization-element.md) 来允许使用 <xref:System.Security.Permissions.PrincipalPermissionAttribute> 属性来授予对服务操作的访问权限。  
  
 [模拟客户端](impersonating-the-client.md)  
 此示例演示如何在服务中模拟调用方应用程序，以便服务可以代表调用方访问系统资源。
