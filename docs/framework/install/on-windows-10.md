---
title: 在 Windows 10 上安装 .NET Framework
description: 了解如何在 Windows 10 或 Windows Server 2016 上安装 .NET Framework。
ms.date: 04/18/2019
ms.openlocfilehash: 8bd54133f67395d0f110dd24e9e77521bbfe2bab
ms.sourcegitcommit: 8bfeb5930ca48b2ee6053f16082dcaf24d46d221
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/18/2020
ms.locfileid: "88558772"
---
# <a name="install-the-net-framework-on-windows-10-and-windows-server-2016-and-later"></a>在 Windows 10 和 Windows Server 2016 及更高版本上安装 .NET Framework

在 Windows 上运行许多应用程序需要 .NET Framework。 本文中的相关说明可帮助你安装所需的 .NET Framework 版本。 [.NET Framework 4.8](https://github.com/Microsoft/dotnet/tree/master/releases/net48) 是可用的最新版本。

在尝试运行应用程序后，你可能转到了此页并在计算机上看到一个对话框，如下所示：

![无法启动此应用程序](./media/this-application-could-not-be-started.png)

## <a name="net-framework-48"></a>.NET Framework 4.8

.NET Framework 4.8 随附：

- [Windows 10 2019 年 5 月更新](https://support.microsoft.com/help/4028685/windows-10-get-the-update)

> [!div class="button"]
> [下载 .NET Framework 4.8](https://dotnet.microsoft.com/download/dotnet-framework/net48)

[.NET Framework 4.8](https://dotnet.microsoft.com/download/dotnet-framework/net48) 可用于运行针对 .NET Framework 4.0 到 4.7.2 生成的应用程序。

[.NET Framework 4.8](https://dotnet.microsoft.com/download/dotnet-framework/net48) 可安装于：

- Windows 10 2018 年 10 月更新（版本 1809）
- Windows 10 2018 年 4 月更新（版本 1803）
- Windows 10 秋季创意者更新（版本 1709）
- Windows 10 创意者更新（版本 1703）
- Windows 10 周年更新（版本 1607）
- Windows Server 2019
- Windows Server 版本 1809
- Windows Server 版本 1803
- Windows 2016 Server

以下系统不支持 .NET Framework 4.8：

- Windows 10 1507
- Windows 10 1511

如果正在使用 Windows 10 1507 或 1511，且想要安装 .NET Framework 4.8，首先需要升级到较新的 Windows 10 版本。

## <a name="net-framework-462"></a>.NET Framework 4.6.2

[.NET Framework 4.6.2](https://dotnet.microsoft.com/download/dotnet-framework/net462) 是 Windows 10 1507 和 1511 上支持的最新 .NET Framework 版本。

.NET Framework 4.6.2 支持针对 .NET Framework 4.0 到 4.6.2 生成的应用。

## <a name="net-framework-35"></a>.NET Framework 3.5

请按照说明操作，[在 Windows 10 上安装 .NET Framework 3.5](dotnet-35-windows-10.md)。

.NET Framework 3.5 支持针对 .NET Framework 1.0 到 3.5 生成的应用。

## <a name="additional-information"></a>其他信息

.NET Framework 4.x 版本是早期版本的就地更新版。 这意味着：

- 计算机上只能安装一个版本的 .NET Framework 4.x。

- 如果计算机上已安装更高版本，则不能安装 .NET Framework 的早期版本。

- .NET Framework 4.x 版本可用于运行针对 .NET Framework 4.0 到该版本生成的应用程序。 例如，.NET Framework 4.7 可用于运行针对 .NET Framework 4.0-4.7 生成的应用程序。 最新版本（即 .NET Framework 4.8）可用于运行使用从 .NET Framework 4.0 开始的 .NET Framework 的所有版本生成的应用程序。

如需可供下载的 .NET Framework 所有版本的列表，请参阅 [.NET 下载](https://dotnet.microsoft.com/download)页。

## <a name="help"></a>帮助

如果无法确定已安装 .NET Framework 的正确版本，可以[联系 Microsoft 获取帮助](mailto:dotnet-install-help@service.microsoft.com?subject=Install-Help)。

## <a name="see-also"></a>请参阅

- [.NET 下载](https://dotnet.microsoft.com/download)
- [安装和卸载 .NET Framework 受阻疑难解答](troubleshoot-blocked-installations-and-uninstallations.md)
- [安装面向开发人员的 .NET Framework](guide-for-developers.md)
- [确定已安装的 .NET Framework 版本](../migration-guide/how-to-determine-which-versions-are-installed.md)
