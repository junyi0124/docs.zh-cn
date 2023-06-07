---
title: 安装本地化的 IntelliSense 文件
description: 了解如何设置开发计算机，以便在 Visual Studio 中为 .NET 5+ 项目（包含 .NET Core）使用本地化的 IntelliSense 文件。
ms.date: 11/06/2020
ms.openlocfilehash: 45eeae12ca79179cacb3d48fca28118de70e0a4f
ms.sourcegitcommit: bc9c63541c3dc756d48a7ce9d22b5583a18cf7fd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/11/2020
ms.locfileid: "94506761"
---
# <a name="how-to-install-localized-intellisense-files-for-net"></a>如何为 .NET 安装本地化的 IntelliSense 文件

[IntelliSense](/visualstudio/ide/using-intellisense) 是一种代码完成辅助工具，可以在不同的集成开发环境 (IDE) 中使用，例如 Visual Studio。 默认情况下，在开发 .NET 项目时，SDK 仅包含英语版本的 IntelliSense 文件。 本文介绍：

- 如何安装这些文件的本地化版本。
- 如何修改 Visual Studio 安装以使用其他语言。

## <a name="prerequisites"></a>系统必备

- [.NET Core 3.0 SDK](https://dotnet.microsoft.com/download/dotnet-core) 或更高版本，例如 [.NET 5 SDK](https://dotnet.microsoft.com/download/dotnet/5.0)。
- [Visual Studio 2019 版本 16.3](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) 或更高版本。

## <a name="download-and-install-the-localized-intellisense-files"></a>下载并安装本地化的 IntelliSense 文件

> [!IMPORTANT]
> 此过程需具有管理员权限，才能将 IntelliSense 文件复制到 .NET 安装文件夹中。

1. 转到[下载 IntelliSense 文件](https://dotnet.microsoft.com/download/intellisense)页面。

1. 下载要使用的语言和版本的 IntelliSense 文件。

1. 提取 zip 文件的内容。

1. 导航到 .NET Intellisense 文件夹。

   1. 导航到 .NET 安装文件夹。 默认情况下，它位于 %ProgramFiles%\dotnet\packs 下  。
   1. 选择要为其安装 IntelliSense 的 SDK，然后导航到关联的路径。 有下列选项：

      | SDK 类型              | 路径                               |
      |-----------------------|------------------------------------|
      | .NET 5+ 和 .NET Core | Microsoft.NETCore.App.Ref         |
      | Windows 桌面       | Microsoft.WindowsDesktop.App.Ref  |
      | .NET Standard         | NETStandard.Library.Ref           |

   1. 导航到要为其安装本地化 IntelliSense 的版本。 例如，5.0.0。
   1. 打开 ref 文件夹  。
   1. 打开 moniker 文件夹。 例如 net5.0。

   因此，要导航到的完整路径看起来将类似于 C:\Program Files\dotnet\packs\Microsoft.NETCore.App.Ref\5.0.0\ref\net5.0。

1. 在刚打开的 moniker 文件夹中创建一个子文件夹。 文件夹名称指示要使用的语言。 下表指定了不同的选项：

   | 语言              | 文件夹名称 |
   | --------------------- | ----------- |
   | 巴西葡萄牙语  | pt-br      |
   | 中文（简体）  | zh-hans    |
   | 中文（繁体） | zh-hant    |
   | 法语                | fr         |
   | 德语                | de         |
   | 意大利语               | it         |
   | 日语              | ja         |
   | 朝鲜语                | ko         |
   | 俄语               | ru         |
   | 西班牙语               | es         |

1. 将在步骤 3 中提取的 .xml 文件复制到此新文件夹。 .xml 文件按 SDK 文件夹细分，因此，请将它们复制到步骤 4 中选择的相应 SDK。

## <a name="modify-visual-studio-language"></a>修改 Visual Studio 语言

要使 Visual Studio 使用其他语言的 IntelliSense，请安装适当的语言包。 这可以在[安装过程中](/visualstudio/install/install-visual-studio#step-6---install-language-packs-optional)完成，也可以之后通过修改 Visual Studio 安装来完成。 如果已将 Visual Studio 配置为所需的语言，那么 IntelliSense 安装已准备就绪。

### <a name="install-the-language-pack"></a>安装语言包

如果在安装过程中未安装所需的语言包，请按以下步骤更新 Visual Studio 来安装语言包：

> [!IMPORTANT]
> 若要安装、更新或修改 Visual Studio，必须使用具有管理员权限的帐户登录。 有关详细信息，请参阅[用户权限与 Visual Studio](/visualstudio/ide/user-permissions-and-visual-studio)。

1. 在计算机上找到 Visual Studio 安装程序。

   例如，在运行 Windows 10 的计算机上，选择“开始”，然后滚动到字母“V”，它作为“Visual Studio 安装程序”在那里列出    。

   ![在 Windows 中打开 Visual Studio 安装程序](./media/localized-intellisense/vs-installer-windows-start.png)

   > [!NOTE]
   > 还可以在以下位置中找到 Visual Studio 安装程序：
   >
   > `C:\Program Files (x86)\Microsoft Visual Studio\Installer\vs_installer.exe`

   可能需要先更新安装程序，然后才能继续操作。 如果是这样，请按照提示操作。

1. 在安装程序中，查找要为其添加语言包的 Visual Studio 版本，然后选择“修改”  。

   ![更新或修改 Visual Studio](./media/localized-intellisense/vs-installer-modify.png)

   > [!IMPORTANT]
   > 如果未看到“修改”按钮，但看到了“更新”按钮，则需要先更新 Visual Studio，才能修改安装   。
   > 更新完成后，应会显示“修改”按钮  。

1. 在“语言包”选项卡中，选择或取消选择要安装或卸载的语言  。

   ![Visual Studio 语言包选项卡](./media/localized-intellisense/vs-modify-language-packs.png)

1. 选择“修改”  。 更新开始。

### <a name="modify-language-settings-in-visual-studio"></a>修改 Visual Studio 中的语言设置

安装所需的语言包后，请修改 Visual Studio 设置以使用其他语言：

1. 打开 Visual Studio。

1. 在“启动”窗口中，选择“继续但无需代码”  。

1. 在菜单栏上，选择“工具”   >   “选项”。 “选项”对话框随即打开。

1. 在“环境”节点下，选择“国际设置”   。

1. 在“语言”下拉列表中，选择所需的语言  。 选择 **“确定”** 。

1. 随即会显示一个对话框，告知必须重启 Visual Studio 才能使所做的更改生效。 选择 **“确定”** 。

1. 重新启动 Visual Studio。

之后，当打开面向刚安装的 IntelliSense 文件版本的 .NET 项目时，IntelliSense 应会按预期方式工作。

## <a name="see-also"></a>另请参阅

- [Visual Studio 中的 IntelliSense](/visualstudio/ide/using-intellisense)
