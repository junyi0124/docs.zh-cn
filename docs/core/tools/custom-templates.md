---
title: dotnet new 自定义模板
description: 了解任意类型 .NET 项目或文件的自定义模板。
author: adegeo
ms.date: 05/20/2020
ms.openlocfilehash: 3995fad864b80d024209c723a0197281e1b0f523
ms.sourcegitcommit: b201d177e01480a139622f3bf8facd367657a472
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2020
ms.locfileid: "94634176"
---
# <a name="custom-templates-for-dotnet-new"></a>dotnet new 自定义模板

[.NET SDK](https://dotnet.microsoft.com/download) 随附了许多已安装且可供使用的模板。 [`dotnet new` 命令](dotnet-new.md)不仅用于使用模板，还用于说明如何安装和卸载模板。 可以为任何类型的项目（如应用、服务、工具或类库）创建自己的自定义模板。 甚至可以创建输出一个或多个独立文件（如配置文件）的模板。

可以从任何 NuGet 源上的 NuGet 包安装自定义模板，具体方法是直接引用 NuGet .nupkg 文件，或指定包含模板的文件系统目录。 借助模板引擎提供的功能，可以替换值、包括和排除文件，并在使用模板时执行自定义处理操作。

模板引擎是开放源代码，在线代码存储库位于 GitHub 上的 [dotnet/templating](https://github.com/dotnet/templating/)。 GitHub 上的 [dotnet new 可用模板](https://github.com/dotnet/templating/wiki/Available-templates-for-dotnet-new)收录了更多模板，包括第三方模板。 若要详细了解如何创建和使用自定义模板，请参阅[如何创建自己的 dotnet new 模板](https://devblogs.microsoft.com/dotnet/how-to-create-your-own-templates-for-dotnet-new/)和 [dotnet/templating GitHub 存储库 Wiki](https://github.com/dotnet/templating/wiki)。

> [!NOTE]
> [dotnet/dotnet-template-samples](https://github.com/dotnet/dotnet-template-samples) GitHub 存储库提供了模板示例。 尽管这些示例是了解模板工作方式的良好资源，但该存储库已存档且不再维护。 这些例子可能已过期且不再有效。

若要按照演示步骤操作并创建模板，请参阅[创建 dotnet new 自定义模板](../tutorials/cli-templates-create-item-template.md)教程。

### <a name="net-default-templates"></a>.NET 默认模板

安装 [.NET SDK](https://dotnet.microsoft.com/download) 时，将收到十多个用于创建项目和文件的内置模板，包括控制台应用、类库、单元测试项目、ASP.NET Core 应用（包括 [Angular](https://angular.io/) 和 [React](https://reactjs.org/) 项目）和配置文件。 若要列出内置模板，请运行带有 `-l|--list` 选项的 `dotnet new` 命令：

```dotnetcli
dotnet new --list
```

## <a name="configuration"></a>Configuration

模板由以下部分组成：

- 源文件和文件夹。
- 配置文件 (template.json)。

### <a name="source-files-and-folders"></a>源文件和文件夹

源文件和文件夹包含运行 `dotnet new <TEMPLATE>` 命令时用户希望模板引擎使用的任何文件和文件夹。 模板引擎旨在将可运行项目用作源代码，以生成项目。 这样做有以下几个好处：

- 模板引擎不要求用户将特殊令牌注入项目的源代码。
- 代码文件不必是特殊文件，也不必以任何方式进行修改，即可与模板引擎配合使用。 因此，处理项目时通常使用的工具也适用于模板内容。
- 生成、运行和调试模板项目，就像生成、运行和调试其他任何项目一样。
- 只需将 ./.template.config/template.json 配置文件添加到项目，即可通过现有项目快速创建模板。

模板中存储的文件和文件夹并不限于正式的 .NET 项目类型。 源文件和文件夹可能包含用户希望在使用模板时创建的任何内容，即使模板引擎仅生成一个文件作为输出也不例外。

可以基于在 template.json 配置文件中提供的逻辑和设置对模板生成的文件进行修改。 用户可以将选项传递到 `dotnet new <TEMPLATE>` 命令以覆盖这些设置。 自定义逻辑的一个常见示例为模板部署的代码文件中的类或变量提供名称。

### <a name="templatejson"></a>template.json

template.json 文件位于模板根目录中的 .template.config 文件夹。 此文件向模板引擎提供配置信息。 最低配置必须包含下表中列出的成员，这足以创建功能模板。

| 成员            | 类型          | 描述 |
| ----------------- | ------------- | ----------- |
| `$schema`         | URI           | template.json 文件的 JSON 架构。 如果指定架构，支持 JSON 架构的编辑器启用 JSON 编辑功能。 例如，[Visual Studio Code](https://code.visualstudio.com/) 要求此成员启用 IntelliSense。 使用值 `http://json.schemastore.org/template`。 |
| `author`          | string        | 模板创建者。 |
| `classifications` | array(string) | 为了找到模板，用户可能会在搜索模板时使用的 0 个或多个模板特征。 如果出现在使用 `dotnet new -l|--list` 命令生成的模板列表中，classifications 还会出现在“Tags”列中。 |
| `identity`        | string        | 此模板的唯一名称。 |
| `name`            | string        | 用户应看到的模板名称。 |
| `shortName`       | string        | 方便用户选择模板的默认速记名称，适用于模板名称由用户指定（而不是通过 GUI 选择）的环境。 例如，通过命令提示符和 CLI 命令使用模板时，短名称非常有用。 |
| `sourceName`       | 字符串        | 源树中的名称，它即将替换为用户指定的名称。 模板引擎将查找配置文件中提及并出现的任何 `sourceName`，并将其替换为文件名和文件内容。 可以在运行模板时使用 `-n` 或 `--name` 选项提供要替换的值。 如果未指定名称，则使用的是当前目录。|
| `preferNameDirectory`       | boolean        | （如果指定了名称，但未设置输出目录）指示是否为模板创建目录（而不是直接在当前目录中创建内容）。 默认值是 False。|

template.json 文件的完整架构位于 [JSON 架构存储](http://json.schemastore.org/template)。 有关 template.json 文件的详细信息，请参阅 [dotnet 创建模板 wiki](https://github.com/dotnet/templating/wiki)。

#### <a name="example"></a>示例

例如，下面是包含两个内容文件的模板文件夹：console.cs 和 readme.txt。 请注意，其中有包含 template.json 文件的名为 .template.config 的所需文件夹。

```text
└───mytemplate
    │   console.cs
    │   readme.txt
    │
    └───.template.config
            template.json
```

template.json 文件如下所示：

```json
{
  "$schema": "http://json.schemastore.org/template",
  "author": "Travis Chau",
  "classifications": [ "Common", "Console" ],
  "identity": "AdatumCorporation.ConsoleTemplate.CSharp",
  "name": "Adatum Corporation Console Application",
  "shortName": "adatumconsole"
}
```

mytemplate 文件夹是可安装的模板包。 安装此包后，`shortName` 可与 `dotnet new` 命令结合使用。 例如，`dotnet new adatumconsole` 会将 `console.cs` 和 `readme.txt` 文件输出到当前文件夹。

## <a name="packing-a-template-into-a-nuget-package-nupkg-file"></a>将模板打包到 NuGet 包（nupkg 文件）

自定义模板与 [dotnet pack](dotnet-pack.md) 命令和 .csproj 文件一起打包。 或者，[NuGet](/nuget/tools/nuget-exe-cli-reference) 可与 [nuget pack](/nuget/tools/cli-ref-pack) 命令以及 .nuspec 文件一起使用。 但是，NuGet 在 Windows 上需要 .NET Framework，在 Linux 和 macOS 上需要 [Mono](https://www.mono-project.com/)。

该 .csproj 文件与传统代码项目 .csproj 文件略有不同。 请注意以下设置：

01. 添加 `<PackageType>` 设置并将其设为 `Template`。
01. 添加 `<PackageVersion>` 设置并将其设为有效的 [NuGet 版本号](/nuget/reference/package-versioning)。
01. 添加 `<PackageId>` 设置并将其设为唯一标识符。 此标识符用于卸载模板包，NuGet 源用它来注册你的模板包。
01. 应设置泛型元数据设置：`<Title>`、`<Authors>`、`<Description>` 和 `<PackageTags>`。
01. 必须设置 `<TargetFramework>` 设置，即使未使用模板过程生成的二进制文件也必须设置。 在下面的示例中，它设置为 `netstandard2.0`。

.nupkg NuGet 包形式的模板包要求所有模板都存储在包中的 content 文件夹中。 还有几个设置将添加到 .csproj 文件以确保生成的 .nupkg 作为模板包安装：

01. `<IncludeContentInPack>` 设置设为 `true` 以包含项目在 NuGet 包中设为“内容”的任何文件。
01. `<IncludeBuildOutput>` 设置设为 `false` 以从 NuGet 包排除编译器生成的所有二进制文件。
01. `<ContentTargetFolders>` 设置设为 `content`。 这可确保设为“内容”的文件存储在 NuGet 包的 content 文件夹中。 NuGet 包中的此文件夹由 dotnet 模板系统解析。

使所有代码文件不被模板项目编译的一个简单的方法是使用 `<ItemGroup>` 元素内项目文件中的 `<Compile Remove="**\*" />` 项。

设置模板包结构的一个简单方法是将所有模板放在单独的文件夹中，然后放到位于 .csproj 文件所在目录的 templates 文件夹的每个模板文件夹中 。 这样，你可以使用单个项目项包括 templates 中的所有文件和文件夹作为“内容”。 在 `<ItemGroup>` 元素中创建 `<Content Include="templates\**\*" Exclude="templates\**\bin\**;templates\**\obj\**" />` 项。

下面是一个遵循上述所有准则的示例 .csproj 文件。 它将 templates 子文件夹打包为“内容”包文件夹，并使所有代码文件都不被编译。

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <PackageType>Template</PackageType>
    <PackageVersion>1.0</PackageVersion>
    <PackageId>AdatumCorporation.Utility.Templates</PackageId>
    <Title>AdatumCorporation Templates</Title>
    <Authors>Me</Authors>
    <Description>Templates to use when creating an application for Adatum Corporation.</Description>
    <PackageTags>dotnet-new;templates;contoso</PackageTags>
    <TargetFramework>netstandard2.0</TargetFramework>

    <IncludeContentInPack>true</IncludeContentInPack>
    <IncludeBuildOutput>false</IncludeBuildOutput>
    <ContentTargetFolders>content</ContentTargetFolders>
  </PropertyGroup>

  <ItemGroup>
    <Content Include="templates\**\*" Exclude="templates\**\bin\**;templates\**\obj\**" />
    <Compile Remove="**\*" />
  </ItemGroup>

</Project>
```

以下示例演示使用 .csproj 创建模板包的文件和文件夹结构。 MyDotnetTemplates.csproj 文件和 templates文件夹都位于名为 project_folder 的根目录中。 templates 文件夹包含两个模板 mytemplate1 和 mytemplate2。 每个模板具有内容文件和包含 template.json 配置文件的 .template.config 文件夹。

```text
project_folder
│   MyDotnetTemplates.csproj
│
└───templates
    ├───mytemplate1
    │   │   console.cs
    │   │   readme.txt
    │   │
    │   └───.template.config
    │           template.json
    │
    └───mytemplate2
        │   otherfile.cs
        │
        └───.template.config
                template.json
```

## <a name="installing-a-template"></a>安装模板

使用 [dotnet new -i|--install](dotnet-new.md) 命令以安装包。

### <a name="to-install-a-template-from-a-nuget-package-stored-at-nugetorg"></a>从 nuget.org 中存储的 NuGet 包安装模板的具体步骤

使用 NuGet 包标识符安装模板包。

```dotnetcli
dotnet new -i <NUGET_PACKAGE_ID>
```

### <a name="to-install-a-template-from-a-local-nupkg-file"></a>从本地 nupkg 文件安装模板的具体步骤

提供指向 .nupkg NuGet 包文件的路径。

```dotnetcli
dotnet new -i <PATH_TO_NUPKG_FILE>
```

### <a name="to-install-a-template-from-a-file-system-directory"></a>从文件系统目录安装模板的具体步骤

模板可从模板文件夹安装，如以上示例中的 mytemplate1 文件夹。 指定 .template.config 文件夹的文件夹路径。 模板目录的路径不需要是绝对路径。 但是，卸载从文件夹安装的模板时需要绝对路径。

```dotnetcli
dotnet new -i <FILE_SYSTEM_DIRECTORY>
```

## <a name="get-a-list-of-installed-templates"></a>获取已安装模板的列表

在没有任何其他参数的情况下，卸载命令将列出所有已安装的模板。

```dotnetcli
dotnet new -u
```

该命令返回如下所示的输出：

```console
Template Instantiation Commands for .NET CLI

Currently installed items:
  Microsoft.DotNet.Common.ItemTemplates
    Templates:
      global.json file (globaljson)
      NuGet Config (nugetconfig)
      Solution File (sln)
      Dotnet local tool manifest file (tool-manifest)
      Web Config (webconfig)
  Microsoft.DotNet.Common.ProjectTemplates.3.0
    Templates:
      Class library (classlib) C#
      Class library (classlib) F#
      Class library (classlib) VB
      Console Application (console) C#
      Console Application (console) F#
      Console Application (console) VB
...
```

`Currently installed items:` 后面的第一级项是用于卸载模板的标识符。 在上述示例中，列出了 `Microsoft.DotNet.Common.ItemTemplates` 和 `Microsoft.DotNet.Common.ProjectTemplates.3.0`。 如果使用文件系统路径安装模板，此标识符将是 .template.config 文件夹的文件夹路径。

## <a name="uninstalling-a-template"></a>卸载模板

使用 [dotnet new -u|--uninstall](dotnet-new.md) 命令卸载包。

如果通过 NuGet 源或直接通过 .nupkg 文件安装包，请提供标识符。

```dotnetcli
dotnet new -u <NUGET_PACKAGE_ID>
```

如果通过指定 .template.config 文件夹的路径安装包，请使用该绝对路径卸载包。 你可以在 `dotnet new -u` 命令提供的输出中看到模板的绝对路径。 有关详细信息，请参阅上文的[获取已安装的模板列表](#get-a-list-of-installed-templates)部分。

```dotnetcli
dotnet new -u <ABSOLUTE_FILE_SYSTEM_DIRECTORY>
```

## <a name="create-a-project-using-a-custom-template"></a>使用自定义模板创建项目

安装模板后，通过执行 `dotnet new <TEMPLATE>` 命令来使用模板，就像使用其他任何预安装模板一样。 还可以为 `dotnet new` 命令指定[选项](dotnet-new.md#options)，包括在模板设置中配置的模板专用选项。 直接向命令提供模板的短名称：

```dotnetcli
dotnet new <TEMPLATE>
```

## <a name="see-also"></a>请参阅

- [创建 dotnet new 自定义模板（教程）](../tutorials/cli-templates-create-item-template.md)
- [dotnet/templating GitHub 存储库 Wiki](https://github.com/dotnet/templating/wiki)
- [dotnet/dotnet-template-samples GitHub 存储库](https://github.com/dotnet/dotnet-template-samples)
- [如何创建自己的 dotnet new 模板](https://devblogs.microsoft.com/dotnet/how-to-create-your-own-templates-for-dotnet-new/)
- [JSON 架构存储中的 template.json 架构](http://json.schemastore.org/template)
