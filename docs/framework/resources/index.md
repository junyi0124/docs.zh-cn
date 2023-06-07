---
title: .NET 应用中的资源
description: 了解 .NET 应用中的资源。 资源是在逻辑上随应用一起部署的任何不可执行的数据。
ms.date: 07/25/2018
helpviewer_keywords:
- deploying applications [.NET Framework], resources
- deploying applications [.NET Core], resources
- application resources
- resource files
- satellite assemblies
- localization
- packaging application resources
- localizing resources
ms.assetid: 8ad495d4-2941-40cf-bf64-e82e85825890
ms.openlocfilehash: 3941c912348a652ddcbaea517cf7626c2a8f88e3
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90535565"
---
# <a name="resources-in-net-apps"></a>.NET 应用中的资源

几乎每一个生产性应用都需要使用资源。 资源是在逻辑上随应用部署的任何不可执行的数据。 资源可以在应用中作为错误消息显示，或者作为用户界面的一部分显示。 资源可以包含多种形式的数据，包括字符串、图像和持久的对象。 （持久化对象必须是可序列化的，才能将这些对象写入到资源文件。）通过在资源文件中存储数据，可以更改这些数据，而无需重新编译整个应用。 还可以将数据存储在一个位置，而无需依赖存储在多个位置的硬编码数据。

.NET Framework 和 .NET Core 为资源的创建和本地化提供全面的支持。 此外，.NET 还支持一种用于打包和部署本地化资源的简单模型。

有关 ASP.NET 中的资源的信息，请参阅 [ASP.NET 网页资源概述](/previous-versions/aspnet/ms227427(v=vs.100))。

## <a name="create-and-localize-resources"></a>创建和本地化资源

在非本地化的应用中，可以使用资源文件作为应用数据的存储库，特别用于存储本来可能在源代码中的多个位置为硬编码的字符串。 通常以文本 (.txt) 或 XML (.resx) 文件形式创建资源，并使用 [Resgen.exe（资源文件生成器）](../tools/resgen-exe-resource-file-generator.md)将其编译为二进制 .resources 文件。 随后，这些文件可由语言编译器嵌入到应用的可执行文件中。 有关创建资源的详细信息，请参阅[创建资源文件](creating-resource-files-for-desktop-apps.md)。

您还可以按特定的区域性对应用的资源进行本地化。 这样可以生成应用的本地化（翻译）版本。 在开发使用本地化资源的应用时，可以指定一个区域性作为非特定或回退区域性，以在没有合适的资源可用时使用该区域性的资源。 通常，非特定区域性的资源存储在应用的可执行文件中。 其余各本地化区域性的资源存储在单独的附属程序集中。 有关详细信息，请参阅[创建附属程序集](creating-satellite-assemblies-for-desktop-apps.md)。

## <a name="package-and-deploy-resources"></a>打包和部署资源

本地化的应用资源部署在[附属程序集](packaging-and-deploying-resources-in-desktop-apps.md)中。 附属程序集包含单个区域性的资源；它不包含任何应用代码。 在附属程序集部署模型中，所创建的应用包含一个默认程序集（通常是主程序集），对于该应用支持的每种区域性，还包含一个附属程序集。 因为附属程序集不是主程序集的一部分，所以您不必替换该应用的主程序集，即可很容易地替换或更新与特定区域性相对应的资源。

在确定哪些资源将构成应用的默认资源程序集时要谨慎。 因为默认资源程序集是主程序集的一部分，所以对它做任何更改都会要求你替换主程序集。 如果没有提供默认资源，则在[资源回退进程](packaging-and-deploying-resources-in-desktop-apps.md)尝试查找默认资源时会引发异常。 在设计良好的应用中，使用资源应永远不会引发异常。

有关详细信息，请参阅[打包和部署资源](packaging-and-deploying-resources-in-desktop-apps.md)一文。

## <a name="retrieve-resources"></a>检索资源

在运行时，应用会基于 <xref:System.Globalization.CultureInfo.CurrentUICulture%2A?displayProperty=nameWithType> 属性指定的区域性为每个线程加载相应的本地化资源。 此属性的值按如下方式派生：

- 向 <xref:System.Globalization.CultureInfo> 属性直接分配一个表示本地化区域性的 <xref:System.Threading.Thread.CurrentUICulture%2A?displayProperty=nameWithType> 对象。

- 如果没有明确分配区域性，则从 <xref:System.Globalization.CultureInfo.DefaultThreadCurrentUICulture%2A?displayProperty=nameWithType> 属性检索默认线程 UI 区域性。

- 如果没有明确分配默认线程 UI 区域性，则在本地计算机上检索当前用户的区域性。 在 Windows 上运行的 .NET 实现通过调用 Windows [`GetUserDefaultUILanguage`](/windows/desktop/api/winnls/nf-winnls-getuserdefaultuilanguage) 函数来完成此操作。

有关如何设置当前 UI 区域性的详细信息，请参阅 <xref:System.Globalization.CultureInfo> 和 <xref:System.Globalization.CultureInfo.CurrentUICulture%2A?displayProperty=nameWithType> 参考页。

随后，通过使用 <xref:System.Resources.ResourceManager?displayProperty=nameWithType> 类，可以为当前的 UI 区域性或特定区域性检索资源。 虽然 <xref:System.Resources.ResourceManager> 类最常用于检索资源，但 <xref:System.Resources?displayProperty=nameWithType> 命名空间包含可用于检索资源的其他类型。 这些方法包括：

- <xref:System.Resources.ResourceReader> 类 - 可用于枚举嵌入在程序集或存储于独立二进制 .resources 文件中的资源。 当您不知道运行时可用资源的准确名称时，这将会很有用。

- <xref:System.Resources.ResXResourceReader> 类 - 可用于从 XML (.resx) 文件中检索资源。

- <xref:System.Resources.ResourceSet> 类 - 可用于检索特定区域性的资源，而不遵循回退规则。 资源可以存储在程序集或独立的二进制 .resources 文件中。 您还可以开发 <xref:System.Resources.IResourceReader> 实现以使用 <xref:System.Resources.ResourceSet> 类从某个其他源中检索资源。

- <xref:System.Resources.ResXResourceSet> 类 - 可用于将 XML 资源文件中的所有项目都检索到内存中。

## <a name="see-also"></a>请参阅

- <xref:System.Globalization.CultureInfo>
- <xref:System.Globalization.CultureInfo.CurrentUICulture%2A?displayProperty=nameWithType>
- [创建资源文件](creating-resource-files-for-desktop-apps.md)
- [打包和部署资源](packaging-and-deploying-resources-in-desktop-apps.md)
- [创建附属程序集](creating-satellite-assemblies-for-desktop-apps.md)
- [检索资源](retrieving-resources-in-desktop-apps.md)
