---
title: 在 Windows 10、8.1、8 上安装 .NET Framework 3.5
description: 了解如何在 Windows 10、Windows 8.1 和 Windows 8 上安装 .NET Framework 3.5。
ms.date: 07/16/2018
ms.openlocfilehash: a385c46d2b3b384bc0a413d1dfa80e88ba7fb00a
ms.sourcegitcommit: 67ebdb695fd017d79d9f1f7f35d145042d5a37f7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/20/2020
ms.locfileid: "92223811"
---
# <a name="install-the-net-framework-35-on-windows-10-windows-81-and-windows-8"></a>在 Windows 10、Windows 8.1 和 Windows 8 上安装 .NET Framework 3.5

必须安装 .NET Framework 3.5，才能在 Windows 10、Windows 8.1 和 Windows 8 上运行应用。 对于旧版 Windows，也可以按照以下说明操作。

## <a name="download-the-offline-installer"></a>下载脱机安装程序

.NET Framework 3.5 SP1 脱机安装程序可在 [.NET Framework 3.5 SP1 下载页](https://dotnet.microsoft.com/download/dotnet-framework/net35-sp1)上找到，并且适用于 Windows 10 之前的 Windows 版本。

## <a name="install-the-net-framework-35-on-demand"></a>按需安装.NET Framework 3.5

如果尝试运行的应用要求安装 .NET Framework 3.5，则会看到以下配置对话框。 选择“安装此功能”  ，启用 .NET Framework 3.5。 此选项需要 Internet 连接。

![.NET Framework 安装对话框的屏幕截图。](./media/dotnet-35-windows-10/dotnet-framework-installation-dialog.png)

### <a name="why-am-i-getting-this-pop-up"></a>为什么我会看到此弹出项？

.NET Framework 是由 Microsoft 创建，用于提供应用程序运行环境。 有多种不同版本。 许多公司都开发使用 .NET Framework 运行的应用程序，并且这些应用都定目标到具体版本。 如果看到此弹出项，表明尝试运行的应用程序需要 .NET Framework 版本 3.5，但未在系统上安装此版本。

## <a name="enable-the-net-framework-35-in-control-panel"></a>在控制面板中启用 .NET Framework 3.5

可以通过 Windows 控制面板启用 .NET Framework 3.5。 此选项需要 Internet 连接。

1. 按下键盘上的 Windows 徽标键 ![Windows 徽标键徽标的屏幕截图](./media/dotnet-35-windows-10/windows-keyboard-logo.png)， 键入“Windows 功能”，然后按 Enter。 随即显示“打开或关闭 Windows 功能”对话框  。

2. 如果弹出提示，选择“.NET Framework 3.5 (包括 .NET 2.0 和 3.0)” 复选框，选择“确定”，然后重启计算机   。

   ![显示通过控制面板安装 .NET 的屏幕截图。](./media/dotnet-35-windows-10/dotnet-control-panel.png)

   无需选择“Windows Communication Foundation (WCF) HTTP 激活”  和“Windows Communication Foundation (WCF) 非 HTTP 激活”  的子项，除非是需要使用此功能的开发者或服务器管理员。

## <a name="troubleshoot-the-installation-of-the-net-framework-35"></a>.NET Framework 3.5 安装疑难解答

安装过程中，你可能会遇到错误 0x800f0906、0x800f0907、0x800f081f 或 0x800F0922，此时请参阅 [.NET Framework 3.5 安装错误：0x800f0906、0x800f0907 或 0x800f081f](https://support.microsoft.com/help/2734782/net-framework-3-5-installation-error-0x800f0906--0x800f081f--0x800f09)，了解如何解决这些问题。

如果仍无法解决安装问题，或未连接到 Internet，可以尝试使用 Windows 安装介质进行安装。 有关详细信息，请参阅[使用部署映像服务和管理 (DISM) 部署 .NET Framework 3.5](/windows-hardware/manufacture/desktop/deploy-net-framework-35-by-using-deployment-image-servicing-and-management--dism)。 如果使用的是 Windows 7、Windows 8.1 或最新的 Windows 10 版本，但没有安装媒体，请在此处创建最新的安装媒体：[为 Windows 创建安装媒体](https://support.microsoft.com/help/15088/windows-create-installation-media)。 有关 Windows 10 按需功能的附加信息：[按需功能](/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities)。

> [!WARNING]
> 如果不依赖 Windows 更新作为源来安装 .NET Framework 3.5，则必须确保严格使用来自相同的、对应的 Windows 操作系统版本的源。 使用来自不同 Windows 操作系统版本的源将安装与 .NET Framework 3.5 不匹配的版本，或导致安装失败，使系统处于不受支持和无法提供服务的状态。
