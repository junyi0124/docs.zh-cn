---
title: 程序集绑定重定向安全权限
description: 了解 .NET 中的应用程序配置文件中显式程序集绑定重定向所需的安全权限。
ms.date: 03/30/2017
helpviewer_keywords:
- side-by-side execution, assembly binding redirection
- assemblies [.NET Framework], binding redirection
ms.assetid: 24a5cdff-7ed9-4195-93f3-edf6899019fc
ms.openlocfilehash: 2de2c50f5adb9e9fa36ea015ef498e9953c83005
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91165216"
---
# <a name="assembly-binding-redirection-security-permission"></a>程序集绑定重定向安全权限

应用程序配置文件中的显式程序集绑定重定向需要安全权限。 这适用于对 .NET Framework 程序集和来自第三方的程序集的重定向。 通过在上设置标志来授予权限 <xref:System.Security.Permissions.SecurityPermissionFlag> <xref:System.Security.Permissions.SecurityPermission> 。 默认情况下，托管程序集没有权限。  
  
 安全权限授予受信任区域中运行的应用程序 (本地计算机) 和 Intranet 区域。 严格禁止在 Internet 区域中运行的应用程序执行程序集绑定重定向。  
  
 如果程序集重定向是在由组件发行者控制的发行者策略文件中执行，或者在管理员控制的计算机配置文件中执行，则不需要该权限。 但是，若要使应用程序使用 [\<publisherPolicy apply="no"/>](./file-schema/runtime/publisherpolicy-element.md) 应用程序配置文件中的元素显式忽略发行者策略，则需要权限。  
  
 下表显示了 **BindingRedirects** 标志的默认安全设置。  
  
|区域|BindingRedirects 标志设置|  
|----------|-----------------------------------|  
|受信任区域 (本地计算机) |**ON**|  
|Intranet 区域|**ON**|  
|Internet 区域|**OFF**|  
|不受信任区域|**OFF**|  
  
 管理员可以更改这些安全设置，以支持或限制给定计算机上的特定方案。 没有用于更改 **BindingRedirects** 标志设置的默认值的工具;管理员必须手动编辑用户计算机上的 Security.config 文件。  
  
## <a name="see-also"></a>请参阅

- [发行者策略文件和并行 (Side-by-Side) 执行](/previous-versions/dotnet/netframework-4.0/06d2bae3(v=vs.100))
- [如何：启用和禁用自动绑定重定向](how-to-enable-and-disable-automatic-binding-redirection.md)
- [并行执行](../deployment/side-by-side-execution.md)
