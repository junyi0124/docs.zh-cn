---
title: 在 Windows 事件跟踪中跟踪事件
ms.date: 03/30/2017
ms.assetid: f812659b-0943-45ff-9430-4defa733182b
ms.openlocfilehash: 4350287aedae73a7ca9556de7ae3f597950e32ea
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90549609"
---
# <a name="tracking-events-into-event-tracing-in-windows"></a>在 Windows 事件跟踪中跟踪事件

此示例演示如何在工作流服务上启用 Windows Workflow Foundation (WF) 跟踪，并在 Windows (ETW) 的事件跟踪中发出跟踪事件。 为了将工作流跟踪记录发到 ETW 中，该示例使用 ETW 跟踪参与者 (<xref:System.Activities.Tracking.EtwTrackingParticipant>)。

该示例中的工作流接收请求，将输入数据的倒数分配给输入变量，并将该倒数返回到客户端。 当输入数据为 0 时，会发生未处理的除数为零异常，从而导致工作流中止。 启用了跟踪后，将向 ETW 发出错误跟踪记录，以后可帮助对错误进行故障排除。 ETW 跟踪参与者通过跟踪配置文件配置为订阅跟踪记录。 跟踪配置文件在 Web.config 文件中定义，作为配置参数提供给 ETW 跟踪参与者。 ETW 跟踪参与者在工作流服务的 Web.config 文件中配置，作为服务行为应用于服务。 在此示例中，使用事件查看器来查看事件日志中的跟踪事件。

## <a name="workflow-tracking-details"></a>工作流跟踪详细信息

Windows Workflow Foundation 提供了跟踪基础结构，用于跟踪工作流实例的执行。 跟踪运行时会创建工作流实例以发出与工作流生命周期有关的事件、来自工作流活动的事件以及自定义事件。 下表详细介绍了跟踪基础结构的主要组件。

|组件|说明|
|---------------|-----------------|
|跟踪运行时|提供基础结构以发出跟踪记录。|
|跟踪参与者|访问跟踪记录。 [!INCLUDE[netfx_current_short](../../../../includes/netfx-current-short-md.md)] 附带了一个跟踪参与者，它作为 Windows 跟踪记录 (ETW) 事件写入跟踪记录。|
|跟踪配置文件|筛选机制，允许跟踪参与者订阅从工作流实例发出的跟踪记录的子集。|

下表详细介绍工作流运行时发出的跟踪记录。

|跟踪记录|说明|
|---------------------|-----------------|
|工作流实例跟踪记录。|描述工作流实例的生命周期。 例如，当工作流启动或完成时发出一个实例记录。|
|活动状态跟踪记录。|详细说明活动执行情况。 这些记录指示工作流活动的状态，例如，当安排活动时、活动完成时或引发错误时。|
|书签恢复记录。|当恢复工作流实例中的书签时发出。|
|自定义跟踪记录。|工作流作者可以在自定义活动中创建并发出自定义跟踪记录。|
|<xref:System.Activities.Tracking.ActivityScheduledRecord>|当某个活动安排其他活动时会发出此记录。|
|<xref:System.Activities.Tracking.FaultPropagationRecord>|当从某个活动传播错误时会发出此记录。|
|<xref:System.Activities.Tracking.CancelRequestedRecord>|当某个活动由其他活动取消时会发出此记录。|

跟踪参与者使用跟踪配置文件来订阅发出的跟踪记录的子集。 跟踪配置文件包含允许订阅特定跟踪记录类型的跟踪查询。 可以在代码或配置中指定跟踪配置文件。

#### <a name="to-use-this-sample"></a>使用此示例

1. 使用 Visual Studio 2010 打开 EtwTrackingParticipantSample 解决方案文件。

2. 要生成解决方案，按 Ctrl+Shift+B。

3. 若要运行解决方案，请按 F5。

    默认情况下，该服务正在侦听端口 53797 (`http://localhost:53797/SampleWorkflowService.xamlx`) 。

4. 使用文件资源管理器打开 WCF 测试客户端。

     ( # A0) 的 WCF 测试客户端位于 \<Visual Studio 2010 installation folder> \Common7\IDE\ 文件夹中。

    默认 Visual Studio 2010 安装文件夹为 C:\Program Files\Microsoft Visual Studio 10.0。

5. 在 WCF 测试客户端中，从 "**文件**" 菜单中选择 "**添加服务**"。

    在输入框中添加终结点地址。 默认值为 `http://localhost:53797/SampleWorkflowService.xamlx`。

6. 打开事件查看器应用程序。

    在调用服务之前事件查看器，从 " **开始** " 菜单中选择 " **运行** "，然后键入 `eventvwr.exe` 。 确保事件日志正在侦听从工作流服务发出的跟踪事件。

7. 在事件查看器的树视图中，导航到 " **事件查看器**"、" **应用程序和服务日志**" 和 " **Microsoft**"。 右键单击 " **Microsoft** " 并选择 "查看"，然后选择 " **查看** 和 **调试日志** " 以启用分析日志和调试日志

    确保选中 " **显示分析和调试日志** " 选项。

8. 在事件查看器中的树视图中，导航到 " **事件查看器**"、" **应用程序和服务日志**"、" **Microsoft**"、" **Windows**"、" **应用程序服务器**" 右键单击 " **分析** "，然后选择 " **启用日志** " 以启用 **分析** 日志。

9. 通过双击 `GetData`，使用 WCF 测试客户端测试服务。

    这会打开 `GetData` 方法。 请求接受一个参数，并确保值为 0（默认值）。

     单击 " **调用**"。

10. 观察从工作流发出的事件。

    切换回事件查看器，导航到 **事件查看器**、 **应用程序和服务日志**、 **Microsoft**、 **Windows**、 **应用程序服务器应用程序**。 右键单击 " **分析** "，然后选择 " **刷新**"。

    事件查看器中会显示工作流事件。 请注意，会显示工作流执行事件，并且其中一个事件是对应于工作流中的错误的未处理异常。 此外，还会从工作流活动发出一个警告事件，指示活动正引发错误。

11. 通过输入不为 0 的数据来重复步骤 9 和 10，以便不会引发错误。

通过跟踪配置文件，可以订阅运行时在工作流实例的状态发生更改时发出的事件。 根据监视需求，可以创建一个非常粗陋的配置文件，用于订阅对工作流进行的一小组高级状态更改。 另一方面，可以创建一个非常精确的配置文件，其输出足够丰富，可在以后重新构造执行。 该示例使用发出一小组事件的 `HealthMonitoring Tracking Profile`，演示从工作流运行时向 ETW 发出的事件。 Web.config 中还提供了另一个发出更多工作流跟踪事件的配置文件，该文件名为 `Troubleshooting Tracking Profile`。 安装 [!INCLUDE[netfx_current_short](../../../../includes/netfx-current-short-md.md)] 时，会在 Machine.config 文件中配置一个具有空名称的默认配置文件。 当未指定配置文件名称或指定了空配置文件名称时，此配置文件由 ETW 跟踪行为配置使用。

运行状况监视跟踪配置文件发出工作流实例记录和活动错误传播记录。 通过将以下跟踪配置文件添加到 Web.config 配置文件来创建此配置文件。

```xml
<tracking>
  <profiles>
    <trackingProfile name="HealthMonitoring Tracking Profile">
      <workflow activityDefinitionId="*">
        <workflowInstanceQueries>
          <workflowInstanceQuery>
            <states>
              <state name="Started"/>
              <state name="Completed"/>
              <state name="Aborted"/>
              <state name="UnhandledException"/>
            </states>
          </workflowInstanceQuery>
        </workflowInstanceQueries>
        <faultPropagationQueries>
          <faultPropagationQuery faultSourceActivityName ="*" faultHandlerActivityName="*"/>
        </faultPropagationQueries>
      </workflow>
    </trackingProfile>
  </profiles>
</tracking>
```

 可以通过将 `EtwTrackingParticipant` 配置更改为以下内容来更改该配置文件。

```xml
<behaviors>
  <serviceBehaviors>
    <behavior>
      <etwTracking profileName="HealthMonitoring Tracking Profile"/>
    </behavior>
  </serviceBehaviors>
</behaviors>
```

#### <a name="to-clean-up-optional"></a>清理（可选）

1. 打开事件查看器。

2. 导航到 **事件查看器**、 **应用程序和服务日志**、 **Microsoft**、 **Windows**、 **应用程序服务器应用程序**。 右键单击 " **分析** "，然后选择 " **禁用日志**"。

3. 导航到 **事件查看器**、 **应用程序和服务日志**、 **Microsoft**、 **Windows**、 **应用程序服务器应用程序**。 右键单击 " **分析** "，然后选择 " **清除日志**"。

4. 选择 " **清除** " 选项可清除事件。

## <a name="known-issue"></a>已知问题

> [!NOTE]
> 事件查看器中存在一个已知问题，即可能无法解码 ETW 事件。 您可能会看到与下面类似的错误消息。
>
> \<id>无法找到来自源 Microsoft-Windows 应用程序服务器-应用程序的事件 ID 的描述。 未在本地计算机上安装引发此事件的组件，或者安装已损坏。 可在本地计算机上安装或修复该组件。
>
> 如果遇到此错误，请在操作窗格中单击“刷新”。 事件现在应能正确解码。

> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> 如果此目录不存在，请参阅[Windows Communication Foundation (wcf) ，并 Windows Workflow Foundation (的 WF](https://www.microsoft.com/download/details.aspx?id=21459)) .NET Framework Windows Communication Foundation ([!INCLUDE[wf1](../../../../includes/wf1-md.md)] 此示例位于以下目录：
>
> `<InstallDrive>:\WF_WCF_Samples\WF\Basic\Tracking\EtwTracking`

## <a name="see-also"></a>请参阅

- [AppFabric 监视示例](/previous-versions/appfabric/ff383407(v=azure.10))
