---
title: 如何：使用 Windows Server App Fabric 承载工作流服务
ms.date: 03/30/2017
ms.assetid: 83b62cce-5fc2-4c6d-b27c-5742ba3bac73
ms.openlocfilehash: 2cf77753a0540e75ae6778065f7fa006729f8d6a
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90555980"
---
# <a name="how-to-host-a-workflow-service-with-windows-server-app-fabric"></a>如何：使用 Windows Server App Fabric 承载工作流服务

在 App Fabric 中承载工作流服务类似于在 IIS/WAS 下承载。 唯一的区别在于 App Fabric 提供的用于部署、监控和管理工作流服务的工具。 本主题使用在 [创建长时间运行的工作流服务](creating-a-long-running-workflow-service.md)中创建的工作流服务。 本主题将指导您创建工作流服务。 本主题将介绍如何使用 App Fabric 承载工作流服务。 有关 Windows Server App Fabric 的详细信息，请参阅 [Windows Server App Fabric 文档](/previous-versions/appfabric/ff384253(v=azure.10))。 完成以下步骤之前，请确保已安装 Windows Server App Fabric。  为此，请打开 Internet Information Services ( # A0) ，在 " **连接** " 视图中单击您的服务器名称，单击 "站点"，然后单击 " **默认**网站"。 在屏幕右侧，应会看到一个名为 " **App Fabric**" 的部分。 如果您看不到这一部分（它位于右侧窗格的顶部），则表示您还未安装 App Fabric。 有关安装 Windows Server App Fabric 的详细信息，请参阅 [安装 Windows Server App fabric](/previous-versions/appfabric/ee790960(v=azure.10))。  
  
### <a name="creating-a-simple-workflow-service"></a>创建简单工作流服务  
  
1. 打开 Visual Studio 2012 并加载在 [创建长时间运行的工作流服务](creating-a-long-running-workflow-service.md) 主题中创建的 OrderProcessing 解决方案。  
  
2. 右键单击 " **OrderService** " 项目并选择 " **属性** "，然后选择 " **Web** " 选项卡。  
  
3. 在 "属性" 页的 " **启动操作** " 部分中，选择 " **特定页面** "，然后在编辑框中键入 Service1。  
  
4. 在属性页的 " **服务器** " 部分中，选择 " **使用本地 IIS Web 服务器** "，并键入以下 URL： `http://localhost/OrderService` 。  
  
5. 单击 " **创建虚拟目录** " 按钮。 这将新建一个虚拟目录，并将项目设置为在生成项目时将所需文件复制到此虚拟目录。  或者，你也可以手动将 .xamlx、web.config 以及任何所需的 DLL 复制到此虚拟目录。  
  
### <a name="configuring-a-workflow-service-hosted-in-windows-server-app-fabric"></a>配置承载于 Windows Server App Fabric 中的工作流服务  
  
1. 打开 Internet 信息服务管理器 (inetmgr.exe)。  
  
2. 在 " **连接** " 窗格中导航到 OrderService 虚拟目录。  
  
3. 右键单击 OrderService，然后选择 " **管理 WCF 和 WF 服务**， **配置 ...**"。 将显示 " **为应用程序配置 WCF 和 WF** " 对话框。  
  
4. 选择 " **常规** " 选项卡以显示有关应用程序的一般信息，如以下屏幕截图所示。  
  
     ![“App Fabric 配置”对话框的“常规”选项卡](media/appfabricconfiguration-general.gif "AppFabricConfiguration-常规")  
  
5. 选择 " **监视** " 选项卡。这会显示各种监视设置，如以下屏幕截图所示。  
  
     ![“App Fabric 配置监视”选项卡](media/appfabricconfiguration-monitoring.gif "AppFabricConfiguration-监视")  
  
     有关在应用结构中配置工作流服务监视的详细信息，请参阅 [配置应用构造监视](/previous-versions/appfabric/ee677384(v=azure.10))。  
  
6. 选择 " **工作流持久性** " 选项卡。这使你可以将应用程序配置为使用 App Fabric 的默认永久性提供程序，如以下屏幕截图所示。  
  
     ![应用程序结构配置 &#45; 持久性](media/appfabricconfiguration-persistence.gif "AppFabricConfiguration-持久性")  
  
     有关在 Windows Server App Fabric 中配置工作流持久性的详细信息，请参阅 [在应用结构中配置工作流持久性](/previous-versions/appfabric/ee677353(v=azure.10))。  
  
7. 选择 " **工作流主机管理** " 选项卡。这样，你可以指定何时卸载和持久保存空闲工作流服务实例，如以下屏幕截图中所示。  
  
     ![应用结构配置工作流主机管理](media/appfabricconfiguration-management.gif "AppFabricConfiguration-管理")  
  
     有关工作流主机管理配置的详细信息，请参阅 [在应用结构中配置工作流主机管理](/previous-versions/appfabric/ff383424(v=azure.10))。  
  
8. 选择 " **自动启动** " 选项卡。这使你可以为应用程序中的工作流服务指定自动启动设置，如以下屏幕截图所示。  
  
     ![显示 App Fabric 自动&#45;启动配置的屏幕截图。](./media/how-to-host-a-workflow-service-with-windows-server-app-fabric/app-fabric-auto-start-configuration.gif)  
  
     有关配置自动启动的详细信息，请参阅 [通过 App Fabric 配置自动启动](/previous-versions/appfabric/ee677261(v=azure.10))。  
  
9. 选择 " **限制** " 选项卡。这允许你为工作流服务配置阻止设置，如以下屏幕截图所示。  
  
     ![显示 App Fabric 限制配置的屏幕截图。](./media/how-to-host-a-workflow-service-with-windows-server-app-fabric/app-fabric-throttling-configuration.gif)  
  
     有关配置限制的详细信息，请参阅 [配置应用程序结构的限制](/previous-versions/appfabric/ee677261(v=azure.10))。  
  
10. 选择 " **安全** " 选项卡。这允许你为应用程序配置安全设置，如以下屏幕截图所示。  
  
     ![App Fabric 安全配置](media/appfabricconfiguration-security.gif "AppFabricConfiguration-安全性")  
  
     有关配置 Windows Server App Fabric 安全性的详细信息，请参阅 [配置应用程序结构的安全性](/previous-versions/appfabric/ee677278(v=azure.10))。  
  
### <a name="using-windows-server-app-fabric"></a>使用 Windows Server App Fabric  
  
1. 生成解决方案以便将所需的文件复制到虚拟目录。  
  
2. 右键单击 "OrderClient" 项目，然后选择 " **调试**" 和 " **启动新实例** " 以启动客户端应用程序。  
  
3. 客户端将运行，Visual Studio 将显示 " **附加安全警告** " 对话框，单击 " **不附加** " 按钮。 这会告知 Visual Studio 不要附加到 IIS 进程进行调试。  
  
4. 客户端应用程序将立即调用工作流服务，然后等待。 工作流服务将转为空闲暂留状态。 通过启动 Internet 信息服务 (inetmgr.exe)，在“连接”窗格中导航至 OrderService 并将其选中，可以对此进行验证。 接下来，单击右侧窗格中的“App Fabric 仪表板”图标。 在持久化 WF 实例下，你会看到一个保留的工作流服务实例，如以下屏幕截图所示。  
  
     ![显示 App Fabric 仪表板的屏幕截图。](./media/how-to-host-a-workflow-service-with-windows-server-app-fabric/app-fabric-dashboard.gif)  
  
     **WF 实例历史记录**列出了有关工作流服务的信息，例如工作流服务激活次数、工作流服务实例完成的数目，以及有故障的工作流实例数。 在 "活动" 或 "空闲实例" 下，将显示一个链接，单击该链接将显示有关空闲工作流实例的详细信息，如以下屏幕截图中所示。  
  
     ![显示保存的工作流实例详细信息的屏幕截图。](./media/how-to-host-a-workflow-service-with-windows-server-app-fabric/persisted-workflow-instance-detail.gif)  
  
     有关 Windows Server App Fabric 功能以及如何使用它们的详细信息，请参阅 [Windows Server App Fabric 托管功能](/previous-versions/appfabric/ee677189(v=azure.10))  
  
## <a name="see-also"></a>请参阅

- [创建长时间运行的工作流服务](creating-a-long-running-workflow-service.md)
- [Windows Server App Fabric 承载功能](/previous-versions/appfabric/ee677189(v=azure.10))
- [安装 Windows Server App Fabric](/previous-versions/appfabric/ee790960(v=azure.10))
- [Windows Server App Fabric 文档](/previous-versions/appfabric/ff384253(v=azure.10))
