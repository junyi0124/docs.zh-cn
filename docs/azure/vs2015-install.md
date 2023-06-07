---
title: 用于 Visual Studio 2015 的 Azure 工具
description: 获取工具以开始使用 Visual Studio 2015 中的 Azure .NET 库。
ms.date: 06/19/2020
ms.custom: azure-sdk-dotnet
ms.openlocfilehash: 72229ce0c0276912ddc5658e34f4572a7948a4e6
ms.sourcegitcommit: cb27c01a8b0b4630148374638aff4e2221f90b22
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/09/2020
ms.locfileid: "86174818"
---
# <a name="azure-tools-for-visual-studio-2015"></a>用于 Visual Studio 2015 的 Azure 工具

安装**用于 Visual Studio 2015 的 Azure SDK** 以及**用于 Visual Studio 2015 的 Service Fabric SDK 和工具**的最快、最简单方法是使用 [Web 平台安装程序](https://www.microsoft.com/web/downloads/platform.aspx)。 Microsoft Web 平台安装程序是一个免费工具，可简化 Microsoft Web 平台的某些组件（包括用于 Visual Studio 2015 的 Azure 工具）的下载、安装和更新。 也可以从 [Azure 下载页](https://azure.microsoft.com/downloads/)以独立组件的形式下载并安装这些 SDK。

## <a name="using-the-web-platform-installer"></a>使用 Web 平台安装程序

1. 下载并运行此 [Web 平台安装程序引导程序](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=VWDOrVs2015AzurePack;MicrosoftAzure-ServiceFabric-VS2015)。

2. 该引导程序会安装 Web 平台安装程序（如果需要），并自动将最新版本的**用于 Visual Studio 2015 的 Azure SDK** 以及**用于 Visual Studio 2015 的 Service Fabric SDK 和工具**项放入“要安装的项”列表中。  单击“安装”  。

    ![Web 平台安装程序](media/vs2015-install/webpi.png)

3. 在下一屏幕中，单击“我接受”。  Web PI 会开始下载并安装所选的组件。

4. 安装完成后，会显示确认屏幕。 单击 **“完成”** 。 现在可以关闭 Web 平台安装程序。

## <a name="verifying-the-installation"></a>验证安装

1. 在 Visual Studio 2015 中，依次单击“工具”菜单、“扩展和更新...”。  

2. 显示的列表包含多个 Azure 工具，例如“Microsoft Azure 应用服务工具”、“Microsoft Azure 存储连接服务”和“Service Fabric 工具”。   

    ![扩展和更新](media/vs2015-install/ext-tools.png)
