---
title: 配置程序集绑定重定向
description: 请参阅如何通过使用应用程序配置文件的 assemblyBinding 元素中的 applyTo 特性在 .NET 中配置程序集绑定重定向。
ms.date: 03/30/2017
helpviewer_keywords:
- side-by-side execution, assembly binding redirection
- assemblies [.NET Framework], binding redirection
ms.assetid: d266cbd8-bf91-41d1-baf0-afbc481a741f
ms.openlocfilehash: 2455cab19132a208fb99454d4131363a624315c5
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96236372"
---
# <a name="configuring-assembly-binding-redirection"></a>配置程序集绑定重定向

默认情况下，应用程序使用一组 .NET Framework 程序集，该程序集随用于编译该应用程序的运行时版本一起提供。 可以使用应用程序配置文件中 [\<assemblyBinding>](../configure-apps/file-schema/runtime/assemblybinding-element-for-runtime.md) 元素上的 appliesTo 特性，将程序集绑定引用重定向到 .NET Framework 程序集的特定版本。 此可选特性用 .NET Framework 版本号来指示它应用于哪个版本。 如果没有指定 appliesTo 特性，\<assemblyBinding> 元素将适用于 .NET Framework 的所有版本 。  
  
 在 .NET Framework 1.1 版中引入了 appliesTo 特性，而在 .NET Framework 1.0 版中则忽略了此特性。 这意味着，即使指定了 appliesTo 特性，在使用 .NET Framework 版本 1.0 时所有的 \<assemblyBinding> 元素也都适用 。  
  
> [!NOTE]
> 使用 appliesTo 特性来限制运行时特定版本的程序集绑定重定向。  
  
 例如，若要重定向 .NET Framework 1.0 版程序集的程序集绑定，应在你的应用程序配置文件中包括以下 XML 代码。  
  
```xml  
<runtime>  
        <assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1" appliesTo="v1.0.3705">  
            <dependentAssembly>
               * assembly information goes here *  
            </dependentAssembly>  
       </assemblyBinding>  
</runtime>  
```  
  
 \<assemblyBinding> 元素要区分顺序。 应首先输入任何 .NET Framework 1.0 版程序集的程序集绑定重定向信息，再输入任何 .NET Framework 1.1 版程序集的程序集绑定重定向信息。 最后，输入任何因不使用 **appliesTo** 特性而适用于所有版本的 .NET Framework 的.NET Framework 程序集重定向的程序集绑定重定向信息。 如果发生重定向冲突，请使用配置文件中的第一个匹配的重定向语句。  
  
 例如，若要将一个引用重定向到 .NET Framework 1.0 版程序集，而将另一个引用重定向到 .NET Framework 1.1 版程序集，将使用以下伪代码中所示的模式。  
  
```xml  
<assemblyBinding xmlns="..." appliesTo="v1.0.3705">
  <!-- .NET Framework version 1.0 redirects here. -->
</assemblyBinding>
  
<assemblyBinding xmlns="..." appliesTo="v1.1.4322">
  <!-- .NET Framework version 1.1 redirects here. -->
</assemblyBinding>
  
<assemblyBinding xmlns="...">
  <!-- Redirects meant for all versions of the .NET Framework. -->
</assemblyBinding>  
```  
  
## <a name="debugging-configuration-file-errors"></a>调试配置文件错误  

 一旦创建了应用程序域，运行时就会分析配置文件并将代码加载到该应用程序域中。 公共语言运行时通过忽略条目来处理配置文件中的错误。 如果配置文件含有格式不正确的 XML，则运行时将忽略整个配置文件。 对于无效的 XML，仅忽略无效的部分。  
  
 可以通过确定是否正在发生程序集绑定重定向来确定是否正在使用某个配置文件。 使用[程序集绑定日志查看器 (Fuslogvw.exe)](../tools/fuslogvw-exe-assembly-binding-log-viewer.md) 查看正在加载哪些程序集。 若要查看所有的程序集绑定，必须在注册表中设置 ForceLog 的条目。  
  
## <a name="see-also"></a>请参阅

- [如何：启用和禁用自动绑定重定向](../configure-apps/how-to-enable-and-disable-automatic-binding-redirection.md)
