---
title: 面向 Azure .NET 开发人员的工具
description: 获取工具以通过 Windows、Linux 和 Mac 环境开始使用 Azure .NET 库。
ms.date: 06/19/2020
ms.custom: azure-sdk-dotnet
ms.openlocfilehash: fa645b152f15550b25a3542ba0cdbb43799536b9
ms.sourcegitcommit: cb27c01a8b0b4630148374638aff4e2221f90b22
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/09/2020
ms.locfileid: "86174819"
---
# <a name="azure-tools-for-developing-with-net"></a>用于使用 .NET 进行开发的 Azure 工具

## <a name="sdk-downloads"></a>SDK 下载

用于 .NET 的 Azure 库作为 [NuGet 程序包](https://www.nuget.org/packages?q=windowsazureofficial)实现。 若要查看 Azure 服务整理的参考文档，请参阅 [API 参考](/dotnet/api/overview/azure/?view=azure-dotnet)。

## <a name="development-tools"></a>开发工具

Visual Studio 内置了用于许多 Azure 服务的工具。 一些 Azure 服务有其他可用的工具或模拟器，例如 [Azure 存储资源管理器](https://azure.microsoft.com/features/storage-explorer/)。 如有必要，请参阅适用于你的 Azure 服务的文档，以获取其他工具的信息。

在下面选择你的首选开发环境。

## <a name="visual-studio-on-windows"></a>[Windows 上的 Visual Studio](#tab/vs)

Visual Studio 2017 版及更高版本针对 Azure 开发提供了内置支持。 下面的步骤介绍了如何在 Visual Studio 中启动 Azure 开发支持。

对于 Visual Studio 2015 及更早版本，<a href="vs2015-install.md">请按照这些说明操作</a>。

### <a name="step-1-download-visual-studio-2019"></a>步骤 1：下载 Visual Studio 2019

如果已安装 Visual Studio 2019，则可跳过此步骤。

> [!div class="nextstepaction"]
> [下载 Visual Studio 2019](https://www.visualstudio.com/downloads/)

### <a name="step-2-install-the-two-azure-workloads"></a>步骤 2：安装两个 Azure 工作负荷

在 Visual Studio 安装程序中，安装 Visual Studio（或修改现有安装）。 确保已选择“Azure 开发”以及“ASP.NET 和 Web 开发”工作负荷。 

### <a name="step-3-develop-with-net-on-azure"></a>步骤 3：在 Azure 上使用 .NET 进行开发

首先，[在 Azure 应用服务中部署第一个 ASP.NET Core Web 应用](/azure/app-service-web/app-service-web-get-started-dotnet)。

## <a name="visual-studio-code-cross-platform"></a>[Visual Studio Code（跨平台）](#tab/vscode)

Visual Studio Code 提供了进行跨平台 Azure 开发所需的一切内容。

### <a name="step-1-download-the-net-core-sdk"></a>步骤 1：下载 .NET Core SDK

用于 .NET Core 应用的 SDK 和命令行工具。

> [!div class="nextstepaction"]
> [下载 .NET Core SDK](https://dotnet.microsoft.com/download)

### <a name="step-2-visual-studio-code"></a>步骤 2：Visual Studio Code

在任何操作系统上编辑和调试 .NET Core 应用：Windows、Mac 和 Linux。

> [!div class="nextstepaction"]
> [下载 Visual Studio Code](https://code.visualstudio.com)

### <a name="step-3-azure-tools-extension"></a>步骤 3：Azure Tools 扩展

在 Visual Studio Code 中安装 Azure Tools 扩展。

> [!div class="nextstepaction"]
> [安装 Azure Tools 扩展](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack)

### <a name="step-4-develop-with-net-on-azure"></a>步骤 4：在 Azure 上使用 .NET 进行开发

首先，[在 Linux 上的 Azure 应用服务中部署你的第一个 ASP.NET Core Web 应用](/azure/app-service/containers/quickstart-dotnetcore)。

---
