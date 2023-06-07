---
title: 全局程序集缓存
description: 了解全局程序集缓存，即安装了 .NET 公共语言运行时的计算机范围的代码缓存。
ms.date: 03/30/2017
helpviewer_keywords:
- assemblies [.NET Framework], global assembly cache
- GAC (global assembly cache)
- ACLs [.NET Framework]
- global assembly cache
- cache [.NET Framework], global assembly cache
- global assembly cache, about
- access control lists [.NET Framework]
ms.assetid: cf5eacd0-d3ec-4879-b6da-5fd5e4372202
ms.openlocfilehash: 57d18c01bd6261e8207d8ad3849a3a7da9a24b6c
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96264603"
---
# <a name="global-assembly-cache"></a>全局程序集缓存

安装了公共语言运行时的每台计算机均具有计算机范围的代码缓存，称为全局程序集缓存。 全局程序集缓存中存储专门指定给由计算机中若干应用程序共享的程序集。  
  
 只能在需要时才通过将程序集安装到全局程序集缓存中来共享程序集。 一般原则是：程序集依赖项保持专用，并将程序集放在应用程序目录中，除非明确要求共享该程序集。 另外，无需为了使 COM 互操作或非托管代码可以访问程序集而将程序集安装到全局程序集缓存。  
  
> [!NOTE]
> 在有些情况下，很明显不需要将程序集安装到全局程序集缓存中。 将组成应用程序的某个程序集置于全局程序集缓存中之后，无法再通过使用 xcopy 命令复制应用程序目录来复制或安装应用程序。 必须同时移动全局程序集缓存中的程序集。  
  
 可以通过两种方法将程序集部署到全局程序集缓存：  
  
- 使用专用于全局程序集缓存的安装程序。 这是将程序集安装到全局程序集缓存中的首选项。  
  
- 使用 Windows SDK 提供的名为[全局程序集缓存工具 (Gacutil.exe)](../tools/gacutil-exe-gac-tool.md) 的开发人员工具。  
  
    > [!NOTE]
    > 在部署方案中，使用 Windows Installer 将程序集安装到全局程序集缓存中。 仅在开发方案中使用全局程序集缓存工具，因为它不提供 Windows Installer 所能提供的程序集引用计数等功能。  
  
 从 .NET Framework 4 开始，全局程序集缓存的默认位置为 %windir%\Microsoft.NET\assembly。 在 .NET Framework 的早期版本中，默认位置为 %windir%\assembly。  
  
 管理员通常使用访问控制列表 (ACL) 来保护 systemroot 目录，从而控制写入和执行访问。 由于全局程序集缓存安装在 systemroot 目录的子目录中，因此它将继承该目录的 ACL。 建议只允许具有“管理员”权限的用户从全局程序集缓存中删除文件。  
  
 部署在全局程序集缓存中的程序集必须使用强名称。 将程序集添加到全局程序集缓存时，可对组成该程序集的所有文件执行完整性检查。 缓存通过执行这些完整性检查来确保该程序集未被篡改，例如，文件已更改，但清单没有反映出此更改。  
  
## <a name="see-also"></a>请参阅

- [.NET 中的程序集](../../standard/assembly/index.md)
- [使用程序集和全局程序集缓存](working-with-assemblies-and-the-gac.md)
- [具有强名称的程序集](../../standard/assembly/strong-named.md)
