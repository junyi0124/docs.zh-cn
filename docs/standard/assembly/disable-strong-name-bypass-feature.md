---
title: 如何：禁用强名称跳过功能
description: 强名称跳过功能可跳过 .NET 中完全受信任的域中的签名验证。 你可以为单个应用程序或所有应用程序替代此功能。
ms.date: 08/20/2019
helpviewer_keywords:
- strong-name bypass feature
- strong-named assemblies, loading into trusted application domains
ms.assetid: 234e088c-3b11-495a-8817-e0962be79d82
ms.openlocfilehash: 2846efbbd76cf677a42a7031e53661d302c6c964
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95687458"
---
# <a name="how-to-disable-the-strong-name-bypass-feature"></a>如何：禁用强名称跳过功能

从 .NET Framework 3.5 版 Service Pack 1 (SP1) 开始，当程序集加载到完全信任的 <xref:System.AppDomain> 对象（如 `MyComputer` 区域的默认 <xref:System.AppDomain>）时，不会验证强名称签名。 这被称之为强名称跳过功能。 在完全信任的环境中，对于已签名的完全信任的程序集，无需考虑其签名，对 <xref:System.Security.Permissions.StrongNameIdentityPermission> 的要求总是成功。 唯一的限制是该程序集必须完全受信任，因为其区域是完全受信任的。 因为在这些条件下，强名称不是决定性因素，所以没有理由验证强名称。 跳过验证强名称签名可显著提高性能。  
  
 该跳过功能适用于未被延迟签名的任何完全信任程序集，以及从其 <xref:System.AppDomain> 属性指定的目录加载到任何完全信任的 <xref:System.AppDomainSetup.ApplicationBase%2A> 中的完全信任程序集。  
  
 可通过设置注册表项值来替代计算机中所有应用程序的跳过功能。 可使用应用程序配置文件来替代单个应用程序的设置。 如果注册表项禁用了单个应用程序的跳过功能，则无法恢复该功能。  
  
 替代跳过功能后，将只验证强名称的正确性，而不检查其 <xref:System.Security.Permissions.StrongNameIdentityPermission>。 如果要确认某个特定的强名称，必须单独执行该检查。  
  
> [!IMPORTANT]
> 如下面的过程所述，是否能强制执行强名称验证取决于注册表项。 如果运行应用程序时使用的帐户没有访问该注册表项的访问控制列表 (ACL) 权限，则该设置无效。 必须确保配置了此注册表项的 ACL 权限，使所有程序集均可读取此项。  
  
## <a name="disable-the-strong-name-bypass-feature-for-all-applications"></a>对所有应用程序禁用强名称跳过功能  
  
- 在 32 位计算机上的系统注册表中，在 HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\\.NETFramework 项下创建名为 `AllowStrongNameBypass`，值为 0 的 DWORD 项。  
  
- 在 64 位计算机上的系统注册表中，在 HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\\.NETFramework 和HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\\.NETFramework 项下创建名为 `AllowStrongNameBypass`，值为 0 的 DWORD 项。  
  
## <a name="disable-the-strong-name-bypass-feature-for-a-single-application"></a>对单个应用程序禁用强名称跳过功能  
  
1. 打开或创建应用程序配置文件。  
  
    有关此文件的详细信息，请参阅[配置应用](../../framework/configure-apps/index.md)中的“应用程序配置文件”一节。  
  
2. 添加以下项：  
  
    ```xml  
    <configuration>  
      <runtime>  
        <bypassTrustedAppStrongNames enabled="false" />  
      </runtime>  
    </configuration>  
    ```  
  
 通过删除配置文件设置或将属性设置为 `true`，可以还原应用程序的跳过功能。  
  
> [!NOTE]
> 只有对计算机启用了跳过功能，才能打开和关闭针对应用程序的强名称验证。 如果对计算机关闭了跳过功能，将对所有应用程序验证强名称，并且不能对单个应用程序跳过验证。  
  
## <a name="see-also"></a>请参阅

- [Sn.exe（强名称工具）](../../framework/tools/sn-exe-strong-name-tool.md)
- [\<bypassTrustedAppStrongNames> 元素](../../framework/configure-apps/file-schema/runtime/bypasstrustedappstrongnames-element.md)
- [创建和使用具有强名称的程序集](create-use-strong-named.md)
