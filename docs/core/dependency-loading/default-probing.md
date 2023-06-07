---
title: 默认探测 - .NET Core
description: 用于定位依赖项的 .NET Core 的 System.Runtime.Loader.AssemblyLoadContext.Default 探测逻辑的概述。
ms.date: 08/09/2019
author: sdmaclea
ms.author: stmaclea
ms.openlocfilehash: 13ce4c7de5f6ce1b76b2e61db810c0f19717540f
ms.sourcegitcommit: cbb19e56d48cf88375d35d0c27554d4722761e0d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/19/2020
ms.locfileid: "88608419"
---
# <a name="default-probing"></a>默认探测

<xref:System.Runtime.Loader.AssemblyLoadContext.Default%2A?displayProperty=nameWithType> 实例负责定位程序集的依赖项。 本文介绍 <xref:System.Runtime.Loader.AssemblyLoadContext.Default%2A?displayProperty=nameWithType> 实例的探测逻辑。

## <a name="host-configured-probing-properties"></a>主机配置的探测属性

运行时启动时，运行时主机提供一组命名的探测属性，这些属性可配置 <xref:System.Runtime.Loader.AssemblyLoadContext.Default%2A?displayProperty=nameWithType> 探测路径。

每个探测属性均可选。 如果存在，则每个属性都是一个字符串值，其中包含绝对路径的分隔列表。 在 Windows 上，分隔符为“;”，在所有其他平台上，分隔符为“:”。

|属性名                 |描述  |
|------------------------------|---------|
|`TRUSTED_PLATFORM_ASSEMBLIES`   | 平台和应用程序程序集文件路径的列表。 |
|`PLATFORM_RESOURCE_ROOTS`       | 用于搜索附属资源程序集的目录路径的列表。 |
|`NATIVE_DLL_SEARCH_DIRECTORIES` | 用于搜索非托管（本机）库的目录路径的列表。        |
|`APP_PATHS`                     | 用于搜索托管程序集的目录路径的列表。 |
|`APP_NI_PATHS`                  | 用于搜索托管程序集的本机映像的目录路径的列表。 |

### <a name="how-are-the-properties-populated"></a>如何填充属性？

填充属性有两个主要方案，具体取决于 \<myapp>.deps.json 文件是否存在。

- 当 \*.deps.json 文件存在时，将对其进行分析以填充探测属性  。
- 如果 \*.deps.json  文件不存在，则假定应用程序的目录以包含所有依赖项。 目录的内容用于填充探测属性。

此外，也会对任何引用框架的 \*.deps.json  文件进行类似的分析。

最后，可使用环境变量 `ADDITIONAL_DEPS` 添加其他依赖项。  `dotnet.exe` 还包含一个 `--additional-deps` 可选参数，用于在应用程序启动时设置此值。

默认不会填充 `APP_PATHS` 和 `APP_NI_PATHS` 属性，大多数应用程序都省略了它们。

可以通过 `System.AppContext.GetData("APP_CONTEXT_DEPS_FILES")` 访问应用程序使用的所有 \*.deps.json 文件的列表。

### <a name="how-do-i-see-the-probing-properties-from-managed-code"></a>如何查看托管代码中的探测属性？

通过使用上表中的属性名称调用 <xref:System.AppContext.GetData(System.String)?displayProperty=nameWithType> 函数，可查看每个属性。

### <a name="how-do-i-debug-the-probing-properties-construction"></a>如何调试探测属性的构造？

启用某些环境变量后，.NET Core 运行时主机将输出有用的跟踪消息：

|环境变量        |描述  |
|----------------------------|---------|
|`COREHOST_TRACE=1`          |启用跟踪。|
|`COREHOST_TRACEFILE=<path>` |跟踪文件路径而不是默认 `stderr`。|
|`COREHOST_TRACE_VERBOSITY`  |将详细程度设置为从 1（最低）到 4（最高）。|

## <a name="managed-assembly-default-probing"></a>托管程序集默认探测

当探测以定位托管程序集时，<xref:System.Runtime.Loader.AssemblyLoadContext.Default%2A?displayProperty=nameWithType> 会按以下顺序查找：

- 与 `TRUSTED_PLATFORM_ASSEMBLIES` 中的 <xref:System.Reflection.AssemblyName.Name?displayProperty=nameWithType> 匹配的文件（在删除文件扩展名之后）。
- 包含公共文件扩展名的 `APP_NI_PATHS` 中的本机映像程序集文件。
- 包含公共文件扩展名的 `APP_PATHS` 中的程序集文件。

## <a name="satellite-resource-assembly-probing"></a>附属（资源）程序集探测

若要查找特定区域性的附属程序集，请构造一组文件路径。

对于 `PLATFORM_RESOURCE_ROOTS` 和 `APP_PATHS` 中的每个路径，附加 <xref:System.Globalization.CultureInfo.Name?displayProperty=nameWithType> 字符串、目录分隔符、<xref:System.Reflection.AssemblyName.Name?displayProperty=nameWithType> 字符串和扩展名“.dll”。

如果存在任何匹配的文件，请尝试加载并返回该文件。

## <a name="unmanaged-native-library-probing"></a>非托管（本机）库探测

当探测以查找非托管库时，将搜索 `NATIVE_DLL_SEARCH_DIRECTORIES` 以查找匹配库。
