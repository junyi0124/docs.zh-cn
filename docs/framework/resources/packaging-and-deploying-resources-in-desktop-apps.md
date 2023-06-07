---
title: 打包和部署 .NET 应用中的资源
description: 使用主程序集（中枢）和附属程序集（轮辐）在 .NET 应用中打包和部署资源。 轮辐包含本地化资源，但不包含代码。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- deploying applications [.NET Framework], resources
- resource files, deploying
- hub-and-spoke resource deployment model
- resource files, packaging
- application resources, packaging
- single resource assembly
- satellite assemblies
- default culture for applications
- names [.NET Framework], resources
- fallback process for resources
- grouping cultures
- application resources, deploying
- application resources, naming conventions
- culture, packaging and deploying resources
- resource files, naming conventions
- packaging application resources
- application resources, fallback processes
- resource files, fallback processes
- localizing resources
- neutral cultures
ms.assetid: b224d7c0-35f8-4e82-a705-dd76795e8d16
ms.openlocfilehash: 056569f86afcbdf124f9e617e4ad07b09a194681
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90553995"
---
# <a name="packaging-and-deploying-resources-in-net-apps"></a>打包和部署 .NET 应用中的资源

应用程序依靠 .NET Framework Resource Manager（由 <xref:System.Resources.ResourceManager> 类表示）来检索已本地化的资源。 Resource Manager 假定使用中枢轮辐式模型来打包和部署资源。 中枢即主程序集，包含不可本地化的可执行代码和单个区域性（称作非特定区域性或默认区域性）的资源。 默认区域性是应用程序的回退区域性；如果找不到已本地化的资源，则使用默认区域性的资源。 每条轮辐均连接到一个附属程序集，该附属程序集包含单个区域性的资源，但不包含任何代码。

此模型有以下优点：

- 部署应用程序后，可针对新区域性以增量方式添加资源。 因为特定于区域性资源的后续开发可能需要大量时间，这使你能够先发布主要应用程序，并在以后发布特定于区域性的资源。
- 可更新和更改应用程序的附属程序集，无需重新编译应用程序。
- 应用程序只需加载那些包含特定区域性所需资源的附属程序集。 这可显著减少系统资源的使用。

 但是，该模型也有以下缺点：

- 必须管理多组资源。
- 测试应用程序的初始成本增加，因为必须测试多种配置。 请注意，从长远角度看，测试使用多个附属程序集测试多个核心应用程序会比测试并维护多个并行国际版本更轻松，成本更低。

## <a name="resource-naming-conventions"></a>资源命名约定

在打包应用程序的资源时，必须使用公共语言运行时所要求的资源命名约定对其进行命名。 运行时可按其区域性名称标识资源。 每个区域性均被赋予唯一名称，通常是与语言相关的两个小写字母的区域性名称和必要情况下，与国家或地区相关的两个大写子母的区域性名称的组合。 子区域性名称跟在区域性名称后，以短划线 (-) 隔开。 例如：ja-JP 表示日本日语，en-US 表示美国英语，de-DE 表示德国德语，de-AT 表示奥地利德语。 请参阅 [Windows 支持的语言/区域名称列表](/openspecs/windows_protocols/ms-lcid/a9eac961-e77d-41a6-90a5-ce1a8b0cdb9c)中的“语言标记”  列。 列名遵循 [BCP 47](https://tools.ietf.org/html/bcp47) 定义的标准。

> [!NOTE]
> 两字母区域性名称有一些例外，如表示中文（简体）的 `zh-Hans`。

> [!NOTE]
> 有关创建资源文件的信息，请参阅[创建资源文件](creating-resource-files-for-desktop-apps.md)和[创建附属程序集](creating-satellite-assemblies-for-desktop-apps.md)。

<a name="cpconpackagingdeployingresourcesanchor1"></a>

## <a name="the-resource-fallback-process"></a>资源回退进程

用于打包和部署资源的中枢轮辐式模型可使用回退过程来定位相应的资源。 如果应用程序请求不可用的已本地化资源，则公共语言运行时会在区域性层次结构中查找与用户应用程序请求最匹配的合适回退资源，并且只在最后迫不得已的情况下才引发异常。 如果在层次结构的任意级别找到了合适的资源，运行时就会使用该资源。 如果未找到资源，则将继续在下一级别中搜索。

若要提高查找性能，可将 <xref:System.Resources.NeutralResourcesLanguageAttribute> 属性应用到主程序集，并向其传递将与主程序集一起使用的非特定语言的名称。

### <a name="net-framework-resource-fallback-process"></a>.NET Framework 资源回退进程

.NET Framework 资源回退进程包含以下步骤：

> [!TIP]
> 可以使用 [\<relativeBindForResources>](../configure-apps/file-schema/runtime/relativebindforresources-element.md) 配置元素来优化资源回退过程和运行时针对资源程序集探测所依据的过程。 有关详细信息，请参阅 [优化资源回退进程](packaging-and-deploying-resources-in-desktop-apps.md#Optimizing)一节。

1. 运行时首先检查[全局程序集缓存](../app-domains/gac.md)，找到与为应用程序请求的区域性匹配的程序集。

     全局程序集缓存可存储由许多应用程序共享的资源程序集。 这使你无需在创建的每个应用程序的目录结构中包括特定的资源集。 如果运行时找到了对程序集的引用，它将搜索程序集，查找请求的资源。 如果在程序集中找到了该项，则使用请求的资源。 如果找不到该项，将继续搜索。

2. 接下来，运行时将检查当前正在执行程序集的目录，查找与请求的区域性匹配的子目录。 如果找到该子目录，它将搜索该子目录以找到请求的区域性的有效附属程序集。 然后，运行时将搜索附属程序集，寻找请求的资源。 如果在程序集中找到该资源，则使用它。 如果找不到该资源，将继续搜索。

3. 接下来，运行时将查询 Windows Installer，以确定是否要按需安装附属程序集。 如果是，它将处理安装，加载程序集，以及搜索它或搜索请求的资源。 如果在程序集中找到该资源，则使用它。 如果找不到该资源，将继续搜索。

4. 运行时引发 <xref:System.AppDomain.AssemblyResolve?displayProperty=nameWithType> 事件以指示找不到附属程序集。 如果选择对事件进行处理，事件处理程序可以返回对其资源将用于查找的附属程序集的引用。 否则，事件处理程序将返回 `null`，搜索继续。

5. 接下来，运行时将再次搜索全局程序集缓存，这次是为了请求的区域性的父程序集。 如果全局程序集缓存中存在父程序集，运行时将搜索程序集，寻找请求的资源。

     父区域性被定义为合适的回退区域性。 将父区域性视为回退候选项，因为提供任何资源都比引发异常更可取。 此过程还允许重复使用资源。 仅当子无需本地化所请求的资源时，才应在父级别包含特定资源。 例如，如果提供 `en`（非特定英语）的附属程序集：`en-GB`（英国英语）和 `en-US`（美国英语），则 `en` 附属程序集应包含公共术语，并且 `en-GB` 和 `en-US` 附属程序集可能只对那些不同的术语提供替代项。

6. 接下来，运行时将检查当前正在执行程序集的目录，查看是否包含父目录。 如果存在父目录，则运行时将搜索该目录，寻找父区域性的有效附属程序集。 如果找到该程序集，运行时将搜索程序集，寻找请求的资源。 如果找到该资源，则使用它。 如果找不到该资源，将继续搜索。

7. 接下来，运行时将查询 Windows Installer，以确定是否要按需安装父级附属程序集。 如果是，它将处理安装，加载程序集，以及搜索它或搜索请求的资源。 如果在程序集中找到该资源，则使用它。 如果找不到该资源，将继续搜索。

8. 运行时引发 <xref:System.AppDomain.AssemblyResolve?displayProperty=nameWithType> 事件以指示找不到合适的回退资源。 如果选择对事件进行处理，事件处理程序可以返回对其资源将用于查找的附属程序集的引用。 否则，事件处理程序将返回 `null`，搜索继续。

9. 接下来，如前前面三个步骤所述，运行时将通过许多可能的级别搜索父程序集。 每个区域性只有一个父区域性（由 <xref:System.Globalization.CultureInfo.Parent%2A?displayProperty=nameWithType> 属性定义），但一个父区域性可能还有其自己的父区域性。 如果区域性的 <xref:System.Globalization.CultureInfo.Parent%2A> 属性返回 <xref:System.Globalization.CultureInfo.InvariantCulture%2A?displayProperty=nameWithType>，父区域性搜索将停止；对于资源回退，固定区域性不被视为父区域性，也不被视为具有资源的区域性。

10. 如果区域性是最初指定的，且已搜索所有父级，但仍未找到资源，则使用默认（回退）区域性的资源。 通常，默认区域性的资源包含在主应用程序集中。 但是，可指定 <xref:System.Resources.NeutralResourcesLanguageAttribute> 特性的 <xref:System.Resources.NeutralResourcesLanguageAttribute.Location%2A> 属性的值为 <xref:System.Resources.UltimateResourceFallbackLocation.Satellite>，以指定资源的最终回退位置是附属程序集，而不是主程序集。

    > [!NOTE]
    > 默认资源是唯一可与主程序集一起编译的资源。 除非通过使用 <xref:System.Resources.NeutralResourcesLanguageAttribute> 属性指定附属程序集，否则为最终回退（最终父级）。 因此，建议在主程序集中始终包含一组默认资源。 这有助于防止引发异常。 通过包含默认资源文件，可为所有资源提供回退，并确保始终向用户呈现至少一种资源，即使该资源不是特定于区域性的。

11. 最后，如果运行时找不到默认（回退）区域性的资源，将引发 <xref:System.Resources.MissingManifestResourceException> 或 <xref:System.Resources.MissingSatelliteAssemblyException> 异常，指示找不到该资源。

例如，假定应用程序请求本地化为墨西哥西班牙语（`es-MX` 区域性）所需的资源。 运行时将首先搜索全局程序集缓存，寻找匹配 `es-MX` 的程序集，但找不到。 然后，运行时将搜索当前正在执行的程序集，寻找 `es-MX` 目录。 如果失败，运行时将再次搜索全局程序集缓存，寻找反映相应回退区域性的父程序集 — 在本例中为 `es`（西班牙语）。 如果找不到父程序集，运行时将针对 `es-MX` 区域性搜索父程序集的所有可能级别，直到它找到对应的资源。 如果找不到资源，运行时将使用默认区域性的资源。

<a name="Optimizing"></a>

#### <a name="optimizing-the-net-framework-resource-fallback-process"></a>优化 .NET Framework 资源回退进程

在下列情况下，可以按运行时搜索附属程序集中的资源所依据的内容优化进程

- 附属程序集部署在与代码程序集相同的位置。 如果代码程序集安装在[全局程序集缓存](../app-domains/gac.md)中，则附属程序集也会安装到全局程序集缓存中。 如果代码程序集安装在一个目录中，则附属程序集安装在该目录的特定于区域性的文件夹中。

- 附属程序集不会按需进行安装。

- 应用程序代码不会处理 <xref:System.AppDomain.AssemblyResolve?displayProperty=nameWithType> 事件。

可通过在应用程序配置文件中包含 [\<relativeBindForResources>](../configure-apps/file-schema/runtime/relativebindforresources-element.md) 元素并将其 `enabled` 属性设为 `true`，优化附属程序集探测，如下例所示。

```xml
<configuration>
   <runtime>
      <relativeBindForResources enabled="true" />
   </runtime>
</configuration>
```

优化的附属程序集探测是选择加入功能。 也就是说，运行时遵循[资源回退过程](packaging-and-deploying-resources-in-desktop-apps.md#cpconpackagingdeployingresourcesanchor1)中记录的步骤，除非 [\<relativeBindForResources>](../configure-apps/file-schema/runtime/relativebindforresources-element.md) 元素位于应用程序的配置文件中，并且其 `enabled` 属性已设为 `true`。 如果出现这种情况，附属程序集探测过程将进行如下修改：

- 运行时使用父代码程序集位置来探测附属程序集。 如果父程序集安装在全局程序集缓存中，则运行时在缓存，而不是在应用程序的目录中探测。 如果父程序集安装在应用程序目录中，则运行时在应用程序目录，而不是在全局程序集缓存中探测。

- 运行时不会查询 Windows Installer，寻找附属程序集的按需安装。

- 如果特定资源程序集探测失败，运行时将不会引发 <xref:System.AppDomain.AssemblyResolve?displayProperty=nameWithType> 事件。

### <a name="net-core-resource-fallback-process"></a>.NET Core 资源回退进程

.NET Core 资源回退进程包含以下步骤：

1. 运行时尝试加载请求的区域性的附属程序集。
     - 检查当前正在执行程序集的目录，查找与请求的区域性匹配的子目录。 如果找到该子目录，它将搜索该子目录以找到请求的区域性的有效附属程序集并加载该程序集。

       > [!NOTE]
       > 在具有区分大小写的文件系统（即 Linux 和 macOS）的操作系统上，区域性名称子目录搜索区分大小写。 子目录名称必须与 <xref:System.Globalization.CultureInfo.Name?displayProperty=nameWithType> 的大小写完全匹配（例如，`es` 或 `es-MX`）。

       > [!NOTE]
       > 如果程序员从 <xref:System.Runtime.Loader.AssemblyLoadContext> 派生了自定义程序集加载上下文，则情况会很复杂。 如果将正在执行程序集加载到自定义上下文中，则运行时会将附属程序集加载到自定义上下文中。 相关详细信息超出了本文档所涉及的范围。 请参阅<xref:System.Runtime.Loader.AssemblyLoadContext>。

     - 如果未找到附属程序集，<xref:System.Runtime.Loader.AssemblyLoadContext> 引发 <xref:System.Runtime.Loader.AssemblyLoadContext.Resolving?displayProperty=nameWithType> 事件以指示找不到附属程序集。 如果选择对事件进行处理，事件处理程序可以加载引用并返回对附属程序集的引用。
     - 如果仍未找到附属程序集，则 AssemblyLoadContext 会导致 AppDomain 触发 <xref:System.AppDomain.AssemblyResolve?displayProperty=nameWithType> 事件以指示找不到附属程序集。 如果选择对事件进行处理，事件处理程序可以加载引用并返回对附属程序集的引用。

2. 如果找到附属程序集，则运行时将搜索请求的资源的附属程序集。 如果在程序集中找到该资源，则使用它。 如果找不到该资源，将继续搜索。

     > [!NOTE]
     > 要在附属程序集中查找资源，运行时将搜索 <xref:System.Resources.ResourceManager> 为当前 <xref:System.Globalization.CultureInfo.Name?displayProperty=nameWithType> 请求的资源文件。 在资源文件中，它搜索所请求的资源名称。 如果找不到上述任何一个，则资源将被视为未找到。

3. 接下来，运行时通过许多潜在级别搜索父区域性程序集，每次均重复步骤 1 和 2。

     父区域性被定义为合适的回退区域性。 将父区域性视为回退候选项，因为提供任何资源都比引发异常更可取。 此过程还允许重复使用资源。 仅当子无需本地化所请求的资源时，才应在父级别包含特定资源。 例如，如果提供 `en`（非特定英语）的附属程序集：`en-GB`（英国英语）和 `en-US`（美国英语），则 `en` 附属程序集应包含公共术语，并且 `en-GB` 和 `en-US` 附属程序集只对那些不同的术语提供替代项。

     每个区域性只有一个父区域性（由 <xref:System.Globalization.CultureInfo.Parent%2A?displayProperty=nameWithType> 属性定义），但一个父区域性可能还有其自己的父区域性。 当区域性的 <xref:System.Globalization.CultureInfo.Parent%2A> 属性返回 <xref:System.Globalization.CultureInfo.InvariantCulture%2A?displayProperty=nameWithType> 时，父区域性搜索将停止。 对于资源回退，固定区域性不被视为父区域性，也不被视为具有资源的区域性。

4. 如果区域性是最初指定的，且已搜索所有父级，但仍未找到资源，则使用默认（回退）区域性的资源。 通常，默认区域性的资源包含在主应用程序集中。 但是，可指定 <xref:System.Resources.NeutralResourcesLanguageAttribute.Location%2A> 属性的值为 <xref:System.Resources.UltimateResourceFallbackLocation.Satellite?displayProperty.nameWithType>，以指示资源的最终回退位置是附属程序集，而不是主程序集。

    > [!NOTE]
    > 默认资源是唯一可与主程序集一起编译的资源。 除非通过使用 <xref:System.Resources.NeutralResourcesLanguageAttribute> 属性指定附属程序集，否则为最终回退（最终父级）。 因此，建议在主程序集中始终包含一组默认资源。 这有助于防止引发异常。 通过包含默认资源文件，可为所有资源提供回退，并确保始终向用户呈现至少一种资源，即使该资源不是特定于区域性的。

5. 最后，如果运行时找不到默认（回退）区域性的资源文件，将引发 <xref:System.Resources.MissingManifestResourceException> 或 <xref:System.Resources.MissingSatelliteAssemblyException> 异常，指示找不到该资源。 如果找到资源文件但是请求的资源不存在，则请求返回 `null`。

### <a name="ultimate-fallback-to-satellite-assembly"></a>最终回退到附属程序集

可选择从主程序集中删除资源，并指定运行时应加载对应于特定区域性的附属程序集中的最终回退资源。 若要控制回退进程，请使用 <xref:System.Resources.NeutralResourcesLanguageAttribute.%23ctor%28System.String%2CSystem.Resources.UltimateResourceFallbackLocation%29> 构造函数并提供 <xref:System.Resources.UltimateResourceFallbackLocation> 参数的值，用于指定 Resource Manager 是应从主程序集还是应从附属程序集中提取回退资源。

下面的 .NET Framework 示例使用 <xref:System.Resources.NeutralResourcesLanguageAttribute> 属性将应用程序回退资源存储在法语 (`fr`) 语言的附属程序集中。 本示例介绍了两个基于文本的资源文件，这两个文件用于定义名为 `Greeting` 的单个字符串资源。 第一个文件 resources.fr.txt 包含法语资源。

```text
Greeting=Bon jour!
```

第二个文件 resources.ru.txt 包含俄语资源。

```text
Greeting=Добрый день
```

从命令行运行[资源文件生成器 (Resgen.exe)](../tools/resgen-exe-resource-file-generator.md) 可将这两个文件编译为 .resources 文件。 对于法语资源，命令为：

resgen.exe resources.fr.txt

对于俄语资源，命令为：

resgen.exe resources.ru.txt

对于法语资源，从命令行运行[程序集连接器 (Al.exe)](../tools/al-exe-assembly-linker.md)，将 .resources 文件嵌入动态链接库，如下所示：

al /t:lib /embed:resources.fr.resources /culture:fr /out:fr\Example1.resources.dll

而对于俄语资源，则为如下所示：

al /t:lib /embed:resources.ru.resources /culture:ru /out:ru\Example1.resources.dll

应用程序源代码位于名为 Example1.cs 或 Example1.vb 的文件中。 它包括 <xref:System.Resources.NeutralResourcesLanguageAttribute> 属性，以指示默认应用程序资源位于 fr 子目录中。 它可实例化 Resource Manager，检索 `Greeting` 资源的值，并将其显示到控制台。

[!code-csharp[Conceptual.Resources.Packaging#1](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.resources.packaging/cs/example1.cs#1)]
[!code-vb[Conceptual.Resources.Packaging#1](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.resources.packaging/vb/example1.vb#1)]

然后，可以从命令行编译 C# 源代码，如下所示：

```console
csc Example1.cs
```

用于 Visual Basic 编译器的命令十分相似：

```console
vbc Example1.vb
```

由于没有嵌入在主程序集中的资源，因此无需使用 `/resource` 切换进行编译。

当从不是俄语的任何系统运行示例时，它将显示以下输出：

```output
Bon jour!
```

## <a name="suggested-packaging-alternative"></a>建议的打包替代项

由于时间或预算约束，可能无法为应用程序支持的每个子区域性均创建一组资源。 但可以为所有相关子区域性可用的父区域性创建单个附属程序集。 例如，可以提供单个英语附属程序集 (en)，请求特定于区域的英语资源的用户将检索该程序集，并且为请求特定于区域的德语资源的用户创建单个德语附属程序集 (de)。 例如，对德国德语 (de-DE)、奥地利德语 (de-AT) 和瑞士德语 (de-CH) 的请求均会回退到德语附属程序集 (de)。 默认资源是最终回退资源，因而应是大多数应用程序用户将请求的资源，因此应仔细选择这些资源。 此方法可部署区域性特定性较低，但可显著减少应用程序本地化成本的资源。

## <a name="see-also"></a>请参阅

- [桌面应用中的资源](index.md)
- [全局程序集缓存](../app-domains/gac.md)
- [创建资源文件](creating-resource-files-for-desktop-apps.md)
- [创建附属程序集](creating-satellite-assemblies-for-desktop-apps.md)
