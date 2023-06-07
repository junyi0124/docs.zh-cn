---
title: 附属程序集加载算法 - .NET Core
description: .NET Core 中附属程序集加载算法的详细说明
ms.date: 08/09/2019
author: sdmaclea
ms.author: stmaclea
ms.openlocfilehash: 17f29a9aca79019daa91736e586bf1b6b753a9ec
ms.sourcegitcommit: d9c7ac5d06735a01c1fafe34efe9486734841a72
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/06/2020
ms.locfileid: "82859529"
---
# <a name="satellite-assembly-loading-algorithm"></a>附属程序集加载算法

使用附属程序集来存储为语言和区域性自定义的本地化资源。

附属程序集使用不同于常规托管程序集的加载算法。

## <a name="when-are-satellite-assemblies-loaded"></a>何时加载附属程序集？

加载本地化资源时加载附属程序集。

加载本地化资源的基本 API 是 <xref:System.Resources.ResourceManager?displayProperty=fullName> 类。 最后，<xref:System.Resources.ResourceManager> 类将为每个 <xref:System.Globalization.CultureInfo.Name?displayProperty=nameWithType> 调用 <xref:System.Reflection.Assembly.GetSatelliteAssembly%2A> 方法。

较高级别的 API 可能会提取低级别 API。

## <a name="algorithm"></a>算法

.NET Core 资源回退进程包含以下步骤：

1. 确定 `active` <xref:System.Runtime.Loader.AssemblyLoadContext> 实例。 在所有情况下，`active` 实例都是执行程序集的 <xref:System.Runtime.Loader.AssemblyLoadContext>。

2. `active` 实例尝试通过以下方式按优先级排序来加载请求的区域性的附属程序集：
    - 检查其缓存。
    - 检查当前正在执行程序集的目录，查找与请求的 <xref:System.Globalization.CultureInfo.Name?displayProperty=nameWithType>（例如 `es-MX`）匹配的子目录。

        > [!NOTE]
        > 3\.0 版之前的 .NET Core 中未实现此功能。
        >
        > [!NOTE]
        > 在 Linux 和 macOS 上，子目录区分大小写，并且必须是以下两种情况之一：
        >
        > - 完全匹配大小写。
        > - 为小写。

    - 如果 `active` 是 <xref:System.Runtime.Loader.AssemblyLoadContext.Default?displayProperty=nameWithType> 实例，则通过运行[默认附属（资源）程序集探测](default-probing.md#satellite-resource-assembly-probing)逻辑。

    - 调用 <xref:System.Runtime.Loader.AssemblyLoadContext.Load%2A?displayProperty=nameWithType> 函数。

    - 引发 <xref:System.Runtime.Loader.AssemblyLoadContext.Resolving?displayProperty=nameWithType> 事件。

    - 引发 <xref:System.AppDomain.AssemblyResolve?displayProperty=nameWithType> 事件。

3. 如果加载附属程序集：
   - 引发 <xref:System.AppDomain.AssemblyLoad?displayProperty=nameWithType> 事件。
   - 将搜索程序集以查找请求的资源。 如果运行时在程序集中找到该资源，则使用它。 如果找不到该资源，将继续搜索。

    > [!NOTE]
    > 要在附属程序集中查找资源，运行时将搜索 <xref:System.Resources.ResourceManager> 为当前 <xref:System.Globalization.CultureInfo.Name?displayProperty=nameWithType> 请求的资源文件。 在资源文件中，它搜索请求的资源名称。 如果找不到上述任何一个，则资源将被视为未找到。

4. 接下来，运行时通过许多潜在级别搜索父区域性程序集，每次均重复步骤 2 和 3。

    每个区域性只有一个父级，由 <xref:System.Globalization.CultureInfo.Parent%2A?displayProperty=nameWithType> 属性定义。

    当区域性的 <xref:System.Globalization.CultureInfo.Parent%2A> 属性为 <xref:System.Globalization.CultureInfo.InvariantCulture?displayProperty=nameWithType> 时，父区域性搜索将停止。

    对于 <xref:System.Globalization.CultureInfo.InvariantCulture>，我们不会返回到步骤 2 和 3，而是继续执行步骤 5。

5. 如果仍未找到资源，则使用默认（回退）区域性的资源。

   通常，默认区域性的资源包含在主应用程序集中。 不过，可以为 <xref:System.Resources.NeutralResourcesLanguageAttribute.Location?displayProperty=nameWithType> 属性指定 <xref:System.Resources.UltimateResourceFallbackLocation.Satellite?displayProperty=nameWithType>。 此值指示资源的最终回退位置是附属程序集，而不是主程序集。

    > [!NOTE]
    > 默认区域性为最终回退。 因此，建议在默认资源文件中始终包含一组详尽的资源。 这有助于防止引发异常。 通过拥有详尽资源集，可为所有资源提供回退，并确保始终向用户呈现至少一种资源，即使该资源不是特定于区域性的。

6. 最后，
   - 如果运行时找不到默认（回退）区域性的资源文件，将引发 <xref:System.Resources.MissingManifestResourceException> 或 <xref:System.Resources.MissingSatelliteAssemblyException> 异常。
   - 如果找到资源文件但是请求的资源不存在，则请求返回 `null`。
