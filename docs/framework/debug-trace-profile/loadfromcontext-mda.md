---
title: loadFromContext MDA
description: 了解 .NET 中的 loadFromContext 托管调试助手 () MDA，如果将程序集加载到 LoadFrom 上下文，则会激活该助手。
ms.date: 03/30/2017
helpviewer_keywords:
- MDAs (managed debugging assistants), LoadFrom context
- managed debugging assistants (MDAs), LoadFrom context
- LoadFrom context
- LoadFromContext MDA
ms.assetid: a9b14db1-d3a9-4150-a767-dcf3aea0071a
ms.openlocfilehash: 631939b38ace4d26d0deb5b104cc5de0df3d9f3a
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96247351"
---
# <a name="loadfromcontext-mda"></a>loadFromContext MDA

如果程序集加载到 `LoadFrom` 上下文，将激活 `loadFromContext` 托管调试助手 (MDA)。 这种情况可能由于调用 <xref:System.Reflection.Assembly.LoadFrom%2A?displayProperty=nameWithType> 或其他类似方法而发生。  
  
## <a name="symptoms"></a>症状  

 使用某些加载器方法可能导致在 `LoadFrom` 上下文中加载程序集。 使用此上下文可能导致序列化、转换和依赖项解析出现意外的行为。 通常，建议将程序集加载到 `Load` 上下文来避免这些问题。 如果没有此 MDA，很难确定程序集加载到了哪个上下文。  
  
## <a name="cause"></a>原因  

 通常，如果从 `Load` 上下文外部的路径加载程序集（例如全局程序集缓存或 <xref:System.AppDomainSetup.ApplicationBase%2A?displayProperty=nameWithType> 属性），则会将程序集加载到 `LoadFrom` 上下文。  
  
## <a name="resolution"></a>解决方法  

 配置应用程序使其不再需要 <xref:System.Reflection.Assembly.LoadFrom%2A> 调用。 可以使用以下技术进行此操作：  
  
- 在全局程序集缓存中安装程序集。  
  
- 将程序集放在 <xref:System.AppDomain> 的 <xref:System.AppDomainSetup.ApplicationBase%2A> 目录。 如果采用默认域，<xref:System.AppDomainSetup.ApplicationBase%2A> 目录是包含启动该进程的可执行文件的目录。 如果不方便移动程序集，可能还需要创建新的 <xref:System.AppDomain>。  
  
- 将探测路径添加到应用程序配置 (.config) 文件，如果依赖程序集位于可执行文件相对的子目录，则添加到辅助应用程序域。  
  
 每种情况下，均可将代码更改为使用 <xref:System.Reflection.Assembly.Load%2A?displayProperty=nameWithType> 方法。  
  
## <a name="effect-on-the-runtime"></a>对运行时的影响  

 MDA 对 CLR 没有任何影响。 它报告由于加载请求而使用的上下文。  
  
## <a name="output"></a>输出  

 MDA 报告程序集已加载到 `LoadFrom` 上下文。 它指定程序集的简单名称和路径。 它还建议避免使用 `LoadFrom` 上下文来减轻风险。  
  
## <a name="configuration"></a>Configuration  
  
```xml  
<mdaConfig>  
  <assistants>  
    <loadFromContext />  
  </assistants>  
</mdaConfig>  
```  
  
## <a name="example"></a>示例  

 以下代码示例展示了一种可激活该 MDA 的情况：  
  
```csharp
using System.Reflection;  
namespace ConsoleApplication1  
{  
    class Program  
    {  
        static void Main(string[] args)  
        {  
            // The following call caused the LoadFrom context to be used  
            // because the assembly is loaded using LoadFrom and the path is
            // located outside of the Load context probing path.
            Assembly.LoadFrom(@"C:\Text\Test.dll");  
        }  
    }  
}  
```  
  
## <a name="see-also"></a>另请参阅

- [使用托管调试助手诊断错误](diagnosing-errors-with-managed-debugging-assistants.md)
