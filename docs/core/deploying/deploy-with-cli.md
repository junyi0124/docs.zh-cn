---
title: 使用 .NET Core CLI 发布应用
description: 了解如何使用 .NET Core CLI 命令发布 .NET Core 应用。
author: adegeo
ms.author: adegeo
ms.date: 12/12/2019
dev_langs:
- csharp
- vb
ms.openlocfilehash: a592b397d1ffee5b224638a8d17ce6fa9e44eea2
ms.sourcegitcommit: dc2feef0794cf41dbac1451a13b8183258566c0e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/24/2020
ms.locfileid: "85325030"
---
# <a name="publish-net-core-apps-with-the-net-core-cli"></a>使用 .NET Core CLI 发布 .NET Core 应用

本文演示了如何使用命令行发布 .NET Core 应用程序。 .NET Core 提供了三种发布应用程序的方式。 依赖于框架的部署生成一个跨平台 .dll 文件，该文件使用本地安装的 .NET Core 运行时。 依赖于框架的可执行文件生成特定于平台的可执行文件，后者使用本地安装的 .NET Core 运行时。 独立可执行文件生成特定于平台的可执行文件，并包含 .NET Core 运行时的本地副本。

请参阅 [.NET Core 应用程序部署](index.md)了解有关这些发布模式的概述。

正在查找有关 CLI 的快速帮助？ 下表列出了一些关于如何发布应用的示例。 可以使用 `-f <TFM>` 参数或通过编辑项目文件来指定目标框架。 有关详细信息，请参阅[发布基本知识](#publishing-basics)。

| 发布模式 | SDK 版本 | 命令 |
| ------------ | ----------- | ------- |
| 依赖框架的部署 | 2.x | `dotnet publish -c Release` |
| 依赖于框架的可执行文件 | 2.2 | `dotnet publish -c Release -r <RID> --self-contained false` |
|                                | 3.0 | `dotnet publish -c Release -r <RID> --self-contained false` |
|                                | 3.0* | `dotnet publish -c Release` |
| 独立部署      | 2.1 | `dotnet publish -c Release -r <RID> --self-contained true` |
|                                | 2.2 | `dotnet publish -c Release -r <RID> --self-contained true` |
|                                | 3.0 | `dotnet publish -c Release -r <RID> --self-contained true` |

\* 使用 SDK 版本 3.0 时，框架依赖可执行文件是运行基本 `dotnet publish` 命令时的默认发布模式。 仅当项目定目标到 .NET Core 2.1  或 .NET Core 3.0  时，这才适用。

## <a name="publishing-basics"></a>发布基本知识

发布应用时，项目文件的 `<TargetFramework>` 设置指定默认目标框架。 可以将目标框架更改为任意有效[目标框架名字对象 (TFM)](../../standard/frameworks.md)。 例如，如果项目使用 `<TargetFramework>netcoreapp2.2</TargetFramework>`，则会创建面向 .NET Core 2.2 的二进制文件。 此设置中指定的 TFM 是[`dotnet publish`](../tools/dotnet-publish.md) 命令使用的默认目标。

如果要以多个框架为目标，可以将 `<TargetFrameworks>` 设置设置为多个以分号分隔的 TFM 值。 可以使用 `dotnet publish -f <TFM>` 命令发布其中一个框架。 例如，如果有 `<TargetFrameworks>netcoreapp2.1;netcoreapp2.2</TargetFrameworks>` 并运行 `dotnet publish -f netcoreapp2.1`，则会创建面向 .NET Core 2.1 的二进制文件。

除非另有设置，否则 [`dotnet publish`](../tools/dotnet-publish.md) 命令的输出目录为 `./bin/<BUILD-CONFIGURATION>/<TFM>/publish/`。 除非使用 `-c` 参数进行更改，否则默认的 BUILD-CONFIGURATION 模式为 Debug。 例如，`dotnet publish -c Release -f netcoreapp2.1` 发布到 `myfolder/bin/Release/netcoreapp2.1/publish/`。

如果使用 .NET Core SDK 3.0 或更高版本，则面向 .NET Core 版本 2.1、2.2、3.0 或更高版本的应用的默认发布模式为依赖于框架的可执行文件。

如果使用 .NET Core SDK 2.1，则面向 .NET Core 版本 2.1 和 2.2 的应用的默认发布模式为依赖于框架的部署。

### <a name="native-dependencies"></a>本机依赖项

如果应用具有本机依赖项，则只能在相同操作系统上运行。 例如，如果应用使用本机 Windows API，则不能在 macOS 或 Linux 上运行。 需要提供特定于平台的代码并为每个平台编译可执行文件。

另外，如果引用的库具有本机依赖项，则应用可能无法在每个平台上运行。 但是，引用的 NuGet 包可能包含特定于平台的版本，以便处理所需的本机依赖项。

使用本机依赖项分发应用时，可能需要使用 `dotnet publish -r <RID>` 开关来指定想要发布的目标平台。 有关运行时标识符的详细信息，请参阅[运行时标识符 (RID) 目录](../rid-catalog.md)。

有关特定于平台的二进制文件的详细信息，请参阅[依赖于框架的可执行文件](#framework-dependent-executable)和[独立部署](#self-contained-deployment)部分。

## <a name="sample-app"></a>示例应用

可使用以下应用来探索发布命令。 通过在终端中运行以下命令来创建应用：

```dotnetcli
mkdir apptest1
cd apptest1
dotnet new console
dotnet add package Figgle
```

控制台模板生成的 `Program.cs` 或 `Program.vb` 文件需要进行以下更改：

```csharp
using System;

namespace apptest1
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine(Figgle.FiggleFonts.Standard.Render("Hello, World!"));
        }
    }
}
```

```vb
Module Program
    Sub Main(args As String())
        Console.WriteLine(Figgle.FiggleFonts.Standard.Render("Hello, World!"))
    End Sub
End Module
```

运行应用 ([`dotnet run`](../tools/dotnet-run.md)) 时，将显示以下输出：

```terminal
  _   _      _ _         __        __         _     _ _
 | | | | ___| | | ___    \ \      / /__  _ __| | __| | |
 | |_| |/ _ \ | |/ _ \    \ \ /\ / / _ \| '__| |/ _` | |
 |  _  |  __/ | | (_) |    \ V  V / (_) | |  | | (_| |_|
 |_| |_|\___|_|_|\___( )    \_/\_/ \___/|_|  |_|\__,_(_)
                     |/
```

## <a name="framework-dependent-deployment"></a>依赖框架的部署

对于 .NET Core SDK 2.x CLI，依赖于框架的部署 (FDD) 是基本 `dotnet publish` 命令的默认模式。

将应用作为 FDD 发布时，会在 `./bin/<BUILD-CONFIGURATION>/<TFM>/publish/` 文件夹中创建 `<PROJECT-NAME>.dll` 文件。 若要运行应用，请导航到输出文件夹并使用 `dotnet <PROJECT-NAME>.dll` 命令。

应用配置为面向 .NET Core 的特定版本。 目标 .NET Core 运行时需要位于运行应用程序的任何计算机上。 例如，如果应用面向 .NET Core 2.2，则运行应用的任何计算机都必须已安装 .NET Core 2.2 运行时。 如[发布基础知识](#publishing-basics)部分中所述，可以编辑项目文件为更改默认目标框架或面向多个框架。

发布 FDD 会创建一个应用，该应用会自动前滚到运行该应用的系统上可用的最新 .NET Core 安全修补程序。 有关编译时的版本绑定的详细信息，请参阅[选择要使用的 .NET Core 版本](../versions/selection.md#framework-dependent-apps-roll-forward)。

## <a name="framework-dependent-executable"></a>依赖于框架的可执行文件

对于 .NET Core SDK 3.x CLI，依赖于框架的可执行文件 (FDE) 是基本 `dotnet publish` 命令的默认模式。 只要想要面向当前操作系统，就不需要指定任何其他参数。

在此模式下，将创建特定于平台的可执行主机来托管跨平台应用。 此模式类似于 FDD，因为 FDD 需要 `dotnet` 命令形式的主机。 每个平台的主机可执行文件名各不相同，其文件名类似于 `<PROJECT-FILE>.exe`。 可以直接运行此可执行文件，而不是调用 `dotnet <PROJECT-FILE>.dll`，这仍然是运行应用的可接受方式。

应用配置为面向 .NET Core 的特定版本。 目标 .NET Core 运行时需要位于运行应用程序的任何计算机上。 例如，如果应用面向 .NET Core 2.2，则运行应用的任何计算机都必须已安装 .NET Core 2.2 运行时。 如[发布基础知识](#publishing-basics)部分中所述，可以编辑项目文件为更改默认目标框架或面向多个框架。

发布 FDE 会创建一个应用，该应用会自动前滚到运行该应用的系统上可用的最新 .NET Core 安全修补程序。 有关编译时的版本绑定的详细信息，请参阅[选择要使用的 .NET Core 版本](../versions/selection.md#framework-dependent-apps-roll-forward)。

对于 .NET Core 2.2 及早期版本，必须使用以下开关通过 `dotnet publish` 命令来发布 FDE：

- `-r <RID>` 此开关使用标识符 (RID) 来指定目标平台。 有关运行时标识符的详细信息，请参阅[运行时标识符 (RID) 目录](../rid-catalog.md)。

- `--self-contained false` 此开关告知 .NET Core SDK 创建可执行文件作为 FDE。

每次使用 `-r` 开关时，输出文件路都将更改为：`./bin/<BUILD-CONFIGURATION>/<TFM>/<RID>/publish/`

如果使用[示例应用](#sample-app)，请运行 `dotnet publish -f netcoreapp2.2 -r win10-x64 --self-contained false`。 此命令将创建以下可执行文件：`./bin/Debug/netcoreapp2.2/win10-x64/publish/apptest1.exe`

> [!NOTE]
> 可以通过启用全局固定模式来降低部署的总大小  。 此模式适用于不具有全局意识且可以使用[固定区域性](xref:System.Globalization.CultureInfo.InvariantCulture)的格式约定、大小写约定以及字符串比较和排序顺序的应用程序。 有关全局固定模式及其启用方式的详细信息，请参阅 [.NET Core 全局固定模式](https://github.com/dotnet/runtime/blob/master/docs/design/features/globalization-invariant-mode.md)。

## <a name="self-contained-deployment"></a>独立部署

发布独立部署 (SCD) 时，.NET Core SDK 创建特定于平台的可执行文件。 发布 SCD 时会包括运行应用所需的所有 .NET Core 文件，但不包含 [.NET Core 的本机依赖项](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md)。 这些依赖项必须在应用运行前存在于系统中。

发布 SCD 会创建一个不会前滚到最新可用 .NET Core 安全修补程序的应用。 有关编译时的版本绑定的详细信息，请参阅[选择要使用的 .NET Core 版本](../versions/selection.md#self-contained-deployments-include-the-selected-runtime)。

必须通过 `dotnet publish` 命令使用以下开关来发布 SCD：

- `-r <RID>` 此开关使用标识符 (RID) 来指定目标平台。 有关运行时标识符的详细信息，请参阅[运行时标识符 (RID) 目录](../rid-catalog.md)。

- `--self-contained true` 此开关告知 .NET Core SDK 创建可执行文件作为 SCD。

> [!NOTE]
> 可以通过启用全局固定模式来降低部署的总大小  。 此模式适用于不具有全局意识且可以使用[固定区域性](xref:System.Globalization.CultureInfo.InvariantCulture)的格式约定、大小写约定以及字符串比较和排序顺序的应用程序。 有关全局固定模式及其启用方式的详细信息，请参阅 [.NET Core 全局固定模式](https://github.com/dotnet/runtime/blob/master/docs/design/features/globalization-invariant-mode.md)。

## <a name="see-also"></a>请参阅

- [.NET Core 应用程序部署概述](index.md)
- [.NET Core 运行时标识符 (RID) 目录](../rid-catalog.md)
