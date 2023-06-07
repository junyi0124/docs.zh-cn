---
title: 如何安装模型生成器
description: 了解如何安装 ML.NET 模型生成器工具
author: luisquintanilla
ms.author: luquinta
ms.date: 11/21/2019
ms.custom: mvc, how-to, mlnet-tooling
ms.openlocfilehash: b944d7f6044553251132824e7e4213119e34500e
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2020
ms.locfileid: "78848647"
---
# <a name="how-to-install-mlnet-model-builder"></a>如何安装 ML.NET 模型生成器

了解如何安装 ML.NET 模型生成器以将机器学习添加到 .NET 应用程序。

> [!NOTE]
> 模型生成器当前为预览版。

## <a name="prerequisites"></a>系统必备

- Visual Studio 2017 版本 15.9.12 或更高版本/Visual Studio 2019
- .NET Core 2.1 SDK 或更高版本。

> [!NOTE]
> 暂不支持 .NET Core 3.0 SDK。

## <a name="limitations"></a>限制

- ML.NET 模型生成器扩展目前仅适用于 Windows 上的 Visual Studio。
- 1 GB 的训练数据集限制
- SQL Server 在训练用途上有 100,000 行的限制
- 不支持适用于 Visual Studio 2017 的 Microsoft SQL Server Data Tools

## <a name="install"></a>安装

可通过 Visual Studio Marketplace 或从 Visual Studio 中安装 ML.NET 模型生成器。

### <a name="visual-studio-marketplace"></a>Visual Studio Marketplace

1. 在 [Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=MLNET.07) 中下载
1. 按照提示安装到相应的 Visual Studio 版本上

### <a name="visual-studio-2017"></a>Visual Studio 2017

1. 从菜单栏选择“工具” > “扩展和更新”

    ![VS2017 打开“扩展管理器”对话框](./media/install-model-builder/vs2017-open-extensions-manager.png)

1. 在“扩展和更新”提示符中选择“联机”节点   。
1. 在搜索栏中搜索“ML.NET 模型生成器”，并在搜索结果中选择 ML.NET 模型搜索器（预览版） 

    ![VS2017 从“扩展管理器”对话框中搜索并安装“模型生成器”扩展](./media/install-model-builder/vs2017-install-model-builder.png)

1. 按照提示完成安装

### <a name="visual-studio-2019"></a>Visual Studio 2019

1. 从菜单栏选择“扩展” > “管理扩展”

    ![VS2019 打开“扩展管理器”对话框](./media/install-model-builder/vs2019-open-extensions-manager.png)

1. 在“扩展和更新”提示符中选择“联机”节点   。
1. 在搜索栏中键入“ML.NET 模型生成器”并选择 ML.NET 模型生成器（预览版） 

    ![VS2019 从“扩展管理器”对话框中搜索并安装“模型生成器”扩展](./media/install-model-builder/vs2019-install-model-builder.png)

1. 按照提示完成安装

## <a name="uninstall"></a>卸载

### <a name="visual-studio-2017"></a>Visual Studio 2017

1. 从菜单栏选择“工具” > “扩展和更新”

    ![VS2017 打开“管理扩展”对话框](./media/install-model-builder/vs2017-open-extensions-manager.png)

1. 在“扩展和更新”提示窗口中展开“已安装”节点并选择“工具”节点   
1. 从工具列表中选择 ML.NET 模型生成器（预览版），然后选择“卸载” 

    ![VS2017 从“扩展管理器”对话框中搜索并卸载“模型生成器”扩展](./media/install-model-builder/vs2017-uninstall-model-builder.png)

1. 按照提示完成卸载。

### <a name="visual-studio-2019"></a>Visual Studio 2019

1. 从菜单栏选择“扩展” > “管理扩展”

    ![VS2019 打开“管理扩展”对话框](./media/install-model-builder/vs2019-open-extensions-manager.png)

1. 在“扩展和更新”提示窗口中展开“已安装”节点并选择“工具”节点   
1. 从工具列表中选择 ML.NET 模型生成器（预览版），然后选择“卸载” 

    ![VS2019 从“扩展管理器”对话框中搜索并卸载“模型生成器”扩展](./media/install-model-builder/vs2019-uninstall-model-builder.png)

1. 按照提示完成卸载。

## <a name="upgrade"></a>Upgrade

升级流程与安装流程类似。 可以从 Visual Studio Marketplace 中下载最新版本，或者使用 Visual Studio 中的扩展管理器进行升级。
