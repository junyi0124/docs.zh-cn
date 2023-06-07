---
title: 安装和管理 SDK 模板 - .NET Core
description: 了解如何在 Windows、Linux 和 macOS 上安装 .NET Core 模板。
author: adegeo
ms.author: adegeo
ms.date: 04/24/2020
zone_pivot_groups: operating-systems-set-one
no-loc:
- dotnet new
- dotnet nuget add source
ms.openlocfilehash: 09acae1409eb0492be10bd3a61b14da5be57c6c7
ms.sourcegitcommit: dc2feef0794cf41dbac1451a13b8183258566c0e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/24/2020
ms.locfileid: "85324496"
---
# <a name="manage-net-project-and-item-templates"></a>管理 .NET 项目和项模板

.NET Core 提供了一个模板系统，允许用户从 NuGet、NuGet 包文件或文件系统目录中安装或卸载模板。 本文介绍如何通过 .NET Core SDK CLI 管理 .NET Core 模板。

若要详细了解如何创建模板，请参阅[教程：创建模板](../tutorials/cli-templates-create-item-template.md)。

## <a name="install-template"></a>安装模板

使用 `-i` 参数通过 [dotnet new](../tools/dotnet-new.md) SDK 命令安装模板。 可以提供模板的 NuGet 包标识符或包含模板文件的文件夹。

### <a name="nuget-hosted-package"></a>NuGet 托管包

.NET CLI 模板上传到 [NuGet](https://www.nuget.org/) 以便进行广泛分发。 还可以从专用源安装模板。 如[本地 NuGet 包](#local-nuget-package)部分所述，可以分发和手动安装 nupkg 模板文件，而不是将模板上传到 NuGet 源。

有关配置 NuGet 源的详细信息，请参阅 [dotnet nuget add source](../tools/dotnet-nuget-add-source.md)。

若要从默认 NuGet 源安装模板包，请使用 `dotnet new -i {package-id}` 命令：

```dotnetcli
dotnet new -i Microsoft.DotNet.Web.Spa.ProjectTemplates
```

若要从默认 NuGet 源安装特定版本的模板包，请使用 `dotnet new -i {package-id}::{version}` 命令：

```dotnetcli
dotnet new -i Microsoft.DotNet.Web.Spa.ProjectTemplates::2.2.6
```

### <a name="local-nuget-package"></a>本地 NuGet 包

创建模板包后，将生成一个 nupkg 文件。 如果有包含模板的 nupkg 文件，则可以使用 `dotnet new -i {path-to-package}` 命令进行安装：

::: zone pivot="os-windows"

```dotnetcli
dotnet new -i c:\code\nuget-packages\Some.Templates.1.0.0.nupkg
```

::: zone-end

::: zone pivot="os-linux,os-macos"

```dotnetcli
dotnet new -i ~/code/nuget-packages/Some.Templates.1.0.0.nupkg
```

::: zone-end

### <a name="folder"></a>文件夹

从 nupkg 文件安装模板的一个替代方法是使用 `dotnet new -i {folder-path}` 命令直接从文件夹安装模板。 指定的文件夹将被视为找到的任何模板的模板包标识符。 安装指定文件夹的层次结构中找到的任何模板。

::: zone pivot="os-windows"

```dotnetcli
dotnet new -i c:\code\nuget-packages\some-folder\
```

::: zone-end

::: zone pivot="os-linux,os-macos"

```dotnetcli
dotnet new -i ~/code/nuget-packages/some-folder/
```

::: zone-end

在命令中指定的 `{folder-path}` 将成为找到的所有模板的模板包标识符。 如[模板列表](#list-templates)部分中指定的一样，可以使用 `dotnet new -u` 命令获取已安装的模板列表。 在此示例中，模板包标识符显示为用于安装的文件夹：

::: zone pivot="os-windows"

```console
dotnet new -u
Template Instantiation Commands for .NET Core CLI

Currently installed items:

... cut to save space ...

  c:\code\nuget-packages\some-folder
    Templates:
      A Template Console Class (templateconsole) C#
      Project for some technology (contosoproject) C#
    Uninstall Command:
      dotnet new -u c:\code\nuget-packages\some-folder
```

::: zone-end

::: zone pivot="os-linux,os-macos"

```console
dotnet new -u
Template Instantiation Commands for .NET Core CLI

Currently installed items:

... cut to save space ...

  /home/username/code/templates
    Templates:
      A Template Console Class (templateconsole) C#
      Project for some technology (contosoproject) C#
    Uninstall Command:
      dotnet new -u /home/username/code/templates
```

::: zone-end

## <a name="uninstall-template"></a>卸载模板

使用 `-u` 参数通过 [dotnet new](../tools/dotnet-new.md) SDK 命令卸载模板。 可以提供模板的 NuGet 包标识符或包含模板文件的文件夹。

### <a name="nuget-package"></a>NuGet 程序包

安装 NuGet 模板包后，无论是从 NuGet 源还是从 nupkg 文件安装的，都可以通过引用 NuGet 包标识符将其卸载。

若要卸载模板包，请使用 `dotnet new -u {package-id}` 命令：

```dotnetcli
dotnet new -u Microsoft.DotNet.Web.Spa.ProjectTemplates
```

### <a name="folder"></a>文件夹

如果通过[文件夹路径](#folder)安装模板，文件夹路径将成为模板包标识符。

若要卸载模板包，请使用 `dotnet new -u {package-folder-path}` 命令：

::: zone pivot="os-windows"

```dotnetcli
dotnet new -u c:\code\nuget-packages\some-folder
```

::: zone-end

::: zone pivot="os-linux,os-macos"

```dotnetcli
dotnet new -u /home/username/code/templates
```

::: zone-end

## <a name="list-templates"></a>模板列表

通过使用不带包标识符的标准卸载命令，可以查看已安装模板的列表以及用于卸载每个模板的命令。

```console
dotnet new -u
Template Instantiation Commands for .NET Core CLI

Currently installed items:

... cut to save space ...

  c:\code\nuget-packages\some-folder
    Templates:
      A Template Console Class (templateconsole) C#
      Project for some technology (contosoproject) C#
    Uninstall Command:
      dotnet new -u c:\code\nuget-packages\some-folder
```

## <a name="install-templates-from-other-sdks"></a>从其他 SDK 安装模板

如果已按顺序安装了每个版本的 SDK（例如安装了 SDK 2.0、SDK 2.1 等），则已安装每个 SDK 的模板。 但是，如果从更高版本的 SDK 版本（如 3.1）开始，则将仅包括 [LTS（长期支持）版本](https://dotnet.microsoft.com/platform/support/policy/dotnet-core)的模板（在发布 SDK 3.1 版本时，为 SDK 2.1 和 SDK 3.1）。 不包含任何其他版本的模板。

NuGet 上提供了 .NET Core 模板，你可以像安装任何其他模板一样安装它们。 有关详细信息，请参阅[安装 NuGet 托管包](#nuget-hosted-package)。

| SDK              | NuGet 包标识符                                                                                                      |
|------------------|-------------------------------------------------------------------------------------------------------------------------------|
| .NET Core 2.1    | [`Microsoft.DotNet.Common.ProjectTemplates.2.1`](https://www.nuget.org/packages/Microsoft.DotNet.Common.ProjectTemplates.2.1) |
| .NET Core 2.2    | [`Microsoft.DotNet.Common.ProjectTemplates.2.2`](https://www.nuget.org/packages/Microsoft.DotNet.Common.ProjectTemplates.2.2) |
| .NET Core 3.0    | [`Microsoft.DotNet.Common.ProjectTemplates.3.0`](https://www.nuget.org/packages/Microsoft.DotNet.Common.ProjectTemplates.3.0) |
| .NET Core 3.1    | [`Microsoft.DotNet.Common.ProjectTemplates.3.1`](https://www.nuget.org/packages/Microsoft.DotNet.Common.ProjectTemplates.3.1) |
| ASP.NET Core 2.1 | [`Microsoft.DotNet.Web.ProjectTemplates.2.1`](https://www.nuget.org/packages/Microsoft.DotNet.Web.ProjectTemplates.2.1)       |
| ASP.NET Core 2.2 | [`Microsoft.DotNet.Web.ProjectTemplates.2.2`](https://www.nuget.org/packages/Microsoft.DotNet.Web.ProjectTemplates.2.2)       |
| ASP.NET Core 3.0 | [`Microsoft.DotNet.Web.ProjectTemplates.3.0`](https://www.nuget.org/packages/Microsoft.DotNet.Web.ProjectTemplates.3.0)       |
| ASP.NET Core 3.1 | [`Microsoft.DotNet.Web.ProjectTemplates.3.1`](https://www.nuget.org/packages/Microsoft.DotNet.Web.ProjectTemplates.3.1)       |

例如，.NET Core SDK 包含面向 .NET Core 2.1 和 .NET Core 3.1 的控制台应用的模板。 如果要面向 .NET Core 3.0，则需要安装 3.0 模板。

01. 尝试创建面向 .NET Core 3.0 的应用。

    ```dotnetcli
    dotnet new console --framework netcoreapp3.0
    ```

    如果看到错误消息，则需要安装模板。

    > 找不到与输入匹配的已安装模板，正在联机搜索匹配的模板…

01. 安装 .NET Core 3.0 项目模板。

    ```dotnetcli
    dotnet new -i Microsoft.DotNet.Common.ProjectTemplates.3.0
    ```

01. 再次尝试创建应用。

    ```dotnetcli
    dotnet new console --framework netcoreapp3.0
    ```

    应该会看到一条消息，指示项目已创建。

    > “控制台应用程序”模板已成功创建。
    >
    > 正在处理创建后操作…正在 path-to-project-file.csproj 上运行“dotnet restore”…正在确定要还原的项目…path-to-project-file.csproj 的还原完成时间为 1.05 秒。
    >
    > 还原成功。

## <a name="see-also"></a>请参阅

- [教程：创建项模板](../tutorials/cli-templates-create-item-template.md)
- [dotnet new](../tools/dotnet-new.md)
- [dotnet nuget add source](../tools/dotnet-nuget-add-source.md)
