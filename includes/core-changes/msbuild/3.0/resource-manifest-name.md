---
ms.openlocfilehash: f24a29a00a1bff34a452c43716d76bf72ef277b5
ms.sourcegitcommit: 488aced39b5f374bc0a139a4993616a54d15baf0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/12/2020
ms.locfileid: "83206215"
---
### <a name="resource-manifest-file-name-change"></a>资源清单文件名更改

从 .NET Core 3.0 开始，默认情况下，MSBuild 会为资源文件生成不同的清单文件名。

#### <a name="version-introduced"></a>引入的版本

3.0

#### <a name="change-description"></a>更改描述

在 .NET Core 3.0 之前，如果没有为项目文件中的 `EmbeddedResource` 项指定 `LogicalName`、`ManifestResourceName` 或 `DependentUpon` 元数据，则 MSBuild 会在 `<RootNamespace>.<ResourceFilePathFromProjectRoot>.resources` 模式中生成清单文件名。 如果未在项目文件中定义 `RootNamespace`，则其默认为项目名称。 例如，根项目目录中名为“Form1.resx”的资源文件的生成清单名称是“MyProject.Form1.resources”。

从 .NET Core 3.0 开始，如果资源文件与同名的源文件（例如 Form1.resx 和 Form1.cs）并置，则 MSBuild 将使用源文件中的类型信息在 `<Namespace>.<ClassName>.resources` 模式中生成清单文件名。 命名空间和类名称是从并置源文件的第一个类型中提取的。 例如，与名为“Form1.cs”的源文件并置的、名为“Form1.resx”的资源文件的生成清单名称是“MyNamespace.Form1.resources”。 需要注意的一点是，文件名的第一部分不同于早期版本的 .NET Core（是 MyNamespace，而不是 MyProject）。

> [!NOTE]
> 如果已在项目文件中的 `EmbeddedResource` 项上指定 `LogicalName`、`ManifestResourceName` 或 `DependentUpon` 元数据，则此更改不会影响该资源文件。

此重大更改是在 .NET Core 项目中添加 `EmbeddedResourceUseDependentUponConvention` 属性时引入的。 默认情况下，不会在 .NET Core 项目文件中显式列出资源文件，因此它们没有 `DependentUpon` 元数据来指定如何命名生成的 .resources 文件。 如果 `EmbeddedResourceUseDependentUponConvention` 设置为 `true`（默认值），则 MSBuild 将查找并置的源文件，并从该文件中提取命名空间和类名。 如果将 `EmbeddedResourceUseDependentUponConvention` 设置为 `false`，则 MSBuild 将根据之前的行为生成清单名称，将 `RootNamespace` 和相对文件路径组合在一起。

#### <a name="recommended-action"></a>建议操作

在大多数情况下，开发人员不需要执行任何操作，应用应可以继续工作。 但是，如果此更改造成应用中断运行，你可以：

- 将代码更改为需要新的清单名称。

- 在项目文件中将 `EmbeddedResourceUseDependentUponConvention` 设置为 `false`，以选择退出新命名约定。

  ```xml
  <PropertyGroup>
    <EmbeddedResourceUseDependentUponConvention>false</EmbeddedResourceUseDependentUponConvention>
  </PropertyGroup>
  ```

#### <a name="category"></a>类别

MSBuild

#### <a name="affected-apis"></a>受影响的 API

不可用
