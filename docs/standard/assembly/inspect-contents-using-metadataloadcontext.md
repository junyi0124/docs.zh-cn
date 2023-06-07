---
title: 如何：使用 MetadataLoadContext 检查程序集内容
description: 了解如何使用 MetadataLoadContext（可实现加载的 API）加载要检查的 .NET 程序集。
author: MSDN-WhiteKnight
ms.date: 03/10/2020
ms.openlocfilehash: 7205230986aa852813a651d2fcb7c5ef88ab18fe
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94831346"
---
# <a name="how-to-inspect-assembly-contents-using-metadataloadcontext"></a>如何：使用 MetadataLoadContext 检查程序集内容

默认情况下，开发人员可以使用 .NET 中的反射 API 检查加载到主执行上下文的程序集内容。 不过，有时无法将程序集加载到执行上下文，例如当程序集是面向其他平台或处理器体系结构编译时，或者它是[引用程序集](reference-assemblies.md)时。 借助 <xref:System.Reflection.MetadataLoadContext?displayProperty=fullName> API，可以加载并检查此类程序集。 加载到 <xref:System.Reflection.MetadataLoadContext> 的程序集仅被作为元数据处理，即可以检查此程序集中的类型，但无法执行其中包含的任何代码。 <xref:System.Reflection.MetadataLoadContext> 不同于主执行上下文，它不会自动加载当前目录中的依赖项；而是使用传递给它的 <xref:System.Reflection.MetadataAssemblyResolver> 所提供自定义绑定逻辑。

## <a name="prerequisites"></a>先决条件

安装 [System.Reflection.MetadataLoadContext](https://www.nuget.org/packages/System.Reflection.MetadataLoadContext) NuGet 包，以使用 <xref:System.Reflection.MetadataLoadContext>。 任意 .NET Standard 2.0 兼容的目标框架均支持它，如 NET Core 2.0 或 .NET Framework 4.6.1。

## <a name="create-metadataassemblyresolver-for-metadataloadcontext"></a>创建 MetadataLoadContext 的 MetadataAssemblyResolver

创建 <xref:System.Reflection.MetadataLoadContext> 需要提供 <xref:System.Reflection.MetadataAssemblyResolver> 的实例。 提供此实例的最简单方法是使用 <xref:System.Reflection.PathAssemblyResolver>，它会从给定程序集路径字符串集合解析程序集。 除了包含你想要直接检查的程序集之外，此集合还应包括所有所需的依赖项。 例如，若要读取位于外部程序集中的自定义属性，则应包含该程序集，否则将引发异常。 多数情况下，应至少包含核心程序集，即包含内置系统类型（如 <xref:System.Object?displayProperty=nameWithType>）的程序集。  下面的代码显示如何使用由检查的程序集和当前运行时的核心程序集组成的集合创建 <xref:System.Reflection.PathAssemblyResolver>：

[!code-csharp[](snippets/inspect-contents-using-metadataloadcontext/MetadataLoadContextSnippets.cs#CoreAssembly)]

如需访问所有 BCL 类型，可以包含此集合中的所有运行时程序集。 下面的代码显示如何使用由检查的程序集和当前运行时的所有程序集组成的集合创建 <xref:System.Reflection.PathAssemblyResolver>：

[!code-csharp[](snippets/inspect-contents-using-metadataloadcontext/MetadataLoadContextSnippets.cs#RuntimeAssemblies)]

## <a name="create-metadataloadcontext"></a>创建 MetadataLoadContext

若要创建 <xref:System.Reflection.MetadataLoadContext>请调用它的构造函数 <xref:System.Reflection.MetadataLoadContext.%23ctor%28System.Reflection.MetadataAssemblyResolver%2CSystem.String%29>，将之前创建的 <xref:System.Reflection.MetadataAssemblyResolver> 作为第一个参数并将核心程序集名称作为第二个参数传递。 可以省略核心程序集名称，在这种情况下，该构造函数将试图使用默认名称：“mscorlib”、“System.Runtime”或“netstandard”。

创建上下文后，可以使用 <xref:System.Reflection.MetadataLoadContext.LoadFromAssemblyPath%2A> 等方法将程序集加载到上下文中。 可以将所有反射 API 用于加载的程序集，涉及代码执行的 API 除外。 <xref:System.Reflection.MemberInfo.GetCustomAttributes%2A> 方法涉及执行构造函数，因此在需要检查 <xref:System.Reflection.MetadataLoadContext> 中的自定义属性时改为使用 <xref:System.Reflection.MemberInfo.GetCustomAttributesData%2A> 方法。

下面的代码示例创建 <xref:System.Reflection.MetadataLoadContext>，将程序集加载到其中，并将程序集属性输出到控制台：

[!code-csharp[](snippets/inspect-contents-using-metadataloadcontext/MetadataLoadContextSnippets.cs#CreateContext)]

## <a name="example"></a>示例

若要查看完整的代码示例，请参阅[使用 MetadataLoadContext 检查程序集内容示例](/samples/dotnet/samples/inspect-assembly-contents-using-metadataloadcontext/)。
