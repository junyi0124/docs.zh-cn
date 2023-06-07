---
title: WCF 中的授权
ms.date: 03/30/2017
helpviewer_keywords:
- authorization [WCF]
- security [WCF], authorization
ms.assetid: 8ea0b552-af65-45b0-a157-c6c111b8ce5e
ms.openlocfilehash: 67da01508fbb8f14b6405b79445bdef297e63288
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96247481"
---
# <a name="authorization-in-wcf"></a>WCF 中的授权

授权是控制对资源（例如服务或文件）的访问和权限的过程。 本部分中的主题介绍如何以各种方式在 Windows Communication Foundation (WCF) 中执行此基本任务。  
  
## <a name="in-this-section"></a>本节内容  

 [访问控制机制](access-control-mechanisms.md)  
 简要概述 WCF 中的授权机制，并提供建议的用法。  
  
 [如何：使用 PrincipalPermissionAttribute 类限制访问](../how-to-restrict-access-with-the-principalpermissionattribute-class.md)  
 演示使用 <xref:System.Security.Permissions.PrincipalPermissionAttribute> 限制对服务的访问的过程。  
  
 [如何：将 ASP.NET 角色提供程序与服务一起使用](how-to-use-the-aspnet-role-provider-with-a-service.md)  
 演练服务的配置，使其能够使用 ASP.NET 的角色提供程序功能。  
  
 [如何：将 ASP.NET 授权管理器角色提供程序与服务一起使用](how-to-use-the-aspnet-authorization-manager-role-provider-with-a-service.md)  
 ASP.NET 可以使用授权管理器来管理网站的授权。 WCF 可以同样利用 ASP.NET/Authorization Manager 组合来授权客户端。  
  
 [使用标识模型管理声明和授权](managing-claims-and-authorization-with-the-identity-model.md)  
 说明将标识模型基础结构用于基于声明的授权的基础知识。  
  
 [委托和模拟](delegation-and-impersonation-with-wcf.md)  
 说明委托和模拟之间的区别。  
  
## <a name="reference"></a>参考  

 <xref:System.ServiceModel.Security>  
  
 <xref:System.ServiceModel.Description.PrincipalPermissionMode>  
  
 <xref:System.ServiceModel.Description.ServiceAuthorizationBehavior>  
  
 <xref:System.Security.Permissions.PrincipalPermissionAttribute>  
  
## <a name="related-sections"></a>相关章节  

 [身份验证](authentication-in-wcf.md)  
  
## <a name="see-also"></a>另请参阅

- [安全性概述](security-overview.md)
- [Windows Server App Fabric 的安全模型](/previous-versions/appfabric/ee677202(v=azure.10))
