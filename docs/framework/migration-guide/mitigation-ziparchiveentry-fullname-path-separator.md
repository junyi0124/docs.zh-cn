---
title: 缓解：ZipArchiveEntry.FullName 路径分隔符
description: 了解 ZipArchiveEntry.FullName 属性的路径分隔符自面向 .NET Framework 4.6.1 的应用起有何变化。
ms.date: 03/30/2017
helpviewer_keywords:
- application compatibility
- ',NET Framework application compatibility'
- .NET Framework 4.6.1
- .NET Framework 4.6.1 retargeting changes
- retargeting changes
ms.assetid: 8d575722-4fb6-49a2-8a06-f72d62dc3766
ms.openlocfilehash: 6e2c0908098f8487f7d429274fe7ad797bedfda1
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96283440"
---
# <a name="mitigation-ziparchiveentryfullname-path-separator"></a>缓解：ZipArchiveEntry.FullName 路径分隔符

自面向 .NET Framework 4.6.1 的应用起，<xref:System.IO.Compression.ZipArchiveEntry.FullName%2A?displayProperty=nameWithType> 属性中使用的路径分隔符已从旧版 .NET Framework 中使用的反斜杠（“\\”）更改为正斜杠（“/”）。 <xref:System.IO.Compression.ZipArchiveEntry?displayProperty=nameWithType> 对象是通过调用 <xref:System.IO.Compression.ZipFile.CreateFromDirectory%2A?displayProperty=nameWithType> 方法的重载之一进行创建。  
  
## <a name="impact"></a>影响  

 该更改使 .NET 实现遵循 [.ZIP 文件格式规范](https://pkware.cachefly.net/webdocs/casestudies/APPNOTE.TXT)的 4.4.17.1 部分，还允许 .ZIP 存档在非 Windows 系统上进行解压缩。  
  
 对于面向非 Windows 操作系统（如 MacOS）上旧版 .NET Framework 的应用程序，解压缩其创建的 zip 文件将无法保留目录结构。 例如，在 MacOS 上，该应用创建一组文件，它们的文件名与目录路径、任何反斜杠（“\\”）字符和文件名相连。 因此，不会保留解压缩的文件的目录结构。  
  
 对于在 Windows 操作系统上由 .NET Framework<xref:System.IO> 命名空间中的 API 解压缩的 .ZIP 文件，此更改造成的影响应该是最小的，因为这些 API 可以无缝地将正斜杠（“/”） 或反斜杠（“\\”）处理为路径分隔符。  
  
## <a name="mitigation"></a>缓解  

 如果此行为不可取，可以在应用程序配置文件的 [\<runtime>](../configure-apps/file-schema/runtime/runtime-element.md) 部分中添加配置设置，从而选择禁用此行为。 下面展示了 `<runtime>` 部分和选择禁用此行为的开关。  
  
```xml  
<runtime>  
   <AppContextSwitchOverrides value="Switch.System.IO.Compression.ZipFile.UseBackslash=true" />  
</runtime>  
```  
  
 此外，对于面向先前版本的 .NET Framework，但在 .NET Framework 4.6.1 及更高版本上运行的应用，可通过将配置设置添加到应用程序配置文件的 [\<runtime>](../configure-apps/file-schema/runtime/runtime-element.md) 部分中来选择启用此行为。 下面展示了 `<runtime>` 部分和选择启用此行为的开关。  
  
```xml  
<runtime>  
   <AppContextSwitchOverrides value="Switch.System.IO.Compression.ZipFile.UseBackslash=false" />  
</runtime>  
```  
  
## <a name="see-also"></a>请参阅

- [应用程序兼容性](application-compatibility.md)
