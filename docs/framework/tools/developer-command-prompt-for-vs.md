---
title: Visual Studio 开发人员命令提示
description: 了解如何使用 Visual Studio 的开发人员命令提示。开发人员命令提示使你可以更轻松地使用 .NET 工具。 它将自动设置特定的环境变量。
ms.date: 01/05/2020
helpviewer_keywords:
- command prompt, Windows SDK
- Visual Studio command prompt
- command prompt, Visual Studio
- SDK command prompt
- tools [.NET Framework], setting environment variables
- environment variables, setting for tools
- developer command prompt
ms.assetid: 94fcf524-9045-4993-bfb2-e2d8bad44219
ms.openlocfilehash: 92416820f47cb778dfcc916b8626df4aa328814c
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/24/2020
ms.locfileid: "87167172"
---
# <a name="developer-command-prompt-for-visual-studio"></a>Visual Studio 开发人员命令提示

Visual Studio 的开发人员命令提示使你可以更轻松地使用 .NET Framework 工具。 它是一个自动设置特定环境变量的命令提示符。 打开开发人员命令提示后，可以为 [.NET Framework](index.md) 工具（如 `ildasm` 或 `clrver`）输入命令。

## <a name="prerequisites"></a>先决条件

- [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)

## <a name="search-for-the-command-prompt-on-your-machine"></a>在计算机中搜索命令提示符

你可能会有多个命令提示符，具体取决于你安装的 Visual Studio 的版本及其他任何 SDK 和工作负载。 如果以下步骤不起作用，可以尝试[在计算机中手动查找文件](#manually-locate-the-files-on-your-machine)或[从 Visual Studio 内部启动命令提示符](#start-the-command-prompt-from-inside-visual-studio)。

### <a name="windows-10"></a>Windows 10

1. 选择“开始”![键盘上的 Windows 徽标键。](./media/developer-command-prompt-for-vs/windows-logo-key-graphic.png) 并滚动到字母“V”。

1. 展开“Visual Studio 2019”文件夹。

1. 选择“VS 2019 开发人员命令提示”（或者你想使用的命令提示符）。

   或者，你可以首先在任务栏的搜索框中键入命令提示符的名称，然后在结果列表开始显示搜索匹配项时选择所需的结果。

   ![在 Windows 10 上显示搜索行为的动画 gif](./media/developer-command-prompt-for-vs/windows10-search.gif)

### <a name="windows-81"></a>Windows 8.1

1. 按 Windows 徽标键![键盘上的 Windows 徽标键](./media/developer-command-prompt-for-vs/windows-logo-key-graphic.png)，转到“开始”屏幕。 例如，在键盘上。

1. 在“开始”屏幕上，按 Ctrl+Tab 打开“应用程序”列表，然后按 V。然后显示一个列表，其中包含所有已安装的 Visual Studio 命令提示。

1. 选择“VS 2019 开发人员命令提示”（或者你想使用的命令提示符）。

### <a name="windows-7"></a>Windows 7

1. 选择“开始”，然后展开“所有程序”。

1. 选择“Visual Studio 2019” > “Visual Studio Tools” > “VS 2019 开发人员命令提示”，或者你想使用的命令提示。

   ![突出显示命令提示符的 Windows 7“开始”菜单](./media/developer-command-prompt-for-vs/windows7-menu.png)

如果安装了其他 SDK，例如 [Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk) 或[之前的版本](https://developer.microsoft.com/windows/downloads/sdk-archive)，则可能看到其他命令提示符。 查看单个工具的文档，以确定应使用哪个版本的命令提示。

## <a name="manually-locate-the-files-on-your-machine"></a>在计算机中手动查找文件

已安装的命令提示符的快捷方式通常放在 Visual Studio 的“开始菜单”文件夹中，例如 %ProgramData%\Microsoft\Windows\Start Menu\Programs\Visual Studio 2019\Visual Studio Tools。 但是如果出于某种原因，搜索命令提示未产生预期的效果，你可以尝试在计算机中手动查找快捷方式。 请尝试搜索命令提示符文件的名称，例如 VsDevCmd.bat，或者转到“工具”文件夹，例如 %ProgramFiles(x86)%\Microsoft Visual Studio\2019\Community\Common7\Tools（该路径根据你的 Visual Studio 版本和安装位置而变化）。

## <a name="start-the-command-prompt-from-inside-visual-studio"></a>从 Visual Studio 内部启动命令提示符

为便于访问，你可以将开发人员命令提示或其他任何命令提示符添加到 Visual Studio 中的“工具”菜单中。 要使该工具可用，请将其添加到外部工具列表中。 步骤如下：

1. 打开 Visual Studio。

1. 在“启动”窗口中，选择“继续但无需代码”。

1. 在菜单栏上，依次选择“工具” > “外部工具”。

1. 在“外部工具”对话框中，选择“添加”按钮。 随即出现一个新项。

1. 为新菜单项输入“标题”，例如 `Command Prompt`。

1. 在“命令”字段中，指定要启动的文件，例如 `%comspec%` 或 `C:\Windows\System32\cmd.exe`。

1. 在“参数”字段中，指定可在其中找到要使用的特定命令提示的位置，例如 `/k "C:\Program Files (x86)\Microsoft Visual Studio\2019\Community\Common7\Tools\VsDevCmd.bat"`。 此命令将启动随 Visual Studio 2019 Community 一起安装的开发人员命令提示。 根据 Visual Studio 版本和安装位置更改此值。

1. 在“初始目录”字段中，指定将在其中启动命令提示符的目录。 通过选择字段旁边的箭头选择值（如“项目目录”）。

1. 选择“确定”  按钮。

   ![已填写字段值的“外部工具”对话框。](./media/developer-command-prompt-for-vs/add-external-tool.png)

   系统添加了新菜单项，并且你可以从“工具”菜单访问命令提示符。

   ![Visual Studio 中的命令提示符菜单项](./media/developer-command-prompt-for-vs/command-prompt-vs-menu.png)

## <a name="see-also"></a>请参阅

- [.NET Framework 工具](index.md)
- [管理外部工具](/visualstudio/ide/managing-external-tools)
- [通过命令行使用 Microsoft C++ 工具集](/cpp/build/building-on-the-command-line)
