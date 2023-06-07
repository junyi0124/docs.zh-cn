---
title: bindingFailure MDA
description: 阅读有关 bindingFailure 托管调试助手（MDA）的信息，当程序集未能加载到 .NET 中时，将激活该助手。
ms.date: 03/30/2017
helpviewer_keywords:
- binding failure
- binding, failures
- MDAs (managed debugging assistants), binding failures
- assemblies [.NET Framework], binding failures
- managed debugging assistants (MDAs), binding failures
- BindingFailure MDA
ms.assetid: 26ada5af-175c-4576-931a-9f07fa1723e9
ms.openlocfilehash: 98c7947c7e5d2a1f0af8c26744d3b292ed8cb4c4
ms.sourcegitcommit: a2c8b19e813a52b91facbb5d7e3c062c7188b457
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/26/2020
ms.locfileid: "85415623"
---
# <a name="bindingfailure-mda"></a>bindingFailure MDA

当程序集加载失败时，将激活 `bindingFailure` 托管调试助手 (MDA)。

## <a name="symptoms"></a>症状

代码已尝试使用静态引用或一种加载程序方法（<xref:System.Reflection.Assembly.Load%2A?displayProperty=nameWithType> 或 <xref:System.Reflection.Assembly.LoadFrom%2A?displayProperty=nameWithType>）加载程序集。 程序集未加载，且引发 <xref:System.IO.FileNotFoundException> 或 <xref:System.IO.FileLoadException> 异常。

## <a name="cause"></a>原因

当运行时无法加载程序集时，会发生绑定失败。 绑定失败的原因可能是下列情况之一：

- 公共语言运行时 (CLR) 找不到请求的程序集。 导致此问题的原因有很多，如未安装程序集或未正确配置应用程序而无法查找程序集。

- 常见的问题方案是将类型传递给另一个应用程序域，这需要 CLR 加载在其他应用程序域包含该类型的程序集。 如果其他应用程序域与原始应用程序域配置不同，那么运行时则不可能加载程序集。 例如，两个应用程序域可能具有不同的 <xref:System.AppDomain.BaseDirectory%2A> 属性值。

- 请求的程序集已损坏或不是程序集。

- 尝试加载程序集的代码没有正确的代码访问安全性权限来加载程序集。

- 用户凭据不提供读取文件所需的权限。

## <a name="resolution"></a>解决方法

第一步是确定 CLR 为何无法绑定到请求的程序集。 运行时找不到或无法加载请求的程序集的原因有很多，如“原因”部分中列出的情形。 建议采用以下操作来排除绑定失败的原因：

- 使用 `bindingFailure` MDA 提供的数据来确定原因：

  - 运行 [Fuslogvw.exe（程序集绑定日志查看器）](../tools/fuslogvw-exe-assembly-binding-log-viewer.md)以读取程序集绑定器生成的错误日志。

  - 确定程序集是否位于请求的位置。 在使用 <xref:System.Reflection.Assembly.LoadFrom%2A> 和 <xref:System.Reflection.Assembly.LoadFile%2A> 方法的情况下，可轻松确定请求的位置。 如果使用 <xref:System.Reflection.Assembly.Load%2A> 方法（利用程序集标识绑定），必须在应用程序域的 <xref:System.AppDomain.BaseDirectory%2A> 属性探测路径和全局程序集缓存中，查找与该标识匹配的程序集。

- 根据上述所确定事项解决失败原因。 可能的解决方案选项如下：

  - 在全局程序集缓存中安装请求的程序集，并调用  <xref:System.Reflection.Assembly.Load%2A> 方法以按标识加载程序集。

  - 将请求的程序集复制到应用程序目录中，并调用 <xref:System.Reflection.Assembly.Load%2A> 方法以按标识加载程序集。

  - 通过更改 <xref:System.AppDomain.BaseDirectory%2A> 属性或添加专用探测路径，重新配置发生绑定失败的应用程序域，以使其包含程序集路径。

  - 更改文件的访问控制列表，以让登录用户读取此文件。

## <a name="effect-on-the-runtime"></a>对运行时的影响

此 MDA 对 CLR 无任何影响。 它只报告与绑定失败有关的数据。

## <a name="output"></a>输出

MDA 会报告加载失败的程序集，包括请求的路径和/或显示名称、绑定上下文、请求加载的应用程序域，以及失败的原因。

显示名称或请求的路径可能为空（如果该数据对 CLR 不可用）。 如果失败的调用是对 <xref:System.Reflection.Assembly.Load%2A> 方法的调用，则运行时有可能无法确定程序集的显示名称。

## <a name="configuration"></a>配置

```xml
<mdaConfig>
  <assistants>
    <bindingFailure />
  </assistants>
</mdaConfig>
```

## <a name="example"></a>示例

以下代码示例展示了一种可激活该 MDA 的情况：

```csharp
using System;
using System.Collections.Generic;
using System.Text;
using System.Reflection;
namespace ConsoleApplication1
{
    class Program
    {
        static void Main(string[] args)
        {
            // This call attempts to load a nonexistent assembly.
            // The call will throw a System.IO.FileNotFound exception
            // and cause the activation of the bindingFailure MDA
            // if it is registered.
            Assembly.Load("NonExistentAssembly");
        }
    }
}
```

## <a name="see-also"></a>请参阅

- [使用托管调试助手诊断错误](diagnosing-errors-with-managed-debugging-assistants.md)
