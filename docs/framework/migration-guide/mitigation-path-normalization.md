---
title: 缓解：路径规范化
description: 了解 .NET Framework 中的路径规范化自面向 .NET Framework 4.6.2 的应用起有何变化。
ms.date: 03/30/2017
ms.assetid: 158d47b1-ba6d-4fa6-8963-a012666bdc31
ms.openlocfilehash: 6f7e07690ab06fc7ef03344556c045405a63c374
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96253591"
---
# <a name="mitigation-path-normalization"></a>缓解：路径规范化

自面向 .NET Framework 4.6.2 的应用起，.NET Framework 中的路径规范化已更改。  
  
## <a name="what-is-path-normalization"></a>什么是路径规范化？  

 路径规范化涉及修改用于标识路径或文件的字符串，使其与目标操作系统上的有效路径一致。 路径规范化通常涉及以下操作：  
  
- 规范化处理组件和目录分隔符。  
  
- 将当前目录应用到相对路径。  
  
- 评估路径中的相对目录 (`.`) 或父目录 (`..`)。  
  
- 删减指定字符。  
  
## <a name="the-changes"></a>更改  

 自面向 .NET Framework 4.6.2 的应用起，路径规范化在以下几个方面进行了更改：  
  
- 运行时在规范化处理路径时以操作系统的 [GetFullPathName](/windows/desktop/api/fileapi/nf-fileapi-getfullpathnamea) 函数为准。  
  
- 路径规范化再也不用删减目录部分的末尾内容（如目录名称末尾的空格）。  
  
- 支持完全信任形式的设备路径语法，包括 `\\.\` 和 `\\?\`（对于 mscorlib.dll 中的文件 I/O API）。  
  
- 运行时不会验证设备语法路径。  
  
- 支持使用设备语法来访问备用数据流。  
  
## <a name="impact"></a>影响  

对于面向 .NET Framework 4.6.2 或更高版本的应用，这些更改默认启用。 这些更改应该会提升性能，同时允许方法访问之前无法访问的路径。  
  
定目标到 .NET Framework 4.6.1 及更低版本、但在 .NET Framework 4.6.2 或更高版本控制下运行的应用不受此更改影响。  
  
## <a name="mitigation"></a>缓解  

 对于面向 .NET Framework 4.6.2 或更高版本的应用，可通过将以下内容添加到应用程序配置文件的 [\<runtime>](../configure-apps/file-schema/runtime/runtime-element.md) 部分，选择弃用此更改而使用旧版规范化：  
  
```xml  
<runtime>  
    <AppContextSwitchOverrides value="Switch.System.IO.UseLegacyPathHandling=true" />
</runtime>  
```  
  
对于面向 .NET Framework 4.6.1 及更低版本，但在 .NET Framework 4.6.2 或更高版本上运行的应用，可通过将以下行添加到应用程序配置文件的 [\<runtime>](../configure-apps/file-schema/runtime/runtime-element.md) 部分，启用对路径规范化的更改：  
  
```xml  
<runtime>  
    <AppContextSwitchOverrides value="Switch.System.IO.UseLegacyPathHandling=false" />
</runtime>  
```  
  
## <a name="see-also"></a>请参阅

- [应用程序兼容性](application-compatibility.md)
