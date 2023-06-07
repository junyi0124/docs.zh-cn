---
title: SQL 跟踪
ms.date: 03/30/2017
ms.assetid: bcaebeb1-b9e5-49e8-881b-e49af66fd341
ms.openlocfilehash: 916c04b03dee296b7e6f5c792f0c4e50fb4203c0
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90559345"
---
# <a name="sql-tracking"></a>SQL 跟踪

此示例演示如何编写一个自定义 SQL 跟踪参与者，该参与者将跟踪记录写入 SQL 数据库。 Windows Workflow Foundation (WF) 提供工作流跟踪，以查看工作流实例的执行情况。 跟踪运行时在工作流执行过程中会发出工作流跟踪记录。 有关工作流跟踪的详细信息，请参阅 [工作流跟踪和跟踪](../workflow-tracking-and-tracing.md)。

## <a name="use-the-sample"></a>使用示例

1. 确认您已安装 SQL Server 2008、 SQL Server 2008 Express 或更新版本。 与示例打包在一起的脚本假定在您的本地计算机上使用 SQL Express 实例。 如果您安装了不同的实例，请在运行此示例之前修改与数据库相关的脚本。

2. 通过在脚本目录 (\WF\Basic\Tracking\SqlTracking\CS\Scripts) 中运行 Trackingsetup.cmd 来创建 SQL Server 跟踪数据库。 这会创建一个名为 TrackingSample 的数据库。

   > [!NOTE]
   > 脚本将在 SQL Express 的默认实例上创建该数据库。 如果您想在不同的数据库实例上安装该数据库，请编辑 Trackingsetup.cmd 脚本。

3. 在 Visual Studio 2010 中打开 SqlTrackingSample。

4. 按**Ctrl** + **Shift** + **B**生成解决方案。

5. 按 **F5** 运行该应用程序。

   浏览器窗口打开和显示侦听应用程序的目录。

6. 在浏览器中，单击 StockPriceService.xamlx。

7. 浏览器显示 StockPriceService 页，其中包含本地服务 WSDL 地址。 复制此地址。

   本地服务 WSDL 地址的示例为 `http://localhost:65193/StockPriceService.xamlx?wsdl` 。

8. 使用文件资源管理器 ( # A0) 运行 WCF 测试客户端。 它位于 *Microsoft Visual Studio 10.0 \ Common7\IDE 目录*中。

9. 在 WCF 测试客户端中，单击 " **文件** " 菜单，然后选择 " **添加服务**"。 将本地服务地址粘贴到文本框中。 单击 **“确定”**，关闭对话框。

10. 在 WCF 测试客户端中，双击 " **GetStockPrice**"。 这会打开 `GetStockPrice` 采用一个参数的操作，键入值 `Contoso` 并单击 " **调用**"。

11. 发出的跟踪记录将写入一个 SQL 数据库中。 若要查看跟踪记录，请在 SQL Management Studio 中打开 TrackingSample 数据库，然后导航到表。 对表运行一个选择查询，将显示存储在相关表中的跟踪记录内的数据。

   有关 SQL Server Management Studio 的详细信息，请参阅 [SQL Server Management Studio 简介](/sql/ssms/sql-server-management-studio-ssms)。 [在此处](https://aka.ms/ssmsfullsetup)下载 SQL Server Management Studio。

## <a name="uninstall-the-sample"></a>卸载示例

1. 在示例目录中运行 theTrackingcleanup 脚本 (*\WF\Basic\Tracking\SqlTracking*) 。

    > [!NOTE]
    > Trackingcleanup.cmd 将尝试删除本地计算机 SQL Express 中的数据库。 如果您使用的是其他 SQL Server 实例，请编辑 Trackingcleanup.cmd。

> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> 如果此目录不存在，请参阅[Windows Communication Foundation (wcf) ，并 Windows Workflow Foundation (的 WF](https://www.microsoft.com/download/details.aspx?id=21459)) .NET Framework Windows Communication Foundation ([!INCLUDE[wf1](../../../../includes/wf1-md.md)] 此示例位于以下目录：
>
> `<InstallDrive>:\WF_WCF_Samples\WF\Basic\Tracking\SqlTracking`

## <a name="see-also"></a>请参阅

- [AppFabric 监视示例](/previous-versions/appfabric/ff383407(v=azure.10))
