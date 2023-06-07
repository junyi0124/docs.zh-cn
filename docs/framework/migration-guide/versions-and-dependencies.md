---
title: .NET Framework 和 Windows OS 版本
description: 了解 .NET Framework 每个版本中的关键功能，以及基础 CLR 版本和 Windows 操作系统安装的版本。
ms.date: 01/17/2020
helpviewer_keywords:
- versions, .NET Framework
ms.assetid: f75a72de-e2f2-4a7a-9574-3f278684ea90
ms.openlocfilehash: f807a9d9a7ccebf2ae71d47e01fbfba7338b1799
ms.sourcegitcommit: 870bc4b4087510f6fba3c7b1c0d391f02bcc1f3e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/23/2020
ms.locfileid: "92471992"
---
# <a name="net-framework-versions-and-dependencies"></a>.NET Framework 版本和依赖关系

每个版本的 .NET framework 都包含公共语言运行时 (CLR)、基类库和其他托管库。 本文按版本介绍了 .NET Framework 的关键功能，提供了有关基础 CLR 版本和相关开发环境的信息，并标识了 Windows 操作系统 (OS) 所安装的版本。

每个 .NET Framework 的新版本都会添加新功能，但保留早期版本中的功能。

[!INCLUDE [net-framework-future](../../../includes/net-framework-future.md)]

CLR 由其自己的版本号标识。 .NET Framework 版本号在每次发布时都会递增，但 CLR 版本并不总是递增的。 例如，.NET Framework 4、4.5 和更高版本包含 CLR 4，而 .NET Framework 2.0、3.0 和 3.5 包含 CLR 2.0。 （没有版本 3 的 CLR。）

> [!TIP]
>
> - 有关受支持操作系统的完整列表，请参阅[系统要求](../get-started/system-requirements.md)。
> - 有关下载，请参阅[安装面向开发人员的 .NET Framework](../install/guide-for-developers.md)。
> - 有关确定计算机上已安装哪些 .NET Framework 版本的信息，请参阅[如何确定安装了哪些 .NET Framework 版本](how-to-determine-which-versions-are-installed.md)。

## <a name="version-information"></a>版本信息

下表总结了 .NET Framework 版本历史，并将每个版本与 Visual Studio、Windows 和 Windows Server 相关联。 Visual Studio 支持多目标功能，因此你将不会限于仅使用列出的 .NET Framework 版本。

- 复选标记图标 ✔️ 表示默认安装 .NET Framework 的操作系统版本。
- 加号图标 ➕ 表示 .NET Framework 未安装但可以安装的操作系统版本。
- 星号 \* 表示必须[在控制面板中](../install/dotnet-35-windows-10.md)（如果是 Windows Server，则通过服务器管理器）启用 .NET Framework（无论是否预安装）的操作系统版本。

| | |
| - | - |
| [.NET Framework 4.8](#net-framework-48) | [.NET Framework 4.7.2](#net-framework-472) | [.NET Framework 4.7.1](#net-framework-471) | [.NET Framework 4.7](#net-framework-47) |
| [.NET Framework 4.6.2](#net-framework-462) | [.NET Framework 4.6.1](#net-framework-461) | [.NET Framework 4.6](#net-framework-46) | [.NET Framework 4.5.2](#net-framework-452) |
| [.NET Framework 4.5.1](#net-framework-451) | [.NET Framework 4.5](#net-framework-45) | [.NET Framework 4](#net-framework-4) | [.NET Framework 3.5](#net-framework-35) |
| [.NET Framework 3.0](#net-framework-30) | [.NET Framework 2.0](#net-framework-20) | [.NET Framework 1.1](#net-framework-11) | [.NET Framework 1.0](#net-framework-10) |

### <a name="net-framework-48"></a>.NET Framework 4.8

- [新增功能](../whats-new/index.md#whats-new-in-net-framework-48)
- [辅助功能的新增功能](../whats-new/whats-new-in-accessibility.md#whats-new-in-accessibility-in-net-framework-48)
- [发行说明](https://github.com/Microsoft/dotnet/tree/master/releases/net48/README.md)

|||
|-|-|
|CLR 版本*|4|
|**Windows 版本**|✔️ 10 2019 年 5 月更新<br/>➕ 10 2018 年 10 月更新（版本 1809）<br/>➕ 10 2018 年 4 月更新（版本 1803）<br/>➕ 10 秋季创意者更新（版本 1709）<br/>➕ 10 创意者更新（版本 1703）<br/>➕ 10 周年更新（版本 1607）<br/>➕ 8.1<br/>➕7|
|**Windows Server 版本**|➕ Windows Server 2019<br/>➕ Windows Server 版本 1809<br/>➕ Windows Server 版本 1803<br/>➕ 2016<br/>➕ 2012 R2<br/>➕ 2012<br/>➕ 2008 R2 SP1|
|**确定已安装的 .NET 版本**|使用 `Release` DWORD：<br/>- 528040（Windows 10 2019 年 5 月更新）<br/>- 528049（所有其他 OS 版本）<br/>（请参阅[说明](how-to-determine-which-versions-are-installed.md)）|

### <a name="net-framework-472"></a>.NET Framework 4.7.2

- [新增功能](../whats-new/index.md#whats-new-in-net-framework-472)
- [辅助功能的新增功能](../whats-new/whats-new-in-accessibility.md#whats-new-in-accessibility-in-net-framework-472)
- [发行说明](https://github.com/Microsoft/dotnet/tree/master/releases/net472/README.md)

|||
|-|-|
|**CLR 版本**|4|
|**包含在 Visual Studio 版本中**|2019<sup>1</sup>|
|**Windows 版本**|✔️ 10 2018 年 10 月更新（版本 1809）<br/>✔️ 10 2018 年 4 月更新（版本 1803）<br/>➕ 10 秋季创意者更新（版本 1709）<br/>➕ 10 创意者更新（版本 1703）<br/>➕ 10 周年更新（版本 1607）<br/>➕ 8.1<br/>➕7|
|**Windows Server 版本**|✔️ Windows Server 2019<br/>✔️ Windows Server 版本 1809<br/>✔️ Windows Server 版本 1803<br/>➕ Windows Server 版本 1709<br/>➕ 2016<br/>➕ 2012 R2<br/>➕ 2012<br/>➕ 2008 R2 SP1|
|**确定已安装的 .NET 版本**|使用 `Release` DWORD：<br/>- 461814（Windows 10 2018 年 10 月更新）<br/>- 461808（Windows 10 2018 年 4 月更新和 Windows Server 版本 1803）<br/>- 461814（所有其他操作系统版本）<br/>（请参阅[说明](how-to-determine-which-versions-are-installed.md)）|

<sup>1</sup> 需要安装“.NET 桌面开发”、“ASP.NET 和 Web 开发”、“Azure 开发”、“Office/SharePoint 开发”、“使用 .NET 的移动开发”或“.NET Core 跨平台开发”工作负荷     。

### <a name="net-framework-471"></a>.NET Framework 4.7.1

- [新增功能](../whats-new/index.md#whats-new-in-net-framework-471)
- [辅助功能的新增功能](../whats-new/whats-new-in-accessibility.md#whats-new-in-accessibility-in-net-framework-471)
- [发行说明](https://github.com/Microsoft/dotnet/tree/master/releases/net471/README.md)

|||
|-|-|
|**CLR 版本**|4|
|**Windows 版本**|✔️ 10 秋季创意者更新（版本 1709）<br/>➕ 10 创意者更新（版本 1703）<br/>➕ 10 周年更新（版本 1607）<br/>➕ 8.1<br/>➕7|
|**Windows Server 版本**|➕ Windows Server 版本 1803<br/>✔️ Windows Server 版本 1709<br/>➕ 2016<br/>➕ 2012 R2<br/>➕ 2012<br/>➕ 2008 R2 SP1|
|**确定已安装的 .NET 版本**|使用 `Release` DWORD：<br/>- 461308（Windows 10 创意者更新和 Windows Server 版本 1709）<br/>- 461310（所有其他操作系统版本）<br/>（请参阅[说明](how-to-determine-which-versions-are-installed.md)）|

### <a name="net-framework-47"></a>.NET Framework 4.7

- [新增功能](../whats-new/index.md#whats-new-in-net-framework-47)
- [发行说明](https://github.com/Microsoft/dotnet/tree/master/releases/net47/README.md)

|||
|-|-|
|**CLR 版本**|4|
|**Windows 版本**|✔️ 10 创意者更新（版本 1703）<br/>➕ 10 周年更新（版本 1607）<br/>➕ 8.1<br/>➕7|
|**Windows Server 版本**|➕ 2016<br/>➕ 2012 R2<br/>➕ 2012<br/>➕ 2008 R2 SP1|
|**确定已安装的 .NET 版本**|使用 `Release` DWORD：<br/>- 460798（Windows 10 创意者更新）<br/>- 460805（所有其他操作系统版本）<br/>（请参阅[说明](how-to-determine-which-versions-are-installed.md)）|

### <a name="net-framework-462"></a>.NET Framework 4.6.2

- [新增功能](../whats-new/index.md#whats-new-in-net-framework-462)
- [发行说明](https://github.com/Microsoft/dotnet/tree/master/releases/net462/README.md)

|||
|-|-|
|**CLR 版本**|4|
|**Windows 版本**|✔️ 10 周年更新（版本 1607）<br/>➕ 10 十一月更新（版本 1511）<br/>➕ 10<br/>➕ 8.1<br />➕ 7|
|**Windows Server 版本**|✔️ 2016<br /><br/>➕ 2012 R2<br />➕ 2012<br />➕ 2008 R2 SP1|
|**确定已安装的 .NET 版本**|使用 `Release` DWORD：<br /><br/>- 394802（Windows 10 周年更新和 Windows Server 2016）<br/>- 394806（所有其他操作系统版本）<br /><br/>（请参阅[说明](how-to-determine-which-versions-are-installed.md)）|

### <a name="net-framework-461"></a>.NET Framework 4.6.1

- [新增功能](../whats-new/index.md#whats-new-in-net-framework-461)
- [发行说明](https://github.com/Microsoft/dotnet/tree/master/releases/net461/README.md)

|||
|-|-|
|**CLR 版本**|4|
|**包含在 Visual Studio 版本中**|2017<sup>1</sup>|
|**Windows 版本**|✔️ 10 十一月更新（版本 1511）<br/>➕ 10<br />➕ 8.1<br />➕ 8<br />➕ 7|
|**Windows Server 版本**|➕ 2012 R2<br />➕ 2012<br />➕ 2008 R2 SP1|
|**确定已安装的 .NET 版本**|使用 `Release` DWORD：<br /><br/>- 394254（Windows 10 十一月更新）<br />- 394271（所有其他操作系统版本）<br /><br/>（请参阅[说明](how-to-determine-which-versions-are-installed.md)）|

<sup>1</sup> 需要安装“.NET 桌面开发”、“ASP.NET 和 Web 开发”、“Azure 开发”、“Office/SharePoint 开发”、“使用 .NET 的移动开发”或“.NET Core 跨平台开发”工作负荷     。

### <a name="net-framework-46"></a>.NET Framework 4.6

- [新增功能](../whats-new/index.md#whats-new-in-net-2015)
- [发行说明](https://github.com/Microsoft/dotnet/tree/master/releases/net46/README.md)

|||
|-|-|
|**CLR 版本**|4|
|**包含在 Visual Studio 版本中**|2015|
|**Windows 版本**|✔️ 10<br /><br />➕ 8.1<br />➕ 8<br />➕ 7<br />➕ Vista|
|**Windows Server 版本**|➕ 2012 R2<br />➕ 2012<br />➕ 2008 R2 SP1<br />➕ 2008 SP2|
|**确定已安装的 .NET 版本**|使用 `Release` DWORD：<br /><br />- 393295 (Windows 10)<br />- 393297（所有其他操作系统版本）<br /><br />（请参阅[说明](how-to-determine-which-versions-are-installed.md)）|

### <a name="net-framework-452"></a>.NET Framework 4.5.2

- [新增功能](../whats-new/index.md#whats-new-in-net-framework-452)
- [发行说明](https://github.com/Microsoft/dotnet/tree/master/releases/net452/README.md)

|||
|-|-|
|**CLR 版本**|4|
|**Windows 版本**|➕ 8.1<br />➕ 8<br />➕ 7<br />➕ Vista|
|**Windows Server 版本**|➕ 2012 R2<br />➕ 2012<br />➕ 2008 R2 SP1<br />➕ 2008 SP2|
|**确定已安装的 .NET 版本**|使用 `Release` DWORD 379893<br /><br />（请参阅[说明](how-to-determine-which-versions-are-installed.md)）|

### <a name="net-framework-451"></a>.NET Framework 4.5.1

- [新增功能](../whats-new/index.md#whats-new-in-net-framework-451)
- [发行说明](https://github.com/Microsoft/dotnet/tree/master/releases/net451/README.md)

|||
|-|-|
|**CLR 版本**|4|
|**包含在 Visual Studio 版本中**|2013|
|**Windows 版本**|✔️ 8.1<br /><br />➕ 8<br />➕ 7<br />➕ Vista|
|**Windows Server 版本**|✔️ 2012 R2<br /><br />➕ 2012<br />➕ 2008 R2 SP1<br />➕ 2008 SP2|
|**确定已安装的 .NET 版本**|使用 `Release` DWORD：<br /><br />- 378675 (Windows 8.1)<br />- 378758（所有其他）<br /><br />（请参阅[说明](how-to-determine-which-versions-are-installed.md)）|

### <a name="net-framework-45"></a>.NET Framework 4.5

- [新增功能](../whats-new/index.md#whats-new-in-net-framework-45)
- [发行说明](https://github.com/Microsoft/dotnet/tree/master/releases/net45/README.md)

|||
|-|-|
|**CLR 版本**|4|
|**包含在 Visual Studio 版本中**|2012|
|**Windows 版本**|✔️ 8<br />➕ 7<br />➕ Vista|
|**Windows Server 版本**|✔️ 2012<br />➕ 2008 R2 SP1<br />➕ 2008 SP2|
|**确定已安装的 .NET 版本**|使用 `Release` DWORD 378389<br /><br />（请参阅[说明](how-to-determine-which-versions-are-installed.md)）|

### <a name="net-framework-4"></a>.NET Framework 4

[新增功能](/previous-versions/dotnet/netframework-4.0/ms171868(v=vs.100))

|||
|-|-|
|**CLR 版本**|4|
|**包含在 Visual Studio 版本中**|2010|
|**Windows 版本**|➕ 7<br />➕ Vista|
|**Windows Server 版本**|➕ 2008 R2 SP1<br />➕ 2008 SP2<br />➕ 2003|
|**确定已安装的 .NET 版本**|请参阅[说明](how-to-determine-which-versions-are-installed.md)|

### <a name="net-framework-35"></a>.NET Framework 3.5

[新增功能](/previous-versions/visualstudio/visual-studio-2008/ms171868\(v=vs.90\))：

- LINQ
- 表达式树
- 改进了对 AJAX 开发的 ASP.NET 支持
- HashSet 集合
- DateTimeOffset
- WCF 和 WF 集成
- 对等网络
- 扩展性的加载项

|||
|-|-|
|**CLR 版本**|2.0|
|**包含在 Visual Studio 版本中**|2008|
|**Windows 版本**|✔️ 10\*<br/>✔️ 8.1\*<br />✔️ 8\*<br />✔️ 7<br /><br />➕ Vista|
|**Windows Server 版本**|➕ Windows Server 版本 1803\*<br/>➕ Windows Server 版本 1709\*<br/>➕ 2016\*<br/>➕ 2012 R2\*<br />➕ 2012\*<br /><br />✔️2008 R2 SP1\*<br /><br/>➕ 2008 SP2<br />➕ 2003|
|**确定已安装的 .NET 版本**|请参阅[说明](how-to-determine-which-versions-are-installed.md)|

### <a name="net-framework-30"></a>.NET Framework 3.0

[新增功能](/previous-versions/visualstudio/visual-studio-2008/bb822048(v=vs.90))：

- Windows Presentation Foundation
- Windows Communication Foundation
- Windows Workflow Foundation
- Windows CardSpace

|||
|-|-|
|**CLR 版本**|2.0|
|**Windows 版本**|✔️ Vista|
|**Windows Server 版本**|✔️ 2008 R2 SP1*<br />✔️ 2008 SP2\*<br /><br />➕ 2003|
|**确定已安装的 .NET 版本**|请参阅[说明](how-to-determine-which-versions-are-installed.md)。|

### <a name="net-framework-20"></a>.NET Framework 2.0

[新增功能](/previous-versions/visualstudio/visual-studio-2008/t357fb32(v=vs.90))：

- 泛型
- 调试器编辑并继续
- 提高了可伸缩性和性能
- ClickOnce 部署
- 在 ASP.NET 2.0 中，提供了新控件以及对各种浏览器的支持
- 64 位支持

|||
|-|-|
|**CLR 版本**|2.0|
|**包含在 Visual Studio 版本中**|2005|
|**Windows 版本**|不可用|
|**Windows Server 版本**|✔️ 2008 R2 SP1<br />✔️ 2008 SP2<br />✔️ 2003|
|**确定已安装的 .NET 版本**|请参阅[说明](how-to-determine-which-versions-are-installed.md)|

### <a name="net-framework-11"></a>.NET Framework 1.1

[新增功能](/previous-versions/visualstudio/visual-studio-2008/h88tthh0(v=vs.90))：

- ASP.NET 移动控件
- 并行执行
- IPv6 支持

|||
|-|-|
|**CLR 版本**|1.1|
|**包含在 Visual Studio 版本中**|2003|
|**Windows 版本**|不可用|
|**Windows Server 版本**|✔️ 2003|
|**确定已安装的 .NET 版本**|请参阅[说明](how-to-determine-which-versions-are-installed.md)|

### <a name="net-framework-10"></a>.NET Framework 1.0

|||
|-|-|
|**CLR 版本**|1.0|
|**包含在 Visual Studio 版本中**|Visual Studio .NET|
|**Windows 版本**|不可用|
|**Windows Server 版本**|不可用|
|**确定已安装的 .NET 版本**|请参阅[说明](how-to-determine-which-versions-are-installed.md)|

> [!NOTE]
>
> - 必须通过[控制面板（适用于 Windows）或服务器管理器（适用于 Windows Server）](../install/dotnet-35-windows-10.md#enable-the-net-framework-35-in-control-panel)在此操作系统上启用 .NET Framework。
> - 通常，不应卸载计算机上安装的 .NET Framework 的任何版本，因为你使用的应用程序可能依赖于特定版本，如果移除该版本，则应用程序可能会中断。 可以在一台计算机上同时加载 .NET Framework 的多个版本。 这意味着，你可以安装 .NET Framework 而无需卸载早期版本。 有关详细信息，请参阅[入门](../get-started/index.md)。

## <a name="remarks-for-version-45-and-later"></a>版本 4.5 及更高版本的备注

.NET Framework 4.5 是可替换计算机上的 .NET Framework 4 的就地更新，同样，.NET Framework 4.5.1、4.5.2、4.6、4.6.1、4.6.2、4.7、4.7.1、4.7.2 和 4.8 是对 .NET Framework 4.5 的就地更新。 就地更新意味着它们使用相同的运行时版本，但是程序集版本会更新，并且包括新类型和成员。 安装其中一个更新后，.NET Framework 4、.NET Framework 4.5、.NET Framework 4.6 或 .NET Framework 4.7 应用应继续运行，而无需重新编译。 但是，反过来则不行。 建议不要在较早版本的 .NET Framework 上运行面向更高版本的 .NET Framework 的应用。 例如，我们不建议在 .NET Framework 4.5 上运行面向 .NET Framework 4.6 的应用。

以下准则将适用：

- 在 Visual Studio 中，可以选择 .NET Framework 4.5 作为项目的目标框架（这将设置 <xref:Microsoft.Build.Tasks.GetReferenceAssemblyPaths.TargetFrameworkMoniker%2A?displayProperty=nameWithType> 属性），以将项目编译为 .NET Framework 4.5 程序集或可执行文件。 此程序集或可执行文件随后可用于安装了 .NET Framework 4.5、4.5.1、4.5.2、4.6、4.6.1、4.6.2、4.7、4.7.1、4.7.2 或 4.8 的任何计算机。

- 在 Visual Studio 中，可以选择 .NET Framework 4.5.1 作为项目​​的目标框架，以将项目编译为 .NET Framework 4.5.1 程序集或可执行文件。 仅在已安装 .NET Framework 4.5.1 或更高版本的计算机上运行此程序集或可执行文件。 将阻止面向 .NET Framework 4.5.1 的可执行文件在仅安装了更低版本的 .NET Framework（​​例如 .NET Framework 4.5）的计算机上运行。 系统会提示用户安装 .NET Framework 4.5.1。 此外，不应从面向更低版本的 .NET Framework（例如 .NET Framework 4.5）的应用中调用 .NET Framework 4.5.1 程序集。

  > [!NOTE]
  > .NET Framework 4.5.1 和 .NET Framework 4.5 在此处仅用作示例。 所述原则适用于任意满足以下条件的应用：应用所面向的 .NET framework 版本高于运行该应用的系统上的 .NET framework 版本。

.NET Framework 中的某些更改可能需要更改应用代码；在使用 .NET Framework 4.5 或更高版本运行现有应用前，请参阅[应用程序兼容性](application-compatibility.md)。 若要了解如何安装当前版本，请参阅[安装面向开发人员的 .NET Framework](../install/guide-for-developers.md)。 有关对 .NET Framework 的支持的信息，请参阅 .NET 网站上的 [.NET Framework 官方支持策略](https://dotnet.microsoft.com/platform/support/policy/dotnet-framework)。

## <a name="remarks-for-older-versions"></a>旧版本的备注

.NET Framework 版本 2.0、3.0 和 3.5 使用同一 CLR 版本 (CLR 2.0) 生成。 这些版本表示单个安装的连续层。 每个版本将基于早期版本以增量方式生成。 无法在计算机上并行运行版本 2.0、3.0 和 3.5。 在安装 3.5 版时，你将自动获得 2.0 和 3.0 层，并且为版本 2.0、3.0 和 3.5 生成的应用程序均可在 3.5 版上运行。 但是，.NET Framework 4 结束了此分层方法，且 .NET Framework 4 及更高版本（.NET Framework 4.5、4.5.1、4.5.2、4.6、4.6.1、4.6.2、4.7、4.7.1、4.7.2 和 4.8）也表示单个安装的连续层。 从 .NET Framework 4 开始，可使用进程内并行托管在单个进程中运行 CLR 的多个版本。 有关详细信息，请参阅[程序集和并行执行](../../standard/assembly/side-by-side-execution.md)。

此外，如果应用面向 2.0、3.0 或 3.5 版，你的用户可能需要先在 Windows 8、Windows 8.1 或 Windows 10 计算机上启用 .NET Framework 3.5，然后才能运行应用。 有关详细信息，请参阅[在 Windows 10、Windows 8.1 和 Windows 8 上安装 .NET Framework 3.5](../install/dotnet-35-windows-10.md)。

## <a name="next-steps"></a>后续步骤

- 如果不熟悉 .NET Framework，请参阅[概述](../get-started/overview.md)以大致了解关键概念和功能。

- 有关 .NET Framework 4.5 及其单点版本中的新功能和改进，请参阅 [.NET Framework 中的新增功能](../whats-new/index.md)。

- 有关将应用迁移到较新版本的 .NET Framework 的信息，请参阅[迁移指南](index.md)。

- 有关确定计算机上安装了哪些版本或更新的信息，请参阅[如何：确定安装了哪些 .NET Framework 版本](how-to-determine-which-versions-are-installed.md)和[如何：确定安装了哪些 .NET Framework 更新](how-to-determine-which-net-framework-updates-are-installed.md)。

## <a name="see-also"></a>请参阅

- [版本兼容性](version-compatibility.md)
| [.NET Framework 官方支持策略](https://dotnet.microsoft.com/platform/support/policy/dotnet-framework)
- [安装和卸载 .NET Framework 受阻疑难解答](../install/troubleshoot-blocked-installations-and-uninstallations.md)
