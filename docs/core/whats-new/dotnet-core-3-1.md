---
title: .NET Core 3.1 的新增功能
description: 了解 .NET Core 3.1 的新增功能。
dev_langs:
- csharp
author: adegeo
ms.author: adegeo
ms.date: 12/04/2019
ms.openlocfilehash: 9caecdcc3516db2bd71420184fbd21f7837552a7
ms.sourcegitcommit: 721c3e4bdbb1ea0bb420818ec944c538fe5c513a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/01/2020
ms.locfileid: "96437933"
---
# <a name="whats-new-in-net-core-31"></a>.NET Core 3.1 的新增功能

本文介绍了 .NET Core 3.1 中的新增功能。 此版本包含对 .NET Core 3.0 的细微改进，重点介绍小型但重要的修复。 .NET Core 3.1 中最重要的特性为，它是[长期支持 (LTS)](#long-term-support) 版本。

如果使用的是 Visual Studio 2019，则必须更新到 [Visual Studio 2019 版本 16.4 或更高版本](https://visualstudio.microsoft.com/downloads/)才能使用 .NET Core 3.1 项目。 有关 Visual Studio 版本 16.4 中新增功能的详细信息，请参阅 [Visual Studio 2019 版本 16.4 中的新增功能](/visualstudio/releases/2019/release-notes-v16.4#whats-new-in-visual-studio-2019-version-164)。

Visual Studio for Mac 也支持 .NET Core 3.1，并且 Visual Studio for Mac 8.4 中就包括 .NET Core 3.1。

有关版本的详细信息，请参阅 [.NET Core 3.1 公告](https://devblogs.microsoft.com/dotnet/announcing-net-core-3-1/)。

- 在 Windows、macOS 或 Linux 上[下载并开始使用 .NET Core 3.1](https://dotnet.microsoft.com/download/dotnet-core/3.1)。

## <a name="long-term-support"></a>长期支持

.NET Core 3.1 是未来三年包含来自 Microsoft 的支持的 LTS 版本。 强烈建议将应用移到 .NET Core 3.1。 其他主要版本的当前生命周期如下所示：

| Release | 说明 |
| ------- | ---- |
| .NET Core 3.0 | 生命周期终结于 2020 年 3 月 3 日。     |
| .NET Core 2.2 | 生命周期终结于 2019 年 12 月 23 日。 |
| .NET Core 2.1 | 生命周期终结于 2021 年 8 月 21 日。    |

有关详细信息，请参阅 [.NET Core 支持策略](https://dotnet.microsoft.com/platform/support/policy/dotnet-core)。

## <a name="macos-apphost-and-notarization"></a>macOS appHost 和公证

仅 macOS 

从已公证的适用于 macOS 的 .NET Core SDK 3.1 开始，默认已禁用 appHost 设置。 有关详细信息，请参阅 [macOS Catalina 公证以及对 .NET Core 下载和项目的影响](../install/macos-notarization-issues.md)。

启用 appHost 设置后，.NET Core 在生成或发布时将生成本机 Mach-O 可执行文件。 如果使用 `dotnet run` 命令从源代码中运行应用，或通过启动 Mach-O 可执行文件直接运行应用，则应用会在 appHost 的上下文中运行。

如果没有 appHost，用户就只能使用 `dotnet <filename.dll>` 命令启动[依赖框架](../deploying/index.md#publish-framework-dependent)的应用。 发布[独立](../deploying/index.md#publish-self-contained)应用时，始终会创建 appHost。

可以在项目级别配置 appHost，或通过 `-p:UseAppHost` 参数切换特定 `dotnet` 命令的 appHost：

- 项目文件

  ```xml
  <PropertyGroup>
    <UseAppHost>true</UseAppHost>
  </PropertyGroup>
  ```

- 命令行参数

  ```dotnetcli
  dotnet run -p:UseAppHost=true
  ```

有关 `UseAppHost` 设置的详细信息，请参阅 [Microsoft.NET.Sdk 的 MSBuild 属性](../project-sdk/msbuild-props.md#useapphost)。

## <a name="windows-forms"></a>Windows 窗体

*仅限 Windows*

> [!WARNING]
> Windows 窗体中发生重大变更。

旧版控件包含在 Windows 窗体中，这些窗体在一段时间内无法在 Visual Studio 设计器工具箱中使用。 它们已替换为 .NET Framework 2.0 中的新控件。 它们已从适用于 .NET Core 3.1 的桌面 SDK 中删除。

| 已删除的控件 | 推荐的替换控件 | 已删除关联的 API |
| --------------- | ----------------------- | ----------------------- |
| DataGrid        | <xref:System.Windows.Forms.DataGridView>      | DataGridCell<br/>DataGridRow<br/>DataGridTableCollection<br/>DataGridColumnCollection<br/>DataGridTableStyle<br/>DataGridColumnStyle<br/>DataGridLineStyle<br/>DataGridParentRowsLabel<br/>DataGridParentRowsLabelStyle<br/>DataGridBoolColumn<br/>DataGridTextBox<br/>GridColumnStylesCollection<br/>GridTableStylesCollection<br/>HitTestType |
| ToolBar         | <xref:System.Windows.Forms.ToolStrip>         | ToolBarAppearance |
| ToolBarButton   | <xref:System.Windows.Forms.ToolStripButton>   | ToolBarButtonClickEventArgs<br/>ToolBarButtonClickEventHandler<br/>ToolBarButtonStyle<br/>ToolBarTextAlign |
| ContextMenu     | <xref:System.Windows.Forms.ContextMenuStrip>  |  |
| :::no-loc text="Menu"::: | <xref:System.Windows.Forms.ToolStripDropDown><br/><xref:System.Windows.Forms.ToolStripDropDownMenu> | MenuItemCollection |
| MainMenu        | <xref:System.Windows.Forms.MenuStrip>         |  |
| MenuItem        | <xref:System.Windows.Forms.ToolStripMenuItem> |  |

我们建议你将应用程序更新到 .NET Core 3.1 并移动到替换控件。 替换控件是一个简单的过程，本质上属于“查找和替换”类型。

## <a name="ccli"></a>C++/CLI

*仅限 Windows*

已添加对创建 C++/CLI（也称为“托管 C++”）项目的支持。 从这些项目生成的二进制文件与 .NET Core 3.0 及更高版本兼容。

若要添加对 Visual Studio 2019 版本 16.4 中的 C++/CLI 的支持，请安装[“使用 C++ 的桌面开发”工作负荷](/cpp/build/vscpp-step-0-installation?view=vs-2019#step-4---choose-workloads)。 此工作负载将两个模板添加到 Visual Studio：

- CLR 类库(.NET Core)
- CLR 空项目(.NET Core)

## <a name="next-steps"></a>后续步骤

- [查看 .NET Core 3.0 和 3.1 之间的重大变更。](../compatibility/3.1.md)
- [查看用于 Windows 窗体应用的 .NET Core 3.1 中的中断性变更。](../compatibility/winforms.md#net-core-31)
