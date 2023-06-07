---
title: 在 Windows 8、Windows 8.1 或 Windows 10 上运行 .NET Framework 1.1 应用
description: 介绍如何适应需要在许多版本的 Windows 操作系统上不再受支持的 .NET Framework 1.1 的应用程序。
ms.date: 05/26/2017
helpviewer_keywords:
- Windows 8, running .NET Framework 1.1 apps
- .NET Framework 1.1, running on Windows 8
ms.assetid: fb14e195-fea5-4561-b9a8-60a67283edb9
ms.openlocfilehash: 6d1cb2f9251bba96d0a378bf4dbab9f7da267037
ms.sourcegitcommit: 267d092663aba36b6b2ea853034470aea493bfae
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/21/2020
ms.locfileid: "80111785"
---
# <a name="run-net-framework-11-apps-on-windows-8-windows-81-or-windows-10"></a>在 Windows 8、Windows 8.1 或 Windows 10 上运行 .NET Framework 1.1 应用

Windows 8、Windows 8.1、Windows Server 2012、Windows Server 2012 R2 或 Windows 10 操作系统上不支持 .NET Framework 1.1。 在某些情况下，运行应用需要 .NET Framework 1.1。 这时，请联系独立软件供应商 (ISV) 以升级该应用，使其可在 .NET Framework 3.5 SP1 或更高版本上运行。 有关详细信息，请参阅[从 .NET Framework 1.1 迁移](../migration-guide/migrating-from-the-net-framework-1-1.md)。

## <a name="install-net-framework-11-from-a-cd-or-download-center"></a>从 CD 或下载中心安装 .NET Framework 1.1

在 Windows 8、Windows 8.1、Windows Server 2012、Windows Server 2012 R2 或 Windows 10 上，无法从 CD 或下载中心手动安装 .NET Framework 1.1。 这种做法不再受支持。 如果尝试安装包，会显示以下错误消息：“无法继续执行安装程序，因为此版本的 .NET Framework 与以前安装的某个版本不兼容。” 若要解决此问题，请安装 [.NET Framework 3.5 SP1](https://www.microsoft.com/download/details.aspx?id=22)。 此版本包括 Windows 8、Windows 8.1 和 Windows 10 上支持的 .NET Framework 2.0（.NET Framework 1.1 的后续版本）。 应始终先尝试安装应用，确定它是否会自动更新到 .NET Framework 的更高版本。 如果不会这样，请与你的 ISV 联系以更新应用。

## <a name="see-also"></a>请参阅

- [从 .NET Framework 1.1 迁移](../migration-guide/migrating-from-the-net-framework-1-1.md)
- [在 Windows 10、Windows 8.1 和 Windows 8 上安装 .NET Framework 3.5](dotnet-35-windows-10.md)
