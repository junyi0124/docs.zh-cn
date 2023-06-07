---
title: 自定义跟踪
ms.date: 03/30/2017
ms.assetid: 2d191c9f-62f4-4c63-92dd-cda917fcf254
ms.openlocfilehash: 3d125c59196cb2a7d33961e0f271aab8c5663a50
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96234565"
---
# <a name="custom-tracking"></a>自定义跟踪

此示例演示如何创建自定义跟踪参与者并将跟踪数据的内容写入控制台。 另外，此示例还演示如何发出使用用户定义的数据填充的 <xref:System.Activities.Tracking.CustomTrackingRecord> 对象。 基于控制台的跟踪参与者将使用代码中创建的跟踪配置文件对象来筛选由工作流发出的 <xref:System.Activities.Tracking.TrackingRecord> 对象。

## <a name="sample-details"></a>示例详细信息

 Windows Workflow Foundation (WF) 提供一个跟踪基础结构，用于跟踪工作流实例的执行。 跟踪运行时实现一个工作流实例，以便发出与工作流生命周期有关的事件、来自工作流活动的事件和自定义跟踪事件。 下表详细介绍了跟踪基础结构的主要组件。

|组件|说明|
|---------------|-----------------|
|跟踪运行时|提供基础结构以发出跟踪记录。|
|跟踪参与者|使用跟踪记录。 .NET Framework 4 附带了一个跟踪参与者，该参与者将跟踪记录作为 Windows (ETW) 事件的事件跟踪来写入。|
|跟踪配置文件|筛选机制，允许跟踪参与者订阅从工作流实例发出的跟踪记录的子集。|

 下表详细介绍工作流运行时发出的跟踪记录。

|跟踪记录|描述|
|---------------------|-----------------|
|工作流实例跟踪记录。|描述工作流实例的生命周期。 例如，当工作流启动或完成时发出一个实例记录。|
|活动状态跟踪记录。|详细说明活动执行情况。 这些记录指示工作流活动的状态，例如，当安排活动时、活动完成时或引发错误时。|
|书签恢复记录。|当恢复工作流实例中的书签时发出。|
|自定义跟踪记录。|工作流作者可以在自定义活动中创建并发出自定义跟踪记录。|

 跟踪参与者使用跟踪配置文件来订阅已发出 <xref:System.Activities.Tracking.TrackingRecord> 对象的子集。 跟踪配置文件包含允许订阅特定跟踪记录类型的跟踪查询。 可以在代码或配置中指定跟踪配置文件。

### <a name="custom-tracking-participant"></a>自定义跟踪参与者

 跟踪参与者 API 允许以用户提供的跟踪参与者扩展跟踪运行时，该用户提供的跟踪参与者可包括用于对工作流运行时发出的 <xref:System.Activities.Tracking.TrackingRecord> 对象进行处理的自定义逻辑。

 若要编写跟踪参与者，用户必须实现 <xref:System.Activities.Tracking.TrackingParticipant>。 具体而言，<xref:System.Activities.Tracking.TrackingParticipant.Track%2A> 方法必须由自定义参与者实现。 此方法在工作流运行时发出 <xref:System.Activities.Tracking.TrackingRecord> 时调用。

```csharp
public abstract class TrackingParticipant
{
    protected TrackingParticipant();

    public virtual TrackingProfile TrackingProfile { get; set; }
    public abstract void Track(TrackingRecord record, TimeSpan timeout);
}
```

 完整的跟踪参与者是在 ConsoleTrackingParticipant.cs 文件中实现的。 下面的代码示例是 <xref:System.Activities.Tracking.TrackingParticipant.Track%2A> 自定义跟踪参与者的方法。

```csharp
protected override void Track(TrackingRecord record, TimeSpan timeout)
{
    ...
    WorkflowInstanceRecord workflowInstanceRecord = record as WorkflowInstanceRecord;
    if (workflowInstanceRecord != null)
    {
        Console.WriteLine(String.Format(CultureInfo.InvariantCulture,
            " Workflow InstanceID: {0} Workflow instance state: {1}",
            record.InstanceId, workflowInstanceRecord.State));
    }

    ActivityStateRecord activityStateRecord = record as ActivityStateRecord;
    if (activityStateRecord != null)
    {
        IDictionary<String, object> variables = activityStateRecord.Variables;
        StringBuilder vars = new StringBuilder();

        if (variables.Count > 0)
        {
            vars.AppendLine("\n\tVariables:");
            foreach (KeyValuePair<string, object> variable in variables)
            {
                vars.AppendLine(String.Format(
                    "\t\tName: {0} Value: {1}", variable.Key, variable.Value));
            }
        }
        Console.WriteLine(String.Format(CultureInfo.InvariantCulture,
            " :Activity DisplayName: {0} :ActivityInstanceState: {1} {2}",
                activityStateRecord.Activity.Name, activityStateRecord.State,
            ((variables.Count > 0) ? vars.ToString() : String.Empty)));
    }

    CustomTrackingRecord customTrackingRecord = record as CustomTrackingRecord;

    if ((customTrackingRecord != null) && (customTrackingRecord.Data.Count > 0))
    {
        ...
    }
    Console.WriteLine();

}
```

 下面的代码示例将向工作流调用程序添加控制台参与者。

```csharp
ConsoleTrackingParticipant customTrackingParticipant = new ConsoleTrackingParticipant()
{
    ...
    // The tracking profile is set here, refer to Program.CS
...
}

WorkflowInvoker invoker = new WorkflowInvoker(BuildSampleWorkflow());
invoker.Extensions.Add(customTrackingParticipant);
```

### <a name="emitting-custom-tracking-records"></a>发出自定义跟踪记录

 此示例还演示从自定义工作流活动发出 <xref:System.Activities.Tracking.CustomTrackingRecord> 对象的功能：

- 创建 <xref:System.Activities.Tracking.CustomTrackingRecord> 对象，并使用用户定义的数据（此数据必须与记录一起发出）对其进行填充。

- <xref:System.Activities.Tracking.CustomTrackingRecord>通过调用的 track 方法发出 <xref:System.Activities.ActivityContext> 。

 下面的示例演示如何发出自定义活动内的 <xref:System.Activities.Tracking.CustomTrackingRecord> 对象。

```csharp
// Create the Custom Tracking Record
CustomTrackingRecord customRecord = new CustomTrackingRecord("OrderIn")
{
    Data =
    {
        {"OrderId", 200},
        {"OrderDate", "20 Aug 2001"}
    }
};

// Emit custom tracking record
context.Track(customRecord);
```

#### <a name="to-use-this-sample"></a>使用此示例

1. 使用 Visual Studio 2010 打开 CustomTrackingSample 解决方案文件。

2. 要生成解决方案，按 Ctrl+Shift+B。

3. 若要运行解决方案，请按 Ctrl+F5。

> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> 如果此目录不存在，请参阅[Windows Communication Foundation (wcf) ，并 Windows Workflow Foundation (的 WF](https://www.microsoft.com/download/details.aspx?id=21459)) .NET Framework Windows Communication Foundation ([!INCLUDE[wf1](../../../../includes/wf1-md.md)] 此示例位于以下目录：  
>
> `<InstallDrive>:\WF_WCF_Samples\WF\Basic\Tracking\CustomTracking`  
  
## <a name="see-also"></a>另请参阅

- [AppFabric 监视示例](/previous-versions/appfabric/ff383407(v=azure.10))
