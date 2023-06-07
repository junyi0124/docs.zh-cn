---
title: 危险权限和策略管理
description: 请参阅 .NET 中各种危险权限的链接。 这些权限只能提供给可信代码，并且仅在必要时才提供。
ms.date: 03/30/2017
helpviewer_keywords:
- permissions [.NET Framework], policy administration
- security [.NET Framework], dangerous permissions
- code security, dangerous permissions
- secure coding, dangerous permissions
- permissions [.NET Framework], dangerous
ms.assetid: 1929e854-23a0-4bb1-94be-e8aa3b609e32
ms.openlocfilehash: a2f4469590fea38924430b07eaf20d49f4dc46e9
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90556936"
---
# <a name="dangerous-permissions-and-policy-administration"></a>危险权限和策略管理

[!INCLUDE[net_security_note](../../../includes/net-security-note-md.md)]

.NET Framework 为其提供权限的多个受保护的操作可能允许绕过安全系统。 应仅对可信的代码，并且仅在必要的时候授予这些危险的权限。 如果它被授予这些权限，通常会对恶意代码没有任何防范。  
  
> [!NOTE]
> 在 .NET Framework 4 中，对 .NET Framework 安全模型和术语进行了重大更改。 有关这些更改的详细信息，请参阅 [安全更改](/previous-versions/dotnet/framework/security/security-changes)。  
  
 下表介绍了危险权限。  
  
|权限|潜在的风险|  
|----------------|--------------------|  
|<xref:System.Security.Permissions.SecurityPermission>||  
|<xref:System.Security.Permissions.SecurityPermissionFlag.UnmanagedCode>|允许托管代码调用到非托管代码中，常常是很危险的。|  
|<xref:System.Security.Permissions.SecurityPermissionFlag.SkipVerification>|如果没有验证，代码可以执行任何操作。|  
|<xref:System.Security.Permissions.SecurityPermissionFlag.ControlEvidence>|无效证据可以欺骗安全策略。|  
|<xref:System.Security.Permissions.SecurityPermissionFlag.ControlPolicy>|修改安全策略的功能可以禁用安全性。|  
|<xref:System.Security.Permissions.SecurityPermissionFlag.SerializationFormatter>|序列化的使用可以避开可访问性机制。 有关详细信息，请参阅 [安全和序列化](security-and-serialization.md)。|  
|<xref:System.Security.Permissions.SecurityPermissionFlag.ControlPrincipal>|设置当前主体的功能可以欺骗基于角色的安全性。|  
|<xref:System.Security.Permissions.SecurityPermissionFlag.ControlThread>|由于与线程关联的安全状态，因此对线程进行操作很危险。|  
|<xref:System.Security.Permissions.ReflectionPermission>||  
|<xref:System.MemberAccessException>|可以使用私有成员来攻克可访问性机制。|  
  
## <a name="see-also"></a>请参阅

- [安全编码准则](../../standard/security/secure-coding-guidelines.md)
