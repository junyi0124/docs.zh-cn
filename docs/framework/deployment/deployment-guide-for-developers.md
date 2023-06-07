---
title: .NET Framework 部署指南（针对开发人员）
description: 阅读面向开发人员的 .NET 部署指南。 如果想要随应用一起安装任何版本的 .NET（从版本 4.5 到 4.8），请使用此信息。
ms.date: 01/17/2020
helpviewer_keywords:
- developer's guide, deploying .NET Framework
- deployment [.NET Framework], developer's guide
ms.assetid: 094d043e-33c4-40ba-a503-e0b20b55f4cf
ms.openlocfilehash: 950ff048a918a52e6c64d8cc15e1ef0f80233b88
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96238647"
---
# <a name="net-framework-deployment-guide-for-developers"></a>.NET Framework 部署指南（针对开发人员）

本主题为想要随自己的应用一起安装任何 .NET Framework 版本（从 .NET Framework 4.5 到 [!INCLUDE[net_current](../../../includes/net-current-version.md)]）的开发人员提供了相关信息。

可从下载页面下载针对 .NET Framework 的可再发行组件包和语言包：

- [.NET Framework 4.8](https://dotnet.microsoft.com/download/dotnet-framework/net48)
- [.NET Framework 4.7.2](https://dotnet.microsoft.com/download/dotnet-framework/net472)
- [.NET Framework 4.7.1](https://dotnet.microsoft.com/download/dotnet-framework/net471)
- [.NET Framework 4.7](https://dotnet.microsoft.com/download/dotnet-framework/net47)
- [.NET Framework 4.6.2](https://dotnet.microsoft.com/download/dotnet-framework/net462)
- [.NET Framework 4.6.1](https://dotnet.microsoft.com/download/dotnet-framework/net461)
- [.NET Framework 4.6](https://dotnet.microsoft.com/download/dotnet-framework/net46)
- [.NET Framework 4.5.2](https://dotnet.microsoft.com/download/dotnet-framework/net452)
- [.NET Framework 4.5.1](https://dotnet.microsoft.com/download/dotnet-framework/net451)
- [.NET Framework 4.5](https://dotnet.microsoft.com/download/dotnet-framework/net45)

 重要说明：

- 从 .NET Framework 4.5.1 到 [!INCLUDE[net_current](../../../includes/net-current-version.md)] 的 .NET Framework 版本已就地更新到 .NET Framework 4.5，这意味着它们将使用相同的运行时版本，但是程序集版本会进行更新，并且包括新类型和成员。

- .NET Framework 4.5 及更高版本以增量方式在 .NET Framework 4上创建。 在安装了 .NET Framework 4 的系统上安装 .NET Framework 4.5 或更高版本时，版本 4 程序集将替换为更新版本。

- 如果你正在应用中引用一个 Microsoft [带外包](../get-started/the-net-framework-and-out-of-band-releases.md) ，则程序集将包括在应用包内。

- 你必须拥有管理员特权才能安装 .NET Framework 4.5 或更高版本。

- Windows 8 和 Windows Server 2012 中已经包括 .NET Framework 4.5，因此你不必在这些操作系统上随你的应用一起部署此组件。 同样，Windows 8.1 和 Windows Server 2012 R2 中也已包括 .NET Framework 4.5.1。 .NET Framework 4.5.2 不包含在任何操作系统中。 .NET Framework 4.6 包括在 Windows 10 中，.NET Framework 4.6.1 包括在 Windows 10 11 月更新中，而 .NET Framework 4.6.2 包括在 Windows 10 周年更新中。  .NET Framework 4.7 包含在 Windows 10 创意者更新中，.NET Framework 4.7.1 包含在 Windows 10 Fall Creators Update 中，.NET Framework 4.7.2 包含在 Windows 10 2018 年 10 月更新和 Windows 10 2018 年 4 月更新中。 .NET Framework 4.8 包含在 Windows 10 2019 年 5 月更新中。 有关硬件和软件要求的完整列表，请参阅[系统要求](../get-started/system-requirements.md)。

- 从 .NET Framework 4.5 开始，你的用户可以在安装过程中查看运行中 .NET Framework 应用的列表并轻松关闭这些应用。 这可能有助于避免系统因安装 .NET Framework 而重新启动。 参见 [减少系统重新启动](reducing-system-restarts.md)。

- 卸载 .NET Framework 4.5 或更高版本也会删除预先存在的 .NET Framework 4 文件。 若要返回到 .NET Framework 4，你必须重新安装它以及它的任何更新。 请参阅[安装 .NET Framework 4](/previous-versions/dotnet/netframework-4.0/5a4x27ek(v=vs.100))。

- .NET Framework 4.5 可再发行组件于 2012 年 10 月 9 日进行了更新，纠正了一个与数字证书中的错误时间戳相关的问题，此问题会导致 Microsoft 生成并签名的文件中的数字签名提前过期。 如果之前安装了日期为 2012 年 8 月 16 日的 .NET Framework 4.5 可再发行组件包，建议使用 [.NET Framework 下载页](https://dotnet.microsoft.com/download/dotnet-framework/net45)内的最新可再发行组件来更新副本。 有关此问题的更多信息，请参阅 [Microsoft 安全公告 2749655](/security-updates/SecurityAdvisories/2012/2749655)。

有关系统管理员可如何在网络中部署 .NET Framework 及其系统依赖项的信息，请参见阅[适用于管理员的部署指南](guide-for-administrators.md)。

## <a name="deployment-options-for-your-app"></a>应用的部署选项

当准备好将你的应用发布到 Web 服务器或其他集中位置时，以便用户可以安装此应用时，你可以选择多种部署方法。 其中一些随 Visual Studio 一起提供。 下表列出了你的应用的部署选项，并指定了支持每个选项的 .NET Framework 可再发行组件包。 除此之外，你还可以为你的应用编写自定义安装程序；有关更多信息，请参见 [将 .NET Framework 安装链接到应用的安装程序](#chaining)一节。

|你的应用的部署策略|可用的部署方法|要使用的 .NET Framework 可再发行组件|
|--------------------------------------|----------------------------------|-------------------------------------------|
|从 Web 安装|- [InstallAware](#installaware-deployment)<br />- [InstallShield](#installshield-deployment)<br />- [WiX 工具集](#wix)<br />- [手动安装](#installing_manually)|[Web installer](#redistributable-packages)|
|从磁盘安装|- [InstallAware](#installaware-deployment)<br />- [InstallShield](#installshield-deployment)<br />- [WiX 工具集](#wix)<br />- [手动安装](#installing_manually)|[Offline installer](#redistributable-packages)|
|从局域网安装（适用于企业应用）|- [ClickOnce](#clickonce-deployment)|[Web 安装程序](#redistributable-packages) （有关限制，请参见 [ClickOnce](#clickonce-deployment) ）或 [脱机安装程序](#redistributable-packages)|

## <a name="redistributable-packages"></a>可再发行组件包

.NET framework 通过两种可再发行组件包提供：Web 安装程序（引导程序）和脱机安装程序（独立的可再发行组件）。 所有 .NET Framework 下载都托管在[下载 .NET Framework 页](https://dotnet.microsoft.com/download/dotnet-framework/)。 下表对两种组件包进行了比较：

||Web 安装程序|脱机安装程序|
|-|-------------------|-----------------------|
|是否需要 Internet 连接？|是|否|
|下载文件大小|较小（仅包含面向目标平台的安装程序）*|较大*|
|语言包|包括**|除非使用面向所有操作系统的程序包，否则必须 [单独安装](#chain_langpack)|
|部署方法|支持所有方法：<br /><br />- [ClickOnce](#clickonce-deployment)<br />- [InstallAware](#installaware-deployment)<br />- [InstallShield](#installshield-deployment)<br />- [Windows Installer XML (WiX)](#wix)<br />- [手动安装](#installing_manually)<br />- [自定义安装（链接）](#chaining)|支持所有方法：<br /><br /> - [ClickOnce](#clickonce-deployment)<br />- [InstallAware](#installaware-deployment)<br />- [InstallShield](#installshield-deployment)<br />- [Windows Installer XML (WiX)](#wix)<br />- [手动安装](#installing_manually)<br />- [自定义安装（链接）](#chaining)|

\* 因为脱机安装程序包含面向所有目标平台的组件，因此它更大。 当运行完安装程序后，Windows 操作系统仅会缓存已用的安装程序。 如果安装后删除脱机安装程序，则所使用的磁盘空间与 Web 安装程序使用的空间相同。 如果你用来创建应用安装程序的工具（例如，[InstallAware](#installaware-deployment) 或 [InstallShield](#installshield-deployment)）提供了一个会在安装后移除的安装文件文件夹，则将脱机安装程序放入此安装文件夹可自动删除该安装程序。

\*\* 如果使用支持自定义安装的 Web 安装程序，则可以使用基于用户的多语言用户界面 (MUI) 的默认语言设置，或使用命令行上的 `/LCID` 选项指定其他语言包。 有关示例，请参见 [使用默认 .NET Framework UI 进行链接](#chaining_default) 一节。

## <a name="deployment-methods"></a>部署方法

 有四种部署方法可用：

- 可以设置一个对 .NET Framework 的依赖项。 使用下列方法之一，你可以将 .NET Framework 指定为应用安装程序中的一个必备组件：

  - 使用 [ClickOnce 部署](#clickonce-deployment) （适用于 Visual Studio）

  - 创建 [InstallAware 项目](#installaware-deployment)（为 Visual Studio 用户提供免费版）

  - 创建 [InstallShield 项目](#installshield-deployment) （适用于 Visual Studio）

  - 使用 [Windows Installer XML (WiX) 工具集](#wix)

- 你可以要求用户 [手动安装 .NET Framework](#installing_manually)。

- 你可以在应用的安装程序中链接（包括） .NET Framework 安装过程，并确定要如何处理 .NET Framework 安装体验：

  - [使用默认 UI](#chaining_default)。 由 .NET Framework 安装程序提供安装体验。

  - [自定义 UI](#chaining_custom) 以提供一种统一安装体验并监视 .NET Framework 的安装进度。

以下各节将详细讨论这些部署方法。

## <a name="setting-a-dependency-on-the-net-framework"></a>设置对 .NET Framework 的依赖项

如果使用 ClickOnce、InstallAware、InstallShield 或 WiX 来部署你的应用，则可以添加一个对 .NET Framework 的依赖项，以便将其作为应用的一部分进行安装。

### <a name="clickonce-deployment"></a>ClickOnce 部署

ClickOnce 部署适用于使用 Visual Basic 和 Visual C# 创建的项目，但不适用于 Visual C++。

在 Visual Studio 中，若要选择 ClickOnce 部署并添加对 .NET Framework 的依赖项：

1. 打开要发布的应用项目。

2. 在“解决方案资源管理器”中，打开项目的快捷菜单，然后选择 **“属性”** 。

3. 选择 **“发布”** 窗格。

4. 选择 **“系统必备”** 按钮。

5. 在 **“系统必备”** 对话框中，确保选中 **“创建用于安装系统必备组件的安装程序”** 复选框。

6. 在必备组件列表中，找到并选择已用于生成项目的.NET Framework 版本。

7. 选择一个选项以指定必备组件的源位置，然后选择 **“确定”** 。

     如果你提供 .NET Framework 下载位置的 URL，则可以指定 .NET Framework 下载页或你自己的网站。 如果要将可再发行组件包放置在自己的服务器上，则它必须为脱机安装程序，而不是 Web 安装程序。 你只能链接到 .NET Framework 下载页上的 Web 安装程序。 该 URL 还可以指定一个用于分发你的应用的磁盘。

8. 在 **“属性页”** 对话框中，选择 **“确定”** 。

<a name="installaware"></a>

### <a name="installaware-deployment"></a>InstallAware 部署

InstallAware 从单个源生成 Windows 应用 (APPX)、Windows Installer (MSI)、本机代码 (EXE) 和 APP-V (Application Virtualization) 包。 可以通过[编辑默认脚本](https://www.installaware.com/msicode.htm)自定义安装，轻松在你的安装程序中[包括任何版本的 .NET Framework](https://www.installaware.com/one-click-pre-requisite-installer.htm)。 例如，InstallAware 会在 Windows 7 上预安装证书，否则 .NET Framework 4.7 安装程序将失败。 有关 InstallAware 的详细信息，请参阅[面向 Windows Installer 的 InstallAware](https://www.installaware.com/) 网站。

### <a name="installshield-deployment"></a>InstallShield 部署

InstallShield 可生成 Windows 应用包（MSIX、APPX）、Windows Installer 包 (MSI) 和本机代码 (EXE) 安装程序。 InstallShield 还提供 Visual Studio 集成。 有关详细信息，请访问 [InstallShield](https://www.flexerasoftware.com/install/products/installshield.html) 网站。

<a name="wix"></a>

### <a name="windows-installer-xml-wix-deployment"></a>Windows Installer XML (WiX) 部署

Windows Installer XML (WiX) 工具集通过 XML 源代码生成 Windows 安装包。 WiX 支持一种命令行环境，可以将其集成到你的生成过程中，以便生成 MSI 和 MSM 安装包。 通过使用 WiX，你可以 [将 .NET Framework 指定为系统必备组件](https://wixtoolset.org/documentation/manual/v3/howtos/redistributables_and_install_checks/install_dotnet.html)，或 [创建链接器](https://wixtoolset.org/documentation/manual/v3/xsd/wix/exepackage.html) 来完全控制 .NET Framework 的部署体验。 有关 WiX 的更多信息，请参阅 [Windows Installer XML (WiX) 工具集](https://wixtoolset.org/) 。

<a name="installing_manually"></a>

## <a name="installing-the-net-framework-manually"></a>手动安装 .NET Framework

在某些情况下，随你的应用一起自动安装 .NET Framework 可能不切实际。 此时，可以让用户自行安装 .NET Framework。 可再发行组件包通过 [两个包](#redistributable-packages)提供。 在安装过程中，请提供有关说明，介绍用户应如何查找并安装 .NET Framework。

<a name="chaining"></a>

## <a name="chaining-the-net-framework-installation-to-your-apps-setup"></a>将 .NET Framework 安装链接到应用的安装程序

如果要为你的应用创建自定义安装程序，则可以在应用的安装过程中链接（包括）.NET Framework 安装过程。 链接提供了两种 .NET Framework 安装 UI 选项：

- 使用由 .NET Framework 安装程序提供的默认 UI。

- 为 .NET Framework 安装创建自定义 UI，与你的应用安装程序保持一致。

这两种方法都允许使用 Web 安装程序或脱机安装程序。 每个包都具有自己的优点：

- 如果使用 Web 安装程序，则 .NET Framework 安装过程将确定所需的安装包，然后从 Web 仅下载并安装该包。

- 如果使用脱机安装程序，则你可以在再发行介质中包括一套完整的 .NET Framework 安装包，以便用户在 Web 安装过程中不需要从 Web 下载任何其他文件。

<a name="chaining_default"></a>

### <a name="chaining-by-using-the-default-net-framework-ui"></a>使用默认 .NET Framework UI 进行链接

若要静默链接 .NET Framework 安装过程并由 .NET Framework 安装程序来提供 UI，请将下列命令添加到你的安装程序：

`<.NET Framework redistributable> /q /norestart /ChainingPackage <PackageName>`

例如，如果你的可执行程序是 Contoso.exe 并且希望静默安装 .NET Framework 4.5 脱机可再发行组件包，请使用以下命令：

`dotNetFx45_Full_x86_x64.exe /q /norestart /ChainingPackage Contoso`

你可以使用其他命令行选项来自定义安装。 例如：

- 若要为用户提供一种方法来关闭运行中的 .NET Framework 应用，从而最大程度减少系统重新启动，请按照如下方式设置被动模式并使用 `/showrmui` 选项：

    `dotNetFx45_Full_x86_x64.exe /norestart /passive /showrmui /ChainingPackage Contoso`

     此命令允许重新启动管理器显示一个消息框，用户可以在安装 .NET Framework 之前关闭 .NET Framework 应用。

- 如果使用 Web 安装程序，则可以使用 `/LCID` 选项来指定语言包。 例如，若要将 .NET Framework 4.5 Web 安装程序链接到 Contoso 安装程序并安装日语语言包，请将下列命令添加到应用的安装过程：

    `dotNetFx45_Full_setup.exe /q /norestart /ChainingPackage Contoso /LCID 1041`

     如果省略 `/LCID` 选项，则安装程序将安装与用户的 MUI 设置匹配的语言包。

    > [!NOTE]
    > 不同的语言包可能具有不同的发布日期。 如果下载中心不提供你所指定的语言包，则安装程序将安装不带语言包的 .NET Framework。 如果用户计算机中已安装 .NET Framework，则安装程序将仅安装语言包。

有关选项的完整列表，请参见 [命名行选项](#command-line-options) 一节。

有关常见的返回代码，请参见 [返回代码](#return-codes) 一节。

<a name="chaining_custom"></a>

### <a name="chaining-by-using-a-custom-ui"></a>使用自定义 UI 进行链接

如果具有自定义安装包，你可能希望静默启动和跟踪 .NET Framework 安装过程，同时显示自己的安装进度视图。 这种情况下，请确保你的代码涵盖以下操作：

- 检查 [.NET Framework 硬件和软件要求](../get-started/system-requirements.md)。

- [检测](#detect_net) 用户计算机中是否已安装 .NET Framework 的正确版本。

    > [!IMPORTANT]
    > 确定是否安装了 .NET Framework 的正确版本时，应检查安装的是目标版本 *还是* 更高版本，而不是确定是否安装了目标版本。 换而言之，应检查从注册表中检索的版本密钥是否大于或等于目标版本的版本密钥，而 *不是* 检查其是否等于目标版本的版本密钥。

- [检测](#detecting-the-language-packs) 用户计算机中是否已安装语言包。

- 如果要控制部署，则静默启动并跟踪 .NET Framework 安装过程（请参阅[如何：获取 .NET Framework 4.5 安装程序的进度](how-to-get-progress-from-the-dotnet-installer.md)。

- 如果要部署脱机安装程序，则 [单独链接语言包](#chain_langpack)。

- 使用 [命令行选项](#command-line-options)自定义部署。 例如，如果要链接 .NET Framework Web 安装程序，但是想重写默认语言包，请使用 `/LCID` 选项（如上一节中所述）。

- [疑难解答](#troubleshooting)。

<a name="detect_net"></a>

### <a name="detecting-the-net-framework"></a>检测 .NET Framework

.NET Framework 安装程序会在安装成功时写入一些注册表项。 通过在注册表中检查 `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\NET Framework Setup\NDP\v4\Full` 文件夹中名为 `Release` 的值 `DWORD`，可以测试是否安装了 .NET Framework 4.5 或更高版本。 （请注意，“NET Framework Setup”不以句点开头。）存在此项即表明计算机中已安装 .NET Framework 4.5 或更高版本。 `Release` 的值用于指示已安装的 .NET Framework 版本。

> [!IMPORTANT]
> 当尝试检测是否存在特定版本时，应检查是否存在  **大于或等于** 版本关键字值的值。

[!INCLUDE[Release key values note](~/includes/version-keys-note.md)]

|Version|Release DWORD 的值|
|-------------|--------------------------------|
|在 Windows 10 2019 年 5 月更新上安装的 .NET Framework 4.8|528040|
|在除 Windows 10 2019 年 5 月更新之外的所有操作系统版本上安装的 .NET Framework 4.8|528049|
|安装在 Windows 10 2018 年 4 月更新和 Windows Server 版本 1803 上的 .NET Framework 4.7.2|461808|
|.NET Framework 4.7.2，安装在除 Windows 10 2018 年 4 月更新和 Windows Server 版本 1803 之外的所有 OS 版本上。 这包括 Windows 10 2018 年 10 月更新。 |461814|
|安装在 Windows 10 秋季创意者更新和 Windows Server 版本 1709 上的 .NET Framework 4.7.1|461308|
|安装在除 Windows 10 秋季创意者更新和 Windows Server 版本 1709 之外的所有 OS 版本上的 .NET Framework 4.7.1|461310|
|在 Windows 10 创意者更新上安装的 .NET Framework 4.7|460798|
|在除 Windows 10 创意者更新之外的所有操作系统版本上安装的 .NET Framework 4.7|460805|
|在 Windows 10 周年纪念版和 Windows Server 2016 上安装的 .NET Framework 4.6.2|394802|
|在除 Windows 10 周年纪念版和 Windows Server 2016 以外的所有操作系统版本上安装的 .NET Framework 4.6.2|394806|
|在 Windows 10 11 月更新上安装的 .NET Framework 4.6.1|394254|
|在除 Windows 10 11 月更新外的所有操作系统版本上安装的 .NET Framework 4.6.1|394271|
|在 Windows 10 上安装的 .NET Framework 4.6|393295|
|在除 Windows 10 之外的所有操作系统版本上安装的 .NET Framework 4.6|393297|
|.NET Framework 4.5.2|379893|
|使用 Windows 8.1 或 Windows Server 2012 R2 安装的 .NET Framework 4.5.1|378675|
|在 Windows 8 和 Windows 7 上安装的 .NET Framework 4.5.1|378758|
|.NET Framework 4.5|378389|

### <a name="detecting-the-language-packs"></a>检测语言包

可以测试是否已安装特定语言包，方法是在注册表的 HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\NET Framework Setup\NDP\v4\Full\\LCID 文件夹内查找名为 `Release` 的 DWORD 值。 （请注意，“NET Framework Setup”不以句点开头。）LCID 指定一个区域设置标识符；有关这些标识符的列表，请参阅[支持的语言](#supported-languages)。

例如，若要检测是否安装了完整的日语语言包 (LCID=1041)，请在注册表中检索以下命名值：

| | |
|-|-|
| 键 | HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\NET Framework Setup\NDP\v4\Full\1041 |
| “属性” | Release |
| 类型 | DWORD |

若要确定是否已安装 .NET Framework 4.5 到 4.7.2 之间的某个特定版本的最终版本语言包，请按照前一部分[检测 .NET Framework](#detect_net) 中所述检查 RELEASE 项 DWORD 值的值。

<a name="chain_langpack"></a>

### <a name="chaining-the-language-packs-to-your-app-setup"></a>将语言包链接到应用安装程序

.NET Framework 提供了一组独立的语言包可执行文件，其中包含适用于特定区域的本地化资源。 这些语言包可以从下载 .NET Framework 页获得：

- [.NET Framework 4.8](https://dotnet.microsoft.com/download/dotnet-framework/net48)
- [.NET Framework 4.7.2](https://dotnet.microsoft.com/download/dotnet-framework/net472)
- [.NET Framework 4.7.1](https://dotnet.microsoft.com/download/dotnet-framework/net471)
- [.NET Framework 4.7](https://dotnet.microsoft.com/download/dotnet-framework/net47)
- [.NET Framework 4.6.2](https://dotnet.microsoft.com/download/dotnet-framework/net462)
- [.NET Framework 4.6.1](https://dotnet.microsoft.com/download/dotnet-framework/net461)
- [.NET Framework 4.6](https://dotnet.microsoft.com/download/dotnet-framework/net46)
- [.NET Framework 4.5.2](https://dotnet.microsoft.com/download/dotnet-framework/net452)
- [.NET Framework 4.5.1](https://dotnet.microsoft.com/download/dotnet-framework/net451)
- [.NET Framework 4.5](https://dotnet.microsoft.com/download/dotnet-framework/net45)

> [!IMPORTANT]
> 语言包不包含运行应用所需的 .NET Framework 组件；你必须在安装语言包之前使用 Web 或脱机安装程序安装 .NET Framework。

从 .NET Framework 4.5.1 开始，包名称采用以下形式 NDP<`version`>-KB<`number`>-x86-x64-AllOS-<`culture`>.exe，其中 `version` 是 .NET Framework 的版本号，`number` 是 Microsoft 知识库文章编号，`culture` 则指定[国家/地区](#supported-languages)。 例如， `NDP452-KB2901907-x86-x64-AllOS-JPN.exe`就是其中一个安装包。 包名称详见本文前面的 [Redistributable Packages](#redistributable-packages) 一节。

若要随 .NET framework 脱机安装程序一起安装语言包，你必须将其链接到应用的安装程序。 例如，若要部署带有日语语言包的.NET Framework 4.5.1 脱机安装程序，请使用下列命令：

`NDP451-KB2858728-x86-x64-AllOS-JPN.exe /q /norestart /ChainingPackage <ProductName>`

如果使用 Web 安装程序，则不必链接语言包；安装程序将安装与用户的 MUI 设置匹配的语言包。 如果要安装其他语言，则可以使用 `/LCID` 选项指定语言包。

有关命名行选项的完整列表，请参见 [命名行选项](#command-line-options) 一节。

### <a name="troubleshooting"></a>疑难解答

#### <a name="return-codes"></a>返回代码

下表列出了 .NET Framework 可再发行安装程序的常见返回代码。 所有版本的安装程序的返回代码都是相同的。 有关详细信息的链接，请参见下一节。

|返回代码|描述|
|-----------------|-----------------|
|0|已成功完成安装。|
|1602|用户已取消安装。|
|1603|安装期间发生错误。|
|1641|需要重新启动才能完成安装。 此消息指示安装成功。|
|3010|需要重新启动才能完成安装。 此消息指示安装成功。|
|5100|用户计算机不满足系统要求。|

#### <a name="download-error-codes"></a>下载错误代码

查看以下内容：

- [后台智能传输服务 (BITS) 错误代码](https://go.microsoft.com/fwlink/?LinkId=180946)

- [URL 名字对象错误代码](https://go.microsoft.com/fwlink/?LinkId=180947)

- [WinHttp 错误代码](https://go.microsoft.com/fwlink/?LinkId=180948)

#### <a name="other-error-codes"></a>其他错误代码

查看以下内容：

- [Windows Installer 错误代码](https://go.microsoft.com/fwlink/?LinkId=180949)

- [Windows 更新代理结果代码](https://go.microsoft.com/fwlink/?LinkId=180951)

## <a name="uninstalling-the-net-framework"></a>卸载 .NET Framework

从 Windows 8 开始，你可以使用“控制面板”中的“打开或关闭 Windows 功能”来卸载 .NET Framework 4.5 或更高版本。 在旧版本 Windows 中，你可以使用“控制面板”中的“添加或删除程序”来卸载 .NET Framework 4.5 或更高版本。

> [!IMPORTANT]
> 对于 Windows 7 和更低版本的操作系统，卸载 .NET Framework 4.5.1、4.5.2、4.6、4.6.1、4.6.2、4.7、4.7.1、4.7.2 或 4.8 不会还原 .NET Framework 4.5 文件，卸载 .NET Framework 4.5 不会还原 .NET Framework 4 文件。 如果要回到旧版本，你必须重新安装此版本及其任何更新。

## <a name="appendix"></a>附录

### <a name="command-line-options"></a>命名行选项

下表列出了将 .NET Framework 4.5 可再发行组件链接到应用安装程序时可以包括的选项。

|选项|描述|
|------------|-----------------|
|**/CEIPConsent**|覆盖默认行为并向 Microsoft 发送匿名反馈以改善将来的部署体验。 仅当安装程序询问你是否同意安装并且用户授权向 Microsoft 发送匿名反馈时，才可以使用此选项。|
|**/chainingpackage** `packageName`|指定执行链接的可执行文件的名称。 向 Microsoft 发送此信息作为匿名反馈以帮助改进将来的部署体验。<br /><br /> 如果包名称包含空格，则可以用双引号作为分隔符；例如： **/chainingpackage "Lucerne Publishing"** 。 有关链接包的示例，请参阅[从安装包获取进度信息](/previous-versions/cc825975(v=vs.100))。|
|**/LCID**  `LCID`<br /><br /> 其中 `LCID` 指定一个区域设置标识符（请参见 [支持的语言](#supported-languages)）|安装由 `LCID` 指定的语言包，并强制使用此语言显示 UI（除非设置为安静模式）。<br /><br /> 对于 Web 安装程序，此选项将从 Web 链接并安装语言包。 **注意：** 只能随 Web 安装程序一起使用此选项。|
|**/log** `file` &#124; `folder`|指定日志文件的位置。 默认为过程的临时文件夹，默认文件名基于安装包。 如果文件扩展名为 .txt，则生成文本日志。 如果指定其他扩展名或不指定扩展名，则创建 HTML 日志。|
|**/msioptions**|指定要为 .msi 和 .msp 项传递的选项；例如： `/msioptions "PROPERTY1='Value'"`。|
|**/norestart**|防止安装程序自动重新启动。 如果使用此选项，则链接应用必须捕获返回代码并处理重新启动操作（请参阅[从安装包获取进度信息](/previous-versions/cc825975(v=vs.100))）。|
|**/passive**|设置被动模式。 显示用于指示安装正在进行的进度栏，但不向用户显示任何提示或错误消息。 在此模式下，当链接包被安装程序链接时，它必须处理 [返回代码](#return-codes)。|
|**/pipe**|创建一个信道，使链接包可以获取进度。|
|**/promptrestart**|仅在被动模式下，如果安装程序需要重新启动，则会提示用户。 如果需要重新启动，则此选项会要求用户进行交互。|
|**/q**|设置安静模式。|
|**/repair**|触发修复功能。|
|**/serialdownload**|强制仅在已下载安装包后才进行安装。|
|**/showfinalerror**|设置被动模式。 仅在安装未成功时显示错误。 如果安装未成功，则此选项要求用户交互。|
|**/showrmui**|仅与 **/passive** 选项一起使用。 显示一个消息框，提示用户关闭当前正在运行的 .NET Framework 应用。 此消息框在被动和非被动模式下具有相同的行为。|
|**/uninstall**|卸载 .NET Framework 可再发行组件。|

### <a name="supported-languages"></a>支持的语言

下表列出了可用于 .NET Framework 4.5 及更高版本的 .NET Framework 语言包。

|LCID|语言 - 国家/地区|culture|
|----------|--------------------------------|-------------|
|1025|阿拉伯语 - 沙特阿拉伯|ar|
|1028|中文 - 繁体|zh-Hant|
|1029|捷克语|cs|
|1030|丹麦语|da|
|1031|德语 - 德国|de|
|1032|希腊语|el|
|1035|芬兰语|fi|
|1036|法语 - 法国|fr|
|1037|希伯来语|he|
|1038|匈牙利语|hu|
|1040|意大利语 - 意大利|it|
|1041|日语|ja|
|1042|朝鲜语|ko|
|1043|荷兰语 - 荷兰|nl|
|1044|挪威语（博克马尔语）|否|
|1045|波兰语|pl|
|1046|葡萄牙语 - 巴西|pt-BR|
|1049|俄语|ru|
|1053|瑞典语|sv|
|1055|土耳其语|tr|
|2052|中文 - 简体|zh-Hans|
|2070|葡萄牙语 - 葡萄牙|pt-PT|
|3082|西班牙语 - 西班牙（现代排序）|es|

## <a name="see-also"></a>请参阅

- [面向管理员的部署指南](guide-for-administrators.md)
- [系统要求](../get-started/system-requirements.md)
- [安装面向开发人员的 .NET Framework](../install/guide-for-developers.md)
- [安装和卸载 .NET Framework 受阻疑难解答](../install/troubleshoot-blocked-installations-and-uninstallations.md)
- [在 .NET Framework 4.5 安装期间减少系统重新启动次数](reducing-system-restarts.md)
- [如何：获取 .NET Framework 4.5 安装程序的进度](how-to-get-progress-from-the-dotnet-installer.md)
