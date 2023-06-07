---
title: 安装和卸载 .NET Framework 受阻疑难解答
description: 解决遇到的 .NET Framework 安装受阻问题。 有关解决问题的信息，请参阅状态消息。
ms.date: 04/18/2019
helpviewer_keywords:
- .NET Framework, troubleshooting blocked installations
- blocked .NET Framework installations, troubleshooting
ms.assetid: c3fdfbc1-ed99-4202-a2b0-8c4f1646385d
ms.openlocfilehash: 9f4ae96a148d850efd92f950a177b32cc75c33ab
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96283167"
---
# <a name="troubleshoot-blocked-net-framework-installations-and-uninstallations"></a>安装和卸载 .NET Framework 受阻疑难解答

运行 .NET Framework 4.5 或更高版本的 [Web 或脱机安装程序时](guide-for-developers.md)，可能会遇到禁止或阻止安装 .NET Framework 的问题。 下表列出了可能产生的阻碍问题，并提供了指向疑难解答信息的链接。

在 Windows 8 和更高版本的操作系统上，.NET Framework 是一个操作系统组件，不能单独卸载。 .NET Framework 的更新会出现在控制面板中“程序和功能”  应用的“已安装的更新”  选项卡上。 对于其他没有预安装 .NET Framework 的操作系统，.NET Framework 会出现在控制面板中“程序和功能”  应用的“卸载或更改程序”  选项卡（或“添加/删除程序”  选项卡）上。 有关预安装了 .NET Framework 的 Windows 版本的信息，请参阅[系统需求](../get-started/system-requirements.md)。

> [!IMPORTANT]
> 由于 .NET Framework 4.x 版本是就地更新，不能在已装有更高版本 .NET Framework 4.x 的系统上安装其早期版本。 例如，在使用 Windows 10 Fall Creators Update 的系统上无法安装 .NET Framework 4.6.2，因为 .NET Framework 4.7.1 已随操作系统预安装。

可以确定系统上安装了哪些版本的 .NET Framework。 请参阅[如何：确定已安装的 .NET Framework 版本](../migration-guide/how-to-determine-which-versions-are-installed.md)以获取详细信息。

在此表中，4.5.x 指 .NET Framework 4.5 及其点发行版 4.5.1 和 4.5.2，4.6.x 指 .NET Framework 4.6 及其点发行版 4.6.1 和 4.6.2，4.7.x 指 .NET Framework 4.7 及其点发行版 4.7.1 和 4.7.2，4.8 指 .NET Framework 4.8。

|阻止消息|了解更多信息或解决此问题|  
|----------------------|--------------------------------------------------|  
|卸载 Microsoft .NET Framework 可能会导致某些应用程序无法正常工作。|通常，你不应卸载计算机上安装的 .NET Framework 的任何版本，因为你使用的应用程序可能取决于 .NET Framework 的特定版本。 有关详细信息，请参阅 *入门* 指南中的 [面向用户的 .NET Framework](../get-started/index.md#ForUsers)。|  
|此计算机上已安装 .NET Framework 4.5.x/4.6.x/4.7.x (ENU) 或更高版本。|无需执行任何操作。<br /><br /> 要确定系统上安装了哪些版本的 .NET Framework，请参阅[如何：确定已安装的 .NET Framework 版本](../migration-guide/how-to-determine-which-versions-are-installed.md)。|  
|.NET Framework 4.5.x/4.6.x/4.7.x/4.8（语言）要求安装 .NET Framework 4.5.x/4.6.x/4.7.x/4.8  。 请从下载中心安装 .NET Framework 4.5.x/4.6.x/4.7.x/4.8 并重新运行安装程序。|你必须先安装 .NET Framework 指定版本的英文版，然后再安装语言包。 有关详细信息，请参阅安装指南中有关[安装语言包](guide-for-developers.md#to-install-language-packs)一节。|  
|无法安装 .NET Framework 4.5.x/4.6.x/4.7.x/4.8。 你的计算机上的其他应用程序与此程序不兼容。<br /><br /> \- 或 -<br /><br /> 你的计算机上的其他应用程序与此程序不兼容。|导致出现此消息的最可能的原因是安装了 .NET Framework 的预览版或 RC 版。 卸载预览版或 RC 版，然后重新运行安装程序。|  
|无法使用此包卸载 .NET Framework 4.5.x/4.6.x/4.7.x/4.8。 若要从计算机中卸载 .NET Framework 4.5.x/4.6.x/4.7.x/4.8，请转到“控制面板”，然后依次选择“程序和功能”、“查看已安装的更新”、“Microsoft Windows (KB2828152) 的更新”和“卸载”     。|你正在安装的程序包不会卸载 .NET Framework 的预览版或 RC 版。<br /><br /> 从“控制面板”卸载预览版或 RC 版。|  
|无法卸载 .NET Framework 4.5.x/4.6.x/4.7.x/4.8。 计算机上的其他应用程序依赖于此程序。|通常，你不应该从计算机卸载 .NET Framework 的任何版本，因为你使用的应用程序可能基于 .NET Framework 的特定版本。 有关详细信息，请参阅 *入门* 指南中的 [面向用户的 .NET Framework](../get-started/index.md#ForUsers)。|  
|.NET Framework 4.5.x/4.6.x/4.7.x/4.8 可再发行组件不适用于此操作系统。 请从 .NET Framework 下载页下载适用于自己的操作系统的 .NET Framework 4.5.x/4.6.x/4.7.x/4.8。|可能是因为尝试在不受支持的平台上安装 .NET Framework 4.5.1、4.5.2、4.6、4.6.1、4.6.2、4.7、4.7.1、4.7.2 或 4.8，也可能是因为已选择的安装程序包不包含适用于所有受支持操作系统的组件。 使用适用于 [4.7.1](https://go.microsoft.com/fwlink/p/?LinkId=852090)、[4.7.2](https://dotnet.microsoft.com/download/dotnet-framework/net472) 或 [4.8](https://dotnet.microsoft.com/download/dotnet-framework/net48) 的脱机安装程序（[适用于 4.5.1](https://go.microsoft.com/fwlink/p/?LinkId=309493)、[4.5.2](https://dotnet.microsoft.com/download/dotnet-framework/net452)、[4.6](https://dotnet.microsoft.com/download/dotnet-framework/net46)、[4.6.1](https://dotnet.microsoft.com/download/dotnet-framework/net461)、[4.6.2](https://go.microsoft.com/fwlink/p/?LinkId=780604)、[4.7](https://go.microsoft.com/fwlink/p/?LinkId=825306)）再次运行安装。 有关详细信息，请参阅适用于受支持操作系统的[安装指南](guide-for-developers.md)和[系统需求](../get-started/system-requirements.md)。|  
|安装本产品之前需要先安装 KB\<*number*> 所对应的更新。|.NET Framework 安装需要在安装 .NET Framework 之前先安装 KB 更新。 安装此更新，然后再次开始 .NET Framework 安装。<br /><br /> 例如，在 Windows 8.1、Windows RT 8.1 和 Windows Server 2012 R2 上安装更新版本的 .NET Framework 需要先安装 [KB 2919355](https://support.microsoft.com/kb/2919355) 所对应的更新。|  
|你的计算机当前正在运行 Windows Server 2008 操作系统的服务器核心安装。 .NET Framework 4.5.x 需要使用更高版本的操作系统。 请安装 Windows Server 2008 R2 SP1 或更高版本并重新运行 .NET Framework 4.5.x 安装程序。|.NET Framework 4.5.1 和 4.5.2 在带有 Windows Server 2008 R2 SP1 或更高版本的服务器核心角色中受支持。 请参阅[系统需求](../get-started/system-requirements.md)。|  
|你没有足够的权限为此计算机的所有用户完成此操作。 以管理员身份登录并重新运行“安装程序”  。|若要安装 .NET Framework，你必须是计算机的管理员。|  
|先前的安装要求重新启动计算机，所以安装程序无法继续。 请重新启动计算机，然后重新运行安装程序。|有时需要重新启动以完全完成安装。 根据说明重新启动计算机并重新运行安装程序。<br /><br /> 在极少数情况下，如果 Windows 检测到缺少一些更新并需要重新启动来安装队列中的下一个更新，则可能要求不止一次重新启动系统。|  
|.NET Framework 安装程序无法在程序兼容性模式下运行。|请参阅本文后面的[程序兼容性问题](#compat)一节。|  
|.NET Framework 4.5.x/4.6.x/4.7.x/4.8 尚未安装，因为组件存储已损坏。|有关详细信息，请参阅[使用 DISM 或系统更新准备工具修复 Windows 更新错误](https://support.microsoft.com/kb/947821)。|  
|安装程序无法运行，因为 Windows Installer 服务在此计算机上不可用。|请参阅 Microsoft 支持网站上的[尝试在 Windows 7 或 Windows Vista 中安装程序时出现的“无法访问 Windows Installer 服务”错误](https://support.microsoft.com/help/2642495/the-windows-installer-service-could-not-be-accessed-error-when-you-try)。|  
|安装程序可能无法正常运行，因为 Windows Update 服务在此计算机上不可用。|可将计算机配置为使用 Windows Server Update Services (WSUS) 而非 Microsoft Windows Update。 有关详细信息，请参阅 [.NET Framework 3.5 安装错误：0x800F0906、0x800F081F、0x800F0907](https://support.microsoft.com/help/2734782/net-framework-3-5-installation-error-0x800f0906-0x800f081f-0x800f0907) 中的错误代码 0x800F0906 部分。<br /><br /> 另请参阅 Microsoft 支持网站上的[如何将 Windows 更新代理更新至最新版本](https://support.microsoft.com/help/949104/how-to-update-the-windows-update-agent-to-the-latest-version)。|  
|安装程序可能无法正常运行，因为后台智能传输服务 (BITS) 在此计算机上不可用。|请参阅 Microsoft 支持网站上的[用于修复基于 Windows Vista 的计算机上的后台智能传输服务 (BITS) 发生崩溃的更新](https://support.microsoft.com/help/940520/an-update-is-available-to-fix-a-background-intelligent-transfer-servic)。|  
|安装程序可能无法正常运行，因为 Windows 更新遇到了错误并显示错误代码 0x80070643 或 0x643。|请参阅 Microsoft 支持网站上的 [.NET Framework 更新安装错误：“0x80070643”或“0x643”](https://support.microsoft.com/kb/976982)。|  
|此操作系统已包含 .NET Framework 4.5.x/4.6.x/4.7.x/4.8。 无需安装 .NET Framework 4.5.x/4.6.x/4.7.x/4.8 可再发行组件。|不执行任何操作。<br /><br /> 要确定系统上安装了哪些版本的 .NET Framework，请参阅[如何：确定已安装的 .NET Framework 版本](../migration-guide/how-to-determine-which-versions-are-installed.md)。 请参阅[系统需求](../get-started/system-requirements.md)以了解受支持的操作系统。|  
|此操作系统不支持 .NET Framework 4.5.x/4.6.x/4.7.x/4.8。|请参阅[系统需求](../get-started/system-requirements.md)以了解受支持的操作系统。<br /><br /> Windows 7 上安装 .NET Framework 失败时，此消息通常会指示未安装 Windows 7 SP1。 在 Windows 7 系统上，.NET Framework 要求安装 Windows 7 SP1。 如果你使用的是 Windows 7 系统，但尚未安装 Service Pack 1，则需要先安装 SP1，然后才能安装 .NET Framework。 有关安装 Windows 7 SP1 的信息，请参阅[了解如何安装 Windows 7 Service Pack 1 (SP1)](https://windows.microsoft.com/windows7/install-windows-7-service-pack-1)。|  
|你的计算机当前正在运行 Windows Server 2008 操作系统的服务器核心安装。 .NET Framework 4.5.x 需要完整版本的操作系统或 Server Core 2008 R2 SP1。 请安装完整版的 Windows Server 2008 SP2、Windows Server 2008 R2 SP1 或 Server Core 2008 R2 SP1，然后重新运行 .NET Framework 4.5.x 安装程序。|.NET Framework 在带有 Windows Server 2008 R2 SP1 或更高版本的服务器核心角色中受支持。 请参阅[系统需求](../get-started/system-requirements.md)。|  
|.NET Framework 4.5.x 已包含在此操作系统中，但当前处于关闭状态（仅限 Windows Server 2012）。| 使用“控制面板”中的“打开或关闭 Windows 功能”，以打开 .NET Framework 4.5.x   。 |  
|此安装程序要求使用 x86 计算机。 不能在 x64 或 IA64 计算机上安装此程序。|请参阅[系统需求](../get-started/system-requirements.md)。|  
|此安装程序要求使用 x64 或 x86 计算机。 不能在 IA64 计算机上安装此程序。|请参阅[系统需求](../get-started/system-requirements.md)。|  

<a name="compat"></a>

### <a name="program-compatibility-issues"></a>程序兼容性问题

.NET Framework 4.5 及其点发行版的安装失败并显示错误代码 1603，或在 Windows 程序兼容性模式下运行时受到阻止。 “程序兼容性助手”指示可能未正确安装 .NET Framework，并提示使用建议的设置（程序兼容性模式）重新安装它。  程序兼容性模式还可能已由“程序兼容性助手”在之前运行 .NET Framework 安装程序失败或取消尝试时进行了设置。

.NET Framework 安装程序无法在程序兼容性模式下运行。 若要解决此阻碍问题，必须使用注册表编辑器以确保未在系统范围内启用兼容性模式设置：

1. 选择“开始”  按钮，然后再选择“运行”  。

1. 在“运行”  对话框中，键入“regedit”，然后选择“确定”  。

1. 在注册表编辑器中，浏览到以下子项：

   - HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows NT\CurrentVersion\AppCompatFlags\Compatibility Assistant\Persisted

   - HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows NT\CurrentVersion\AppCompatFlags\Layers

1. 在“名称”列中查找 .NET Framework 4.5、4.5.1、4.5.2、4.6、4.6.1、4.6.2、4.7、4.7.1 或 4.7.2 下载名称（具体取决于要安装的版本），然后删除这些项。 有关下载名称，请参阅[安装面向开发人员的 .NET Framework](guide-for-developers.md) 一文。

1. 重新运行版本 4.5、4.5.1、4.5.2 或 4.6、4.6.1、4.6.2、4.7、4.7.1 或 4.7.2 的 .NET Framework 安装程序。

## <a name="see-also"></a>请参阅

- [安装面向开发人员的 .NET Framework](guide-for-developers.md)
- [如何：确定已安装的 .NET Framework 版本](../migration-guide/how-to-determine-which-versions-are-installed.md)
- [版本和依赖关系](../migration-guide/versions-and-dependencies.md)
