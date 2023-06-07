---
title: 了解 AssemblyLoadContext - .NET Core
description: 用来了解 .NET Core 中 AssemblyLoadContext 的用途和行为的关键概念。
ms.date: 08/09/2019
author: sdmaclea
ms.author: stmaclea
ms.openlocfilehash: 4d3f0e50e7c336469bd9af4d1589427388684434
ms.sourcegitcommit: dfcbc096ad7908cd58a5f0aeabd2256f05266bac
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/21/2020
ms.locfileid: "92332817"
---
# <a name="understanding-systemruntimeloaderassemblyloadcontext"></a>了解 System.Runtime.Loader.AssemblyLoadContext

<xref:System.Runtime.Loader.AssemblyLoadContext> 类对于 .NET Core 是唯一的。 本文尝试使用概念性信息来补充 <xref:System.Runtime.Loader.AssemblyLoadContext> API 文档。

本文与实现动态加载的开发人员有关，尤其是动态加载框架的开发人员。

## <a name="what-is-the-assemblyloadcontext"></a>什么是 AssemblyLoadContext？

每个 .NET Core 应用程序均隐式使用 <xref:System.Runtime.Loader.AssemblyLoadContext>。
它是运行时的提供程序，用于定位和加载依赖项。 只要加载了依赖项，就会调用 <xref:System.Runtime.Loader.AssemblyLoadContext> 实例来定位该依赖项。

- 它提供定位、加载和缓存托管程序集和其他依赖项的服务。

- 为了支持动态代码加载和卸载，它创建了一个独立上下文，用于在其自己的 <xref:System.Runtime.Loader.AssemblyLoadContext> 实例中加载代码及其依赖项。

## <a name="when-do-you-need-multiple-assemblyloadcontext-instances"></a>何时需要多个 AssemblyLoadContext 实例？

单个 <xref:System.Runtime.Loader.AssemblyLoadContext> 实例限制为每个简单程序集名称 <xref:System.Reflection.AssemblyName.Name?displayProperty=nameWithType> 只加载 <xref:System.Reflection.Assembly> 的一个版本。

当动态加载代码模块时，此限制可能会成为一个问题。 每个模块都是独立编译的，并且可能依赖于不同版本的 <xref:System.Reflection.Assembly>。 当不同的模块依赖于常用库的不同版本时，通常会出现此问题。

为了支持动态加载代码，<xref:System.Runtime.Loader.AssemblyLoadContext> API 提供在同一个应用程序中加载 <xref:System.Reflection.Assembly> 冲突版本的功能。 每个 <xref:System.Runtime.Loader.AssemblyLoadContext> 实例提供一个唯一字典，该字典将每个 <xref:System.Reflection.AssemblyName.Name?displayProperty=nameWithType> 映射到特定的 <xref:System.Reflection.Assembly> 实例。

它还提供了一种方便的机制，将与代码模块相关的依赖项分组，以便以后进行卸载。

## <a name="what-is-special-about-the-assemblyloadcontextdefault-instance"></a>AssemblyLoadContext.Default 实例有什么特别之处？

启动时，运行时将自动填充 <xref:System.Runtime.Loader.AssemblyLoadContext.Default?displayProperty=nameWithType> 实例。  它使用[默认探测](default-probing.md)来定位和查找所有静态依赖项。

它解决了最常见的依赖项加载方案。

## <a name="how-does-assemblyloadcontext-support-dynamic-dependencies"></a>AssemblyLoadContext 如何支持动态依赖项？

<xref:System.Runtime.Loader.AssemblyLoadContext> 具有可替代的各种事件和虚函数。

<xref:System.Runtime.Loader.AssemblyLoadContext.Default?displayProperty=nameWithType> 实例仅支持替代事件。

[托管程序集加载算法](loading-managed.md)、[附属程序集加载算法](loading-resources.md)和[非托管（本机）库加载算法](loading-unmanaged.md)文章引用了所有可用事件和虚拟函数。  这些文章显示加载算法中的每个事件和函数的相对位置。 本文不会重现该信息。

本部分介绍相关事件和函数的一般原则。

- **可重复** 。 针对特定依赖项的查询必须始终产生相同的响应。 必须返回同一个已加载的依赖项实例。 此要求是保持缓存一致性的基础。 特别是对于托管程序集，我们要创建 <xref:System.Reflection.Assembly> 缓存。 缓存键是一个简单的程序集名称 <xref:System.Reflection.AssemblyName.Name?displayProperty=nameWithType>。
- **通常不引发** 。  当找不到请求的依赖项时，这些函数应返回 `null` 而不是引发。 引发将提前结束搜索，并将异常传至调用方。 应将引发限制为针对意外错误，如程序集损坏或内存不足等情况。
- **避免递归** 。 请注意，这些函数和处理程序实现了用于定位依赖项的加载规则。 实现不应调用触发递归的 API。 代码通常应调用 AssemblyLoadContext  加载函数，这些函数需要特定路径或内存引用参数。
- **加载到正确的 AssemblyLoadContext** 。 选择加载依赖项的位置是应用程序特定的。  选择是通过这些事件和函数实现的。 当代码调用 AssemblyLoadContext  时，按路径加载函数在你要加载代码的实例上调用它们。 有时返回 `null`，并让 <xref:System.Runtime.Loader.AssemblyLoadContext.Default?displayProperty=nameWithType> 处理加载可能是最简单的选项。
- **注意线程争用** 。 加载可由多个线程触发。 AssemblyLoadContext 通过以原子方式将程序集添加到其缓存来处理线程争用。 将丢弃争用失败方的实例。 在实现逻辑中，不要添加未正确处理多个线程的额外逻辑。

## <a name="how-are-dynamic-dependencies-isolated"></a>如何隔离动态依赖项？

每个 <xref:System.Runtime.Loader.AssemblyLoadContext> 实例都表示 <xref:System.Reflection.Assembly> 实例和 <xref:System.Type> 定义的唯一范围。

这些依赖项之间没有任何二进制隔离。 它们仅通过不按名称查找彼此来进行隔离。

在每个 <xref:System.Runtime.Loader.AssemblyLoadContext> 中：

- <xref:System.Reflection.AssemblyName.Name?displayProperty=nameWithType> 可以引用不同的 <xref:System.Reflection.Assembly> 实例。
- <xref:System.Type.GetType%2A?displayProperty=nameWithType> 可能会为同一类型 `name` 返回不同类型的实例。

## <a name="how-are-dependencies-shared"></a>如何共享依赖项？

可以在 <xref:System.Runtime.Loader.AssemblyLoadContext> 实例之间轻松共享依赖项。 常规模型用于一个 <xref:System.Runtime.Loader.AssemblyLoadContext> 来加载依赖项。  另一个通过使用对已加载程序集的引用来共享依赖项。

此共享是运行时程序集所必需的。 这些程序集只能加载到 <xref:System.Runtime.Loader.AssemblyLoadContext.Default?displayProperty=nameWithType>。 `ASP.NET`、`WPF` 或 `WinForms` 等框架也是如此。

建议将共享依赖项加载到 <xref:System.Runtime.Loader.AssemblyLoadContext.Default?displayProperty=nameWithType>。 此共享是常见的设计模式。

共享是通过自定义 <xref:System.Runtime.Loader.AssemblyLoadContext> 实例编码实现的。 <xref:System.Runtime.Loader.AssemblyLoadContext> 具有可替代的各种事件和虚函数。 当这些函数中的任何函数返回对在另一个 <xref:System.Runtime.Loader.AssemblyLoadContext> 实例中加载的 <xref:System.Reflection.Assembly> 实例的引用时，将共享 <xref:System.Reflection.Assembly> 实例。 标准加载算法会延迟 <xref:System.Runtime.Loader.AssemblyLoadContext.Default?displayProperty=nameWithType> 加载，以简化通用共享模式。  请参阅[托管程序集加载算法](loading-managed.md)。

## <a name="complications"></a>复杂情况

### <a name="type-conversion-issues"></a>类型转换问题

当两个 <xref:System.Runtime.Loader.AssemblyLoadContext> 实例包含具有相同 `name` 的类型定义时，它们不是同一类型。 当且仅当它们来自同一个 <xref:System.Reflection.Assembly> 实例时，它们的类型相同。

使事情复杂化的是，这些不匹配类型的异常消息可能会令人困惑。 在异常消息中，按简单类型名称来引用这些类型。 在这种情况下，常见异常消息的格式如下所示：

> 无法将类型为“IsolatedType”的对象转换为类型“IsolatedType”。

### <a name="debugging-type-conversion-issues"></a>调试类型转换问题

如果给定一对不匹配的类型，还必须要了解：

- 每种类型的 <xref:System.Type.Assembly?displayProperty=nameWithType>
- 每种类型的 <xref:System.Runtime.Loader.AssemblyLoadContext>，这可以通过 <xref:System.Runtime.Loader.AssemblyLoadContext.GetLoadContext(System.Reflection.Assembly)?displayProperty=nameWithType> 函数获得。

如果给定两个对象 `a` 和 `b`，在调试器中评估以下内容将非常有用：

```csharp
// In debugger look at each assembly's instance, Location, and FullName
a.GetType().Assembly
b.GetType().Assembly
// In debugger look at each AssemblyLoadContext's instance and name
System.Runtime.Loader.AssemblyLoadContext.GetLoadContext(a.GetType().Assembly)
System.Runtime.Loader.AssemblyLoadContext.GetLoadContext(b.GetType().Assembly)
```

### <a name="resolving-type-conversion-issues"></a>解决类型转换问题

可以通过两种设计模式来解决这些类型转换问题。

1. 使用常见的共享类型。 此共享类型可以是基元运行时类型，也可以涉及在共享程序集中创建新的共享类型。  共享类型通常是在应用程序的程序集中定义的[接口](../../csharp/language-reference/keywords/interface.md)。 另请参阅：[如何共享依赖项？](#how-are-dependencies-shared)。

2. 使用封送处理技术从一种类型转换为另一种类型。
