---
title: 单文件应用程序
description: 了解单文件应用程序的本质以及应考虑使用此应用程序部署模型的原因。
author: lakshanf
ms.author: lakshanf
ms.date: 08/28/2020
ms.openlocfilehash: 16e9586cfc29072fa2ca70dc482272a5a0e7306a
ms.sourcegitcommit: 39b1d5f2978be15409c189a66ab30781d9082cd8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/14/2020
ms.locfileid: "92050411"
---
# <a name="single-file-deployment-and-executable"></a>单文件部署和可执行文件

通过将所有依赖应用程序的文件捆绑到一个二进制文件中，为应用程序开发人员提供一个具有吸引力的选项，那就是将应用程序作为单个文件进行部署和分发。 此部署模型从 .NET Core 3.0 开始提供，在 .NET 5.0 中进行了增强。 之前在 .NET Core 3.0 中，当用户运行单文件应用时，.NET Core 主机先将所有文件提取到一个临时目录，然后再运行该应用程序。 .NET 5.0 改进了这一体验，它可直接运行代码，无需从应用中提取文件。

单文件部署可用于[依赖框架的部署模型](index.md#publish-framework-dependent)和[独立应用程序](index.md#publish-self-contained)。 独立应用程序中单个文件的大小很大，因为它包含运行时和框架库。 单文件部署选项可与 [ReadyToRun](ready-to-run.md) 和 [Trim（.NET 5.0 中的一项试验功能）](trim-self-contained.md)发布选项结合使用。

## <a name="api-incompatibility"></a>API 不兼容

某些 API 与单文件部署不兼容，如果应用程序使用这些 API，可能需要进行修改。 如果使用第三方框架或包，则它们也可能使用了这样的 API 并需要修改。 出现问题的最常见原因是依赖于应用程序附带的文件或 DLL 的文件路径。

下表提供了用于单文件的相关运行时库 API 详细信息。

| API                            | 注意                                                                   |
|--------------------------------|------------------------------------------------------------------------|
| `Assembly.Location`            | 返回空字符串。                                               |
| `Module.FullyQualifiedName`    | 返回值为 `<Unknown>` 的字符串，或引发异常。 |
| `Module.Name`                  | 返回值为 `<Unknown>` 的字符串。                        |
| `Assembly.GetFile`             | 引发 <xref:System.IO.IOException>。                                   |
| `Assembly.GetFiles`            | 引发 <xref:System.IO.IOException>。                                   |
| `Assembly.CodeBase`            | 引发 <xref:System.PlatformNotSupportedException>。                    |
| `Assembly.EscapedCodeBase`     | 引发 <xref:System.PlatformNotSupportedException>。                    |
| `AssemblyName.CodeBase`        | 返回 `null`。                                                        |
| `AssemblyName.EscapedCodeBase` | 返回 `null`。                                                        |

我们提供了关于修复常见问题的一些建议：

* 若要访问可执行文件旁边的文件，请使用 <xref:System.AppContext.BaseDirectory?displayProperty=nameWithType>。

* 若要查找可执行文件的文件名，请使用 <xref:System.Environment.GetCommandLineArgs?displayProperty=nameWithType> 的第一个元素。

* 若要避免完全发送松散文件，请考虑使用[嵌入的资源](../../framework/resources/creating-resource-files-for-desktop-apps.md)。

## <a name="attaching-a-debugger"></a>附加调试器

在 Linux 上，可以附加到自包含单文件进程或调试故障转储的唯一调试器是[有 LLDB 的 SOS](../diagnostics/dotnet-sos.md)。

在 Windows 和 Mac 上，可以使用 Visual Studio 和 VS Code 调试故障转储。 要附加到运行自包含单文件可执行文件，需要额外的文件：_mscordbi.{dll,so}_

如果没有此文件，Visual Studio 可能会生成错误“无法附加到进程。 未安装调试组件。” 而 VS Code 可能生成错误“未能附加到进程:未知错误:0x80131c3c。”

若要修复这些错误，需要将 mscordbi 复制到可执行文件旁边。 mscordbi 默认 `publish` 到具有应用程序运行时 ID 的子目录中。 例如，如果使用适用于 Windows 的 `dotnet` CLI 并使用 `-r win-x64` 参数发布自包含单文件可执行文件，则可执行文件将置于 bin/Debug/net5.0/win-x64/publish 中。 mscordbi.dll 的副本将存在于 bin/Debug/net5.0/win-x64 中。 

## <a name="other-considerations"></a>其他注意事项

默认情况下，单文件不捆绑本机库。 在 Linux 上，我们将运行时预链接到捆绑包中，并且仅将应用程序本机库部署到单文件应用所在的目录中。 在 Windows 上，我们仅预链接托管代码，而且运行时库和应用程序本机库都部署到单文件应用所在的目录中。 目的是确保提供良好的调试体验，这要求将本机文件从单文件中排除。 可选择设置标志 `IncludeNativeLibrariesForSelfExtract`，从而在单文件捆绑包中包含本机库，但在运行单文件应用程序时，这些文件将被提取到客户端计算机上的临时目录中。

单文件应用程序旁边将具有所有相关的 PDB 文件，并且默认情况下不会绑定。 如果要在生成的项目的程序集内包含 PDB，请将 `DebugType` 设置为 `embedded`，如[下方](#include-pdb-files-inside-the-bundle)所详述。

托管的 C++ 应组件不太适合进行单文件部署，建议使用 C# 或其他非托管 C++ 语言编写应用程序来实现单文件兼容。

## <a name="exclude-files-from-being-embedded"></a>将文件排除在嵌入之外

通过设置以下元数据，可明确指定不在单文件中嵌入某些文件：

```xml
<ExcludeFromSingleFile>true</ExcludeFromSingleFile>
```

例如，若要将某些文件放置在发布目录中，但不将它们捆绑到单文件中：

```xml
<ItemGroup>
  <Content Update="Plugin.dll">
    <CopyToPublishDirectory>PreserveNewest</CopyToPublishDirectory>
    <ExcludeFromSingleFile>true</ExcludeFromSingleFile>
  </Content>
</ItemGroup>
```

## <a name="include-pdb-files-inside-the-bundle"></a>在绑定内包括 PDB 文件

可以使用下面的设置将程序集的 PDB 文件嵌入到程序集本身 (`.dll`)。 由于符号是程序集的一部分，因此，它们也会是单文件应用程序的一部分：

```xml
<DebugType>embedded</DebugType>
```

例如，将以下属性添加到程序集的项目文件，以将 PDB 文件嵌入到该程序集：

```xml
<PropertyGroup>
  <DebugType>embedded</DebugType>
</PropertyGroup>
```

## <a name="publish-a-single-file-app---cli"></a>发布单文件应用 - CLI

使用 [dotnet publish](../tools/dotnet-publish.md) 命令发布单文件应用程序。 发布应用时，请设置以下属性：

- 针对特定运行时进行发布：`-r win-x64`
- 作为单个文件进行发布：`-p:PublishSingleFile=true`

以下示例将 Windows 应用作为独立的单文件应用程序发布。

```dotnetcli
dotnet publish -r win-x64 -p:PublishSingleFile=true --self-contained true
```

以下示例将 Linux 应用作为依赖框架的单文件应用程序发布。

```dotnetcli
dotnet publish -r linux-x64 -p:PublishSingleFile=true --self-contained false
```

有关详细信息，请参阅[使用 .NET Core CLI 发布 .NET Core 应用](deploy-with-cli.md)。

## <a name="publish-a-single-file-app---visual-studio"></a>发布单文件应用 - Visual Studio

Visual Studio 创建可重用的发布配置文件，用于控制应用程序的发布方式。

01. 在“解决方案资源管理器”窗格中，右键单击要发布的项目  。 选择“发布”。

    :::image type="content" source="media/single-file/visual-studio-solution-explorer.png" alt-text="解决方案资源管理器，其中右键单击菜单突出显示了“发布”选项。":::

    如果还没有发布配置文件，请按照说明创建一个并选择“文件夹”目标类型  。

01. 选择“编辑”  。

    :::image type="content" source="media/single-file/visual-studio-publish-edit-settings.png" alt-text="解决方案资源管理器，其中右键单击菜单突出显示了“发布”选项。":::

01. 在“配置文件设置”对话框中，设置以下选项  ：

    - 将“部署模式”设置为“自包含”   。
    - 将“目标运行时”设置为要发布到的平台  。
    - 选择“生成单个文件”。

    选择“保存”保存设置并返回到“发布”对话框   。

    :::image type="content" source="media/single-file/visual-studio-publish-single-file-properties.png" alt-text="解决方案资源管理器，其中右键单击菜单突出显示了“发布”选项。":::

01. 选择“发布”，将应用作为单个文件发布。

有关详细信息，请参阅[使用 Visual Studio 发布 .NET Core 应用](deploy-with-vs.md)。

## <a name="publish-a-single-file-app---visual-studio-for-mac"></a>发布单文件应用 - Visual Studio for Mac

Visual Studio for Mac 不提供将应用作为单个文件发布的选项。 你需要按照[发布单文件应用 - CLI](#publish-a-single-file-app---cli) 部分中的说明手动发布。 有关详细信息，请参阅[使用 .NET Core CLI 发布 .NET Core 应用](deploy-with-cli.md)。

## <a name="see-also"></a>请参阅

- [.NET Core 应用程序部署](index.md)。
- [使用 .NET Core CLI 发布 .NET Core 应用](deploy-with-cli.md)。
- [使用 Visual Studio 发布 .NET Core 应用](deploy-with-vs.md)。
- [`dotnet publish` 命令](../tools/dotnet-publish.md)。
