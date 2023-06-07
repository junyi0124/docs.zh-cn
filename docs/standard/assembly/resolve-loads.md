---
title: 解析程序集加载
description: 本文介绍 .NET AppDomain.AssemblyResolve 事件。 将此事件用于需要对程序集加载进行控制的应用程序。
ms.date: 08/20/2019
helpviewer_keywords:
- assemblies [.NET], resolving loads
- application domains, loading assemblies
- resolving assembly loads
- assemblies [.NET], loading
- application domains, resolving assembly loads
ms.assetid: 5099e549-f4fd-49fb-a290-549edd456c6a
dev_langs:
- csharp
- vb
- cpp
ms.openlocfilehash: edd101e57793668d71d44db08f191ae412c6d998
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95720901"
---
# <a name="resolve-assembly-loads"></a>解析程序集加载

.NET 为对程序集加载需要更强控制的应用程序提供了 <xref:System.AppDomain.AssemblyResolve?displayProperty=nameWithType> 事件。 通过处理此事件，应用程序可从常规探测路径外部将程序集加载到加载上下文、从几个程序集版本中选择要加载的版本、发出动态程序集并返回此程序集，等等。 本主题指导如何处理 <xref:System.AppDomain.AssemblyResolve> 事件。  
  
> [!NOTE]
> 若要在仅限反射的上下文中解决程序集加载问题，请改用 <xref:System.AppDomain.ReflectionOnlyAssemblyResolve?displayProperty=nameWithType> 事件。  
  
## <a name="how-the-assemblyresolve-event-works"></a>AssemblyResolve 事件的工作原理  

 注册 <xref:System.AppDomain.AssemblyResolve> 事件的处理程序后，每当运行时无法按名称绑定到程序集时，此处理程序都将被调用。 例如，从用户代码中调用以下方法可能导致引发 <xref:System.AppDomain.AssemblyResolve> 事件：  
  
- <xref:System.AppDomain.Load%2A?displayProperty=nameWithType> 方法重载或 <xref:System.Reflection.Assembly.Load%2A?displayProperty=nameWithType> 方法重载，其第一个参数是表示要加载的程序集的显示名称的字符串（即，<xref:System.Reflection.Assembly.FullName%2A?displayProperty=nameWithType> 属性返回的字符串）。  
  
- <xref:System.AppDomain.Load%2A?displayProperty=nameWithType> 方法重载或 <xref:System.Reflection.Assembly.Load%2A?displayProperty=nameWithType> 方法重载，其第一个参数是标识要加载的程序集的 <xref:System.Reflection.AssemblyName> 对象。  
  
- <xref:System.Reflection.Assembly.LoadWithPartialName%2A?displayProperty=nameWithType> 方法重载。  
  
- <xref:System.AppDomain.CreateInstance%2A?displayProperty=nameWithType> 或 <xref:System.AppDomain.CreateInstanceAndUnwrap%2A?displayProperty=nameWithType> 方法重载，实例化另一个应用程序域中的对象。  
  
### <a name="what-the-event-handler-does"></a>事件处理程序的功能  

 <xref:System.AppDomain.AssemblyResolve> 事件的处理程序接收要加载的程序集的显示名称（在 <xref:System.ResolveEventArgs.Name%2A?displayProperty=nameWithType> 属性中）。 如果处理程序无法识别程序集名称，则返回 `null` (C#)、`Nothing` (Visual Basic) 或 `nullptr` (Visual C++)。  
  
 如果处理程序识别了程序集名称，它可以加载并返回满足此请求的程序集。 下面的列表介绍了一些示例方案。  
  
- 如果处理程序知道某一版本程序集的位置，它可以使用 <xref:System.Reflection.Assembly.LoadFrom%2A?displayProperty=nameWithType> 或 <xref:System.Reflection.Assembly.LoadFile%2A?displayProperty=nameWithType> 方法加载程序集，然后返回已加载的程序集（如果成功）。  
  
- 如果处理程序有权访问以字节数组形式存储的程序集的数据库，则它可以通过使用可采用字节数组的一种 <xref:System.Reflection.Assembly.Load%2A?displayProperty=nameWithType> 方法重载来加载字节数组。  
  
- 处理程序可以生成动态程序集并将其返回。  
  
> [!NOTE]
> 处理程序必须将程序集加载到加载源上下文或加载上下文，或加载不具有上下文的程序集。如果处理程序使用 <xref:System.Reflection.Assembly.ReflectionOnlyLoad%2A?displayProperty=nameWithType> 或 <xref:System.Reflection.Assembly.ReflectionOnlyLoadFrom%2A?displayProperty=nameWithType> 方法将程序集加载到仅限反射的上下文，则引发 <xref:System.AppDomain.AssemblyResolve> 事件的加载尝试将失败。  
  
 事件处理程序负责返回适当的程序集。 通过将 <xref:System.ResolveEventArgs.Name%2A?displayProperty=nameWithType> 属性值传递到 <xref:System.Reflection.AssemblyName.%23ctor%28System.String%29> 构造函数，处理程序可以解析所请求程序集的显示名称。 从 .NET Framework 4 开始，处理程序可使用 <xref:System.ResolveEventArgs.RequestingAssembly%2A?displayProperty=nameWithType> 属性确定当前请求是否是另一程序集的依赖项。 此信息有助于识别满足依赖关系的程序集。  
  
 事件处理程序返回的程序集版本可能与请求的版本不同。  
  
 大多数情况下，处理程序返回的程序集在加载上下文中显示，与处理程序将其加载到的上下文无关。 例如，如果处理程序使用 <xref:System.Reflection.Assembly.LoadFrom%2A?displayProperty=nameWithType> 方法将程序集加载到加载源上下文，当处理程序返回此程序集时，它将在加载上下文中显示。 但在以下情况，处理程序返回程序集时将不会显示其上下文：  
  
- 处理程序加载了不具有上下文的程序集。  
  
- <xref:System.ResolveEventArgs.RequestingAssembly%2A?displayProperty=nameWithType> 属性不为 null。  
  
- 加载请求的程序集（即 <xref:System.ResolveEventArgs.RequestingAssembly%2A?displayProperty=nameWithType> 属性返回的程序集）时未加载上下文。  
  
 有关上下文的信息，请参阅 <xref:System.Reflection.Assembly.LoadFrom%28System.String%29?displayProperty=nameWithType> 方法重载。  
  
 可将同一程序集的多个版本加载到同一应用程序域中。 不推荐此做法，因为可能导致类型分配问题。 请参阅[适用于程序集加载的最佳做法](../../framework/deployment/best-practices-for-assembly-loading.md)。  
  
### <a name="what-the-event-handler-should-not-do"></a>事件处理程序的禁忌操作  

处理 <xref:System.AppDomain.AssemblyResolve> 事件的主要规则是不应试图返回无法识别的程序集。 编写处理程序时应了解哪些程序集可能会导致引发该事件。 处理程序集应对其他程序集返回 null。  

> [!IMPORTANT]
> 从 .NET Framework 4 开始，<xref:System.AppDomain.AssemblyResolve> 事件针对附属程序集引发。 此更改会影响为早期版本的 .NET Framework 编写的事件处理程序（如果此类事件处理程序尝试解决所有程序集加载请求）。 忽略无法识别的程序集的事件处理程序不受此更改的影响：这些程序会返回 NULL，并遵循正常的回退机制。  

加载程序集时，事件处理程序禁止使用可导致递归引发 <xref:System.AppDomain.AssemblyResolve> 事件的任何 <xref:System.AppDomain.Load%2A?displayProperty=nameWithType> 或 <xref:System.Reflection.Assembly.Load%2A?displayProperty=nameWithType> 方法重载，因为可能导致堆栈溢出。 （请参阅本主题前面部分提供的列表。）即使为加载请求提供异常处理也会发生此情况，因为异常都是在所有事件处理程序返回后引发的。 因此，如果未找到 `MyAssembly`，下面的代码将导致堆栈溢出：  

```cpp
using namespace System;
using namespace System::Reflection;

ref class Example
{
internal:
    static Assembly^ MyHandler(Object^ source, ResolveEventArgs^ e)
    {
        Console::WriteLine("Resolving {0}", e->Name);
        return Assembly::Load(e->Name);
    }
};

void main()
{
    AppDomain^ ad = AppDomain::CreateDomain("Test");
    ad->AssemblyResolve += gcnew ResolveEventHandler(&Example::MyHandler);

    try
    {
        Object^ obj = ad->CreateInstanceAndUnwrap(
            "MyAssembly, version=1.2.3.4, culture=neutral, publicKeyToken=null",
            "MyType");
    }
    catch (Exception^ ex)
    {
        Console::WriteLine(ex->Message);
    }
}

/* This example produces output similar to the following:

Resolving MyAssembly, Version=1.2.3.4, Culture=neutral, PublicKeyToken=null
Resolving MyAssembly, Version=1.2.3.4, Culture=neutral, PublicKeyToken=null
...
Resolving MyAssembly, Version=1.2.3.4, Culture=neutral, PublicKeyToken=null
Resolving MyAssembly, Version=1.2.3.4, Culture=neutral, PublicKeyToken=null

Process is terminated due to StackOverflowException.
 */
```

```csharp
using System;
using System.Reflection;

class BadExample
{
    static void Main()
    {
        AppDomain ad = AppDomain.CreateDomain("Test");
        ad.AssemblyResolve += MyHandler;

        try
        {
            object obj = ad.CreateInstanceAndUnwrap(
                "MyAssembly, version=1.2.3.4, culture=neutral, publicKeyToken=null",
                "MyType");
        }
        catch (Exception ex)
        {
            Console.WriteLine(ex.Message);
        }
    }

    static Assembly MyHandler(object source, ResolveEventArgs e)
    {
        Console.WriteLine("Resolving {0}", e.Name);
        return Assembly.Load(e.Name);
    }
}

/* This example produces output similar to the following:

Resolving MyAssembly, Version=1.2.3.4, Culture=neutral, PublicKeyToken=null
Resolving MyAssembly, Version=1.2.3.4, Culture=neutral, PublicKeyToken=null
...
Resolving MyAssembly, Version=1.2.3.4, Culture=neutral, PublicKeyToken=null
Resolving MyAssembly, Version=1.2.3.4, Culture=neutral, PublicKeyToken=null

Process is terminated due to StackOverflowException.
 */
```

```vb
Imports System.Reflection

Class BadExample

    Shared Sub Main()

        Dim ad As AppDomain = AppDomain.CreateDomain("Test")
        AddHandler ad.AssemblyResolve, AddressOf MyHandler

        Try
            Dim obj As object = ad.CreateInstanceAndUnwrap(
                "MyAssembly, version=1.2.3.4, culture=neutral, publicKeyToken=null",
                "MyType")
        Catch ex As Exception
            Console.WriteLine(ex.Message)
        End Try
    End Sub

    Shared Function MyHandler(ByVal source As Object, _
                              ByVal e As ResolveEventArgs) As Assembly
        Console.WriteLine("Resolving {0}", e.Name)
        Return Assembly.Load(e.Name)
    End Function
End Class

' This example produces output similar to the following:
'
'Resolving MyAssembly, Version=1.2.3.4, Culture=neutral, PublicKeyToken=null
'Resolving MyAssembly, Version=1.2.3.4, Culture=neutral, PublicKeyToken=null
'...
'Resolving MyAssembly, Version=1.2.3.4, Culture=neutral, PublicKeyToken=null
'Resolving MyAssembly, Version=1.2.3.4, Culture=neutral, PublicKeyToken=null
'
'Process is terminated due to StackOverflowException.
```

## <a name="see-also"></a>请参阅

- [适用于程序集加载的最佳做法](../../framework/deployment/best-practices-for-assembly-loading.md)
- [使用应用程序域](../../framework/app-domains/use.md)
