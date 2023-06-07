---
title: 如何：使用 COM+ 服务模型配置工具
ms.date: 03/30/2017
helpviewer_keywords:
- COM+ [WCF], using service model configuration tool
ms.assetid: 7e68cd8d-5fda-4641-b92f-290db874376e
ms.openlocfilehash: 2d21ec8ccf84a7c9af235af8e0afd444044cf81b
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95729382"
---
# <a name="how-to-use-the-com-service-model-configuration-tool"></a>如何：使用 COM+ 服务模型配置工具

在选择了适当的宿主模式之后，就可使用 COM+ 服务模型配置命令行工具 (ComSvcConfig.exe) 来配置将作为 Web 服务公开的应用程序接口。

> [!NOTE]
> 你必须具有计算机上的管理员身份，才能执行下列各项任务。

 在 Windows 7 计算机上使用 ComSvcConfig.exe 配置 Web 服务以使用最新服务模型版本（当前版本为 v4.5）时，请执行以下步骤：

1. 将注册表项设置  `[HKEY_LOCAL_COMPUTER\SOFTWARE\Microsoft\.NETFramework]\OnlyUseLatestCLR` 为 DWORD 值0x00000001

2. 运行 comsvcconfig.exe

3. 将步骤 1 中的注册表项还原到其原始值，或者在不存在原始值时将该注册表项删除。

> [!IMPORTANT]
> 还原此注册表项十分重要。 这是一个兼容性注册表项。 不还原此更改可能会造成在计算机上运行的其他 .NET 应用程序产生问题）

> [!WARNING]
> 在 Windows 8 计算机上使用 ComSvcConfig.exe/install 时，会显示一个对话框，其中显示 "你的电脑上的应用需要以下 Windows 功能： .NET Framework 3.5 (包括 .NET 2.0 和 .NET 3.0" （如果未安装 .NET Framework 3.5）。 可忽略此对话框。 或者，可以将 OnlyUseLatestCLR 注册表项设置为 DWORD 值 0x00000001

## <a name="add-an-interface-using-the-com-hosting-mode"></a>使用 COM + 宿主模式添加接口

- 使用 `/install` 和 `/hosting:complus` 选项运行 ComSvcConfig，如下面的示例所示。

    ```console
    ComSvcConfig.exe /install /application:OnlineStore /contract:ItemOrders.Financial,IFinances /hosting:complus /verbose
    ```

     此命令将 `IFinances` 组件（它属于 OnlineStore COM+ 应用程序）的 `ItemOrders.IFinancial` 接口添加到将作为 Web 服务公开的接口集。 此服务使用 COM+ 宿主模式，因此要求显式激活应用程序。

     尽管通配符星号)  (\* 可用于组件和接口，但请避免使用它，因为你可能希望仅将选定的功能作为 Web 服务公开。 如果对此组件的将来版本运行命令，则使用通配符可能意外地公开在确定配置语法时尚不存在的接口。

     /verbose 选项指示该工具除显示所有错误以外，还要显示警告。

     所公开的服务的协定将包含 `IFinances` 接口中的所有方法。

## <a name="add-specific-methods-from-an-interface-using-the-com-hosting-mode"></a>使用 COM + 宿主模式从接口添加特定方法

- 使用 `/install` 和 `/hosting:complus` 选项以及所需方法的显式命名运行 ComSvcConfig，如下面的示例所示。

    ```console
    ComSvcConfig.exe /install /application:OnlineStore /contract:ItemOrders.Financial,IFinances.{Credit,Debit} /hosting:complus /verbose
    ```

     此命令仅将 `Credit` 接口中的 `Debit` 和 `IFinances` 方法作为操作添加到所公开的服务协定中。 此接口中的其他所有方法将在协定中省略，并且无法从 Web 服务客户端调用。

## <a name="add-an-interface-using-the-web-hosting-mode"></a>使用 Web 宿主模式添加接口

- 使用 `/install` 选项和 `/hosting:was` 选项运行 ComSvcConfig，如下面的示例所示。

    ```console
    ComSvcConfig.exe /install /application:OnlineWarehouse /contract:ItemInventory.Warehouse,IStockLevels /hosting:was /webDirectory:root/OnlineWarehouse /mex /verbose
    ```

     此命令将 `IStockLevels` 组件（它属于 OnlineWarehouse COM+ 应用程序）的 `ItemInventory.Warehouse` 接口添加到将作为 Web 服务公开的接口集。 此服务以 Web 方式承载在 IIS 的 OnlineWarehouse 虚拟目录中，而不是承载在 COM+ 中，因此应用程序将根据需要自动激活。

     若要使用 Web 承载的进程内配置，必须使用组件服务管理控制台将 COM+ 应用程序配置为作为库应用程序而不是服务器应用程序运行。 被配置为服务器应用程序的应用程序使用标准 Web 承载模式，并促使进程跃点处理每个请求。

     `/mex` 选项用于添加其他元数据交换 (MEX) 服务终结点，它们与应用程序的服务终结点使用相同的传输协议，以支持要从服务中检索协定定义的客户端。

## <a name="remove-a-web-service-for-a-specified-interface"></a>删除指定接口的 Web 服务

- 使用 `/uninstall` 选项运行 ComSvcConfig，如下面的示例所示。

    ```console
    ComSvcConfig.exe /uninstall /application:OnlineStore /contract:ItemOrders.Financial,IFinances /hosting:complus
    ```

     此命令将移除 `IFinances` 组件（它属于 OnlineStore COM+ 应用程序）上的 `ItemOrders.Financial` 接口。

## <a name="list-currently-exposed-interfaces"></a>列出当前公开的接口

- 使用 `/list` 选项运行 ComSvcConfig，如下面的示例所示。

    ```console
    ComSvcConfig.exe /list
    ```

     此命令列出当前公开的接口以及相应的地址和绑定详细信息，范围局限于本地计算机。

## <a name="list-specific-currently-exposed-interfaces"></a>列出特定的当前公开的接口

- 使用 `/list` 选项运行 ComSvcConfig，如下面的示例所示。

    ```console
    ComSvcConfig.exe /list /application:OnlineStore /hosting:complus
    ```

     此命令列出当前公开的 COM+ 承载接口以及相应的地址和绑定详细信息，范围局限于本地计算机上的 OnlineStore COM+ 应用程序。

## <a name="display-help-for-options"></a>显示选项的帮助

- 使用 /? 选项运行 ComSvcConfig， 如下面的示例所示。

    ```console
    ComSvcConfig.exe /?
    ```

## <a name="see-also"></a>另请参阅

- [与 COM + 应用程序集成概述](integrating-with-com-plus-applications-overview.md)
