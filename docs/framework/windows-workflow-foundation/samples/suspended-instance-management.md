---
title: 挂起的实例管理
ms.date: 03/30/2017
ms.assetid: f5ca3faa-ba1f-4857-b92c-d927e4b29598
ms.openlocfilehash: 620cd2299a3b08ee9330e13830c714c5d0ebb068
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96293645"
---
# <a name="suspended-instance-management"></a>挂起的实例管理

此示例演示如何管理已挂起的工作流实例。  <xref:System.ServiceModel.Activities.Description.WorkflowUnhandledExceptionBehavior> 的默认操作为 `AbandonAndSuspend`。 这意味着，在默认情况下，从 <xref:System.ServiceModel.WorkflowServiceHost> 中承载的某个工作流实例抛出的未处理异常会导致从内存中释放（放弃）该实例，而该实例的持久版本将被标记为已挂起。 已挂起的工作流实例在取消挂起之前无法运行。

 此示例演示如何实现用于查询已挂起实例的命令行实用工具，以及如何为用户提供恢复或终止实例的选项。 在此示例中，一个工作流服务有意引发异常，从而使该服务挂起。 然后可以使用命令行实用工具查询该实例，并随后恢复或终止该实例。

## <a name="demonstrates"></a>演示

 <xref:System.ServiceModel.WorkflowServiceHost><xref:System.ServiceModel.Activities.Description.WorkflowUnhandledExceptionBehavior> <xref:System.ServiceModel.Activities.WorkflowControlEndpoint> 在 WINDOWS WORKFLOW FOUNDATION (WF) 中。

## <a name="discussion"></a>讨论 (Discussion)

 本示例中实现的命令行实用工具特定于 [!INCLUDE[netfx_current_long](../../../../includes/netfx-current-long-md.md)] 中附带的 SQL 实例存储区实现。 如果您具有该实例存储区的自定义实现，则可以使用特定于您实例存储区的实现替换示例中的 `WorkflowInstanceCommand` 实现，来改编此实用工具。

 所提供的实现直接针对 SQL 实例存储区来运行 SQL 命令以列出挂起的实例，并依赖于添加到 <xref:System.ServiceModel.Activities.WorkflowControlEndpoint> 的 <xref:System.ServiceModel.WorkflowServiceHost>，以便恢复或终止实例。

#### <a name="to-set-up-build-and-run-the-sample"></a>设置、生成和运行示例

1. 此示例要求启用以下 Windows 组件：

    1. Microsoft Message Queues (MSMQ) Server

    2. SQL Server Express

2. 设置 SQL Server 数据库。

    1. 在 Visual Studio 2010 命令提示符下，运行 SuspendedInstanceManagement 示例目录中的 "setup .cmd"，此操作执行以下操作：

        1. 使用 SQL Server Express 创建持久性数据库。 如果持久性数据库已存在，则删除该数据库并重新创建一个

        2. 设置持久性数据库。

        3. 将 IIS APPPOOL\DefaultAppPool 和 NT AUTHORITY\Network Service 添加到设置持久性数据库时定义的 InstanceStoreUsers 角色。

3. 设置服务队列。

    1. 在 Visual Studio 2010 中，右键单击 **SampleWorkflowApp** 项目，然后单击 " **设为启动项目**"。

    2. 按 **F5** 编译并运行 SampleWorkflowApp。 这将创建所需队列。

    3. 按 **enter** 停止 SampleWorkflowApp。

    4. 通过在命令提示符下运行 Compmgmt.msc 来打开计算机管理控制台。

    5. 展开 " **服务和应用程序**"、" **消息队列**" 和 " **专用队列**"。

    6. 右键单击 " **ReceiveTx** " 队列，然后选择 " **属性**"。

    7. 选择 " **安全** " 选项卡，"允许 **每个人** " 具有 **接收消息**、 **扫视消息** 和 **发送消息** 的权限。

4. 现在运行示例。

    1. 在 Visual Studio 2010 中，按 **Ctrl + F5** 再次运行 SampleWorkflowApp 项目而不进行调试。 将在控制台窗口中输出两个终结点地址：一个用于应用程序终结点，另一个来自 <xref:System.ServiceModel.Activities.WorkflowControlEndpoint>。 随后会创建一个工作流实例，该实例的跟踪记录会出现在控制台窗口中。 该工作流实例会引发异常，从而导致该实例挂起或中止。

    2. 随后可以使用命令行实用工具对这些实例中的任何一个进行进一步操作。 命令行参数的语法如下：

         `SuspendedInstanceManagement -Command:[CommandName] -Server:[ServerName] -Database:[DatabaseName] -InstanceId:[InstanceId]`

         支持的命令为：`Query`、`Resume` 和 `Terminate`。  只有 `Resume` 和 `Terminate` 才需要 InstanceId 开关。

#### <a name="to-cleanup-optional"></a>清理（可选）

1. 通过在 `vs2010` 命令提示符下运行 Compmgmt.msc 来打开计算机管理控制台。

2. 展开 " **服务和应用程序**"、" **消息队列**" 和 " **专用队列**"。

3. 删除 **ReceiveTx** 队列。

4. 若要删除持久性数据库，请运行 cleanup.cmd。

> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> 如果此目录不存在，请参阅[Windows Communication Foundation (wcf) ，并 Windows Workflow Foundation (的 WF](https://www.microsoft.com/download/details.aspx?id=21459)) .NET Framework Windows Communication Foundation ([!INCLUDE[wf1](../../../../includes/wf1-md.md)] 此示例位于以下目录：  
>
> `<InstallDrive>:\WF_WCF_Samples\WF\Application\SuspendedInstanceManagement`
