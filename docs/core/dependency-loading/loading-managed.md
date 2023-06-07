---
title: 托管程序集加载算法 - .NET Core
description: .NET Core 中托管程序集加载算法的详细信息说明
ms.date: 08/09/2019
author: sdmaclea
ms.author: stmaclea
ms.openlocfilehash: 312a320676be6eb453697e0704ab771a6707618b
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/14/2020
ms.locfileid: "73973499"
---
# <a name="managed-assembly-loading-algorithm"></a>托管程序集加载算法

托管程序集与涉及不同阶段的算法一起定位并加载。

除附属程序集和 `WinRT` 程序集之外的所有托管程序集都使用相同算法。

## <a name="when-are-managed-assemblies-loaded"></a>何时加载托管程序集？

触发托管程序集加载的最常见机制是静态程序集引用。 每当代码使用在另一个程序集中定义的类型时，编译器都会插入这些引用。 根据运行时的需要加载这些程序集 (`load-by-name`)。

直接使用特定的 API 也将触发加载：

|API  |描述  |`Active` <xref:System.Runtime.Loader.AssemblyLoadContext> |
|---------|---------|---------|
|<xref:System.Runtime.Loader.AssemblyLoadContext.LoadFromAssemblyName%2A?displayProperty=nameWithType>|`Load-by-name`|[this](../../csharp/language-reference/keywords/this.md) 实例。|
|<xref:System.Runtime.Loader.AssemblyLoadContext.LoadFromAssemblyPath%2A?displayProperty=nameWithType><p><xref:System.Runtime.Loader.AssemblyLoadContext.LoadFromNativeImagePath%2A?displayProperty=nameWithType>|从路径加载。|[this](../../csharp/language-reference/keywords/this.md) 实例。|
<xref:System.Runtime.Loader.AssemblyLoadContext.LoadFromStream%2A?displayProperty=nameWithType>|从对象加载。|[this](../../csharp/language-reference/keywords/this.md) 实例。|
|<xref:System.Reflection.Assembly.LoadFile%2A?displayProperty=nameWithType>|在新的 <xref:System.Runtime.Loader.AssemblyLoadContext> 实例中从路径加载|新的 <xref:System.Runtime.Loader.AssemblyLoadContext> 实例。|
<xref:System.Reflection.Assembly.LoadFrom%2A?displayProperty=nameWithType>|在 <xref:System.Runtime.Loader.AssemblyLoadContext.Default%2A?displayProperty=nameWithType> 实例中从路径加载。<p>向 <xref:System.Runtime.Loader.AssemblyLoadContext.Default%2A?displayProperty=nameWithType> 添加 <xref:System.Runtime.Loader.AssemblyLoadContext.Resolving> 处理程序。 处理程序将从其目录加载程序集的依赖项。|<xref:System.Runtime.Loader.AssemblyLoadContext.Default%2A?displayProperty=nameWithType> 实例。|
|<xref:System.Reflection.Assembly.Load(System.Reflection.AssemblyName)?displayProperty=nameWithType><p><xref:System.Reflection.Assembly.Load(System.String)?displayProperty=nameWithType><p><xref:System.Reflection.Assembly.LoadWithPartialName%2A?displayProperty=nameWithType>|`Load-by-name`。|从调用方推断。<p>首选 <xref:System.Runtime.Loader.AssemblyLoadContext> 方法。|
|<xref:System.Reflection.Assembly.Load(System.Byte[])?displayProperty=nameWithType><p><xref:System.Reflection.Assembly.Load(System.Byte[],System.Byte[])?displayProperty=nameWithType>|从新 <xref:System.Runtime.Loader.AssemblyLoadContext> 实例的对象中加载。|新的 <xref:System.Runtime.Loader.AssemblyLoadContext> 实例。|
<xref:System.Type.GetType(System.String)?displayProperty=nameWithType><p><xref:System.Type.GetType(System.String,System.Boolean)?displayProperty=nameWithType><p><xref:System.Type.GetType(System.String,System.Boolean,System.Boolean)?displayProperty=nameWithType>|`Load-by-name`。|从调用方推断。<p>首选使用 `assemblyResolver` 参数的 <xref:System.Type.GetType%2A?displayProperty=nameWithType> 方法。|
<xref:System.Reflection.Assembly.GetType%2A?displayProperty=nameWithType>|如果类型 `name` 描述程序集限定的泛型类型，则触发 `Load-by-name`。|从调用方推断。<p>使用程序集限定的类型名称时，首选 <xref:System.Type.GetType%2A?displayProperty=nameWithType>。|
<xref:System.Activator.CreateInstance(System.String,System.String)?displayProperty=nameWithType><p><xref:System.Activator.CreateInstance(System.String,System.String,System.Object[])?displayProperty=nameWithType><p><xref:System.Activator.CreateInstance(System.String,System.String,System.Boolean,System.Reflection.BindingFlags,System.Reflection.Binder,System.Object[],System.Globalization.CultureInfo,System.Object[])?displayProperty=nameWithType>|`Load-by-name`。|从调用方推断。<p>首选采用 <xref:System.Type> 参数的 <xref:System.Activator.CreateInstance%2A?displayProperty=nameWithType> 方法。|

## <a name="algorithm"></a>算法

以下算法描述运行时如何加载托管程序集。

1. 确定 `active` <xref:System.Runtime.Loader.AssemblyLoadContext>。

    - 对于静态程序集引用，`active` <xref:System.Runtime.Loader.AssemblyLoadContext> 是已加载引用程序集的实例。
    - 首选 API 使 `active` <xref:System.Runtime.Loader.AssemblyLoadContext> 显式。
    - 其他 API 推断 `active` <xref:System.Runtime.Loader.AssemblyLoadContext>。 对于这些 API，将使用 <xref:System.Runtime.Loader.AssemblyLoadContext.CurrentContextualReflectionContext?displayProperty=nameWithType> 属性。 如果其值为 `null`，则使用推断的 <xref:System.Runtime.Loader.AssemblyLoadContext> 实例。
    - 请见上表。

2. 对于 `Load-by-name` 方法，活动 <xref:System.Runtime.Loader.AssemblyLoadContext> 将加载程序集。 通过以下方式按优先级排序：
    - 检查其 `cache-by-name`。

    - 调用 <xref:System.Runtime.Loader.AssemblyLoadContext.Load%2A?displayProperty=nameWithType> 函数。

    - 检查 <xref:System.Runtime.Loader.AssemblyLoadContext.Default%2A?displayProperty=nameWithType> 实例的缓存并运行[托管程序集默认探测](default-probing.md#managed-assembly-default-probing)逻辑。

    - 引发 AssemblyLoadContext 活动的 <xref:System.Runtime.Loader.AssemblyLoadContext.Resolving?displayProperty=nameWithType> 事件。

    - 引发 <xref:System.AppDomain.AssemblyResolve?displayProperty=nameWithType> 事件。

3. 对于其他类型的加载，`active` <xref:System.Runtime.Loader.AssemblyLoadContext> 加载程序集。 通过以下方式按优先级排序：
    - 检查其 `cache-by-name`。

    - 从指定的路径或原始程序集对象加载。

4. 在任一情况下，如果新加载了一个程序集，则：
   - 引发 <xref:System.AppDomain.AssemblyLoad?displayProperty=nameWithType> 事件。
   - 将引用添加到程序集的 <xref:System.Runtime.Loader.AssemblyLoadContext> 实例的 `cache-by-name`。

5. 如果找到了程序集，则会根据需要将引用添加到 `active` <xref:System.Runtime.Loader.AssemblyLoadContext> 实例的 `cache-by-name` 中。
