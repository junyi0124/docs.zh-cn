---
title: 如何：创建自定义主体标识
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- IPrincipal
- IAuthorizationPolicy
- PrincipalPermissionMode
- PrincipalPermissionAttribute
ms.assetid: c4845fca-0ed9-4adf-bbdc-10812be69b61
ms.openlocfilehash: 6c50d6b0ac2baa2dc61431af4afb8dca3860456a
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96249301"
---
# <a name="how-to-create-a-custom-principal-identity"></a>如何：创建自定义主体标识

<xref:System.Security.Permissions.PrincipalPermissionAttribute> 是一种控制对服务方法进行访问的声明性方式。 当使用此属性时，<xref:System.ServiceModel.Description.PrincipalPermissionMode> 枚举指定执行授权检查的模式。 当此模式设置为 <xref:System.ServiceModel.Description.PrincipalPermissionMode.Custom> 时，用户可以使用它指定一个由 <xref:System.Security.Principal.IPrincipal> 属性返回的自定义 <xref:System.Threading.Thread.CurrentPrincipal%2A> 类。 本主题演示的是将 <xref:System.ServiceModel.Description.PrincipalPermissionMode.Custom> 与自定义授权策略和自定义主体结合使用的方案。  
  
 有关使用的详细信息 <xref:System.Security.Permissions.PrincipalPermissionAttribute> ，请参阅 [如何：使用 PrincipalPermissionAttribute 类限制访问权限](../how-to-restrict-access-with-the-principalpermissionattribute-class.md)。  
  
## <a name="example"></a>示例  

 [!code-csharp[PrincipalPermissionMode#8](../../../../samples/snippets/csharp/VS_Snippets_CFX/principalpermissionmode/cs/source.cs#8)]
 [!code-vb[PrincipalPermissionMode#8](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/principalpermissionmode/vb/source.vb#8)]  
  
## <a name="compiling-the-code"></a>编译代码  

 编译代码需要引用以下命名空间：  
  
- <xref:System>  
  
- <xref:System.Collections.Generic>  
  
- <xref:System.Security.Permissions>  
  
- <xref:System.Security.Principal>  
  
- <xref:System.Threading>  
  
- <xref:System.ServiceModel>  
  
- <xref:System.ServiceModel.Channels>  
  
- <xref:System.ServiceModel.Description>  
  
- <xref:System.IdentityModel.Claims>  
  
- <xref:System.IdentityModel.Policy>  
  
## <a name="see-also"></a>另请参阅

- <xref:System.ServiceModel.Description.PrincipalPermissionMode>
- <xref:System.Security.Permissions.PrincipalPermissionAttribute>
- [如何：将 ASP.NET 角色提供程序与服务一起使用](../feature-details/how-to-use-the-aspnet-role-provider-with-a-service.md)
- [如何：使用 PrincipalPermissionAttribute 类限制访问](../how-to-restrict-access-with-the-principalpermissionattribute-class.md)
