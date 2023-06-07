---
title: 跟踪配置文件
ms.date: 03/30/2017
ms.assetid: 22682566-1cd9-4672-9791-fb3523638e18
ms.openlocfilehash: ceeb0f5533bb4c637ea7df52249f5b00067d9b3d
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90551382"
---
# <a name="tracking-profiles"></a>跟踪配置文件

跟踪配置文件包含跟踪查询，这些查询允许跟踪参与者订阅当工作流实例的状态在运行时发生更改时发出的工作流事件。

## <a name="tracking-profiles"></a>跟踪配置文件

跟踪配置文件用来指定为工作流实例发出的跟踪信息。  如果未指定配置文件，则发出所有跟踪事件。 如果指定了配置文件，将发出在配置文件中指定的跟踪事件。 根据您的监视需求，可以编写一个非常一般的配置文件，用来订阅对工作流进行的一小组高级状态更改。 相反，也可以创建一个非常详细的配置文件，其生成的事件足够丰富，可在以后重新构造详细的执行流。

在标准 .NET Framework 配置文件中或在代码中指定，将配置文件清单本身作为 XML 元素进行跟踪。 下面的示例摘自配置文件中的 [!INCLUDE[netfx_current_long](../../../includes/netfx-current-long-md.md)]跟踪配置文件，跟踪参与者可利用它订阅 `Started` 和 `Completed` 工作流事件。

```xml
<system.serviceModel>
    ...
    <tracking>
     <profiles>
      <trackingProfile name="Sample Tracking Profile">
        <workflow activityDefinitionId="*">
          <workflowInstanceQueries>
            <workflowInstanceQuery>
              <states>
                <state name="Started"/>
                <state name="Completed"/>
              </states>
            </workflowInstanceQuery>
          </workflowInstanceQueries>
        </workflow>
      </trackingProfile>
    </profiles>
  </tracking>
    ...
</system.serviceModel>
```

下面的示例演示使用代码创建的等效跟踪配置文件。

```csharp
TrackingProfile profile = new TrackingProfile()
{
    Name = "CustomTrackingProfile",
    Queries =
    {
        new WorkflowInstanceQuery()
        {
            // Limit workflow instance tracking records for started and
            // completed workflow states.
            States = { WorkflowInstanceStates.Started, WorkflowInstanceStates.Completed },
        }
    }
};
```

利用 <xref:System.Activities.Tracking.ImplementationVisibility> 特性，可以通过跟踪配置文件中的可见性模式筛选跟踪记录。 复合活动是顶级活动，包含构成其实现的其他活动。 可见性模式指定工作流活动中的复合活动发出的跟踪记录，这些跟踪记录用于指定是否跟踪构成实现的活动。 可见性模式在跟踪配置文件级别应用。 对工作流中单个活动的跟踪记录的筛选由跟踪配置文件中的查询控制。 有关详细信息，请参阅本文档中的 **跟踪配置文件查询类型** 部分。

跟踪配置文件中的 `implementationVisibility` 特性指定的两种可见性模式包括 `RootScope` 和 `All`。 如果复合活动不是工作流的根，则使用 `RootScope` 模式会禁止构成活动实现的活动的跟踪记录。 这意味着，如果将使用其他活动实现的某一活动添加到工作流中，并将 `implementationVisibility` 设置为 RootScope，则仅跟踪该复合活动中的顶级活动。 如果活动是工作流的根，该活动的实现即为工作流自身，因此将发出构成实现的活动的跟踪记录。 使用 All 模式允许发出根活动及其所有复合活动的所有跟踪记录。

例如，假设 *MyActivity* 是一个复合活动，其实现包含两个活动： *Activity1* 和 *Activity2*。 如果将此活动添加到工作流，并使用设置为的跟踪配置文件启用跟踪 `implementationVisibility` `RootScope` ，则只会为 *MyActivity*发出跟踪记录。 但是，对于 activity *Activity1* 和 *Activity2*，不会发出任何记录。

但是，如果将 `implementationVisibility` 跟踪配置文件的属性设置为 `All` ，则会同时为 *MyActivity*和活动 *Activity1* 和 *Activity2*发出跟踪记录。

`implementationVisibility` 标志适用于以下跟踪记录类型：

- ActivityStateRecord

- FaultPropagationRecord

- CancelRequestedRecord

- ActivityScheduledRecord

> [!NOTE]
> implementationVisibility 设置无法筛选出活动实现发出的 CustomTrackingRecord。

在代码中，按如下所示在跟踪配置文件中将 `implementationVisibility` 功能指定为 <xref:System.Activities.Tracking.ImplementationVisibility.RootScope>：

```csharp
TrackingProfile sampleTrackingProfile = new TrackingProfile()
{
    Name = "Sample Tracking Profile",
    ImplementationVisibility = ImplementationVisibility.RootScope
};
```

在配置文件中，按如下所示在跟踪配置文件中将 `implementationVisibility` 功能指定为 <xref:System.Activities.Tracking.ImplementationVisibility.All>：

```xml
<tracking>
      <profiles>
        <trackingProfile name="Shipping Monitoring" implementationVisibility="All">
          <workflow activityDefinitionId="*">
...
         </workflow>
        </trackingProfile>
      </profiles>
</tracking>
```

跟踪配置文件中的 `ImplementationVisibility` 设置是可选的。 默认情况下，其值设置为 `RootScope`。 此外，该特性的值还区分大小写。

### <a name="tracking-profile-query-types"></a>跟踪配置文件查询类型

跟踪配置文件组织为跟踪记录的声明性订阅，利用这些订阅可以查询特定跟踪记录的工作流运行时。 查询类型有多种，可用于订阅 <xref:System.Activities.Tracking.TrackingRecord> 对象的不同类。 可以通过配置或代码指定跟踪配置文件。 以下是最常见的查询类型：

- <xref:System.Activities.Tracking.WorkflowInstanceQuery> - 用于跟踪工作流实例生命周期更改，例如，上面演示的 `Started` 和 `Completed`。 <xref:System.Activities.Tracking.WorkflowInstanceQuery> 用于订阅以下 <xref:System.Activities.Tracking.TrackingRecord> 对象：

  - <xref:System.Activities.Tracking.WorkflowInstanceRecord>

  - <xref:System.Activities.Tracking.WorkflowInstanceAbortedRecord>

  - <xref:System.Activities.Tracking.WorkflowInstanceUnhandledExceptionRecord>

  - <xref:System.Activities.Tracking.WorkflowInstanceTerminatedRecord>

  - <xref:System.Activities.Tracking.WorkflowInstanceSuspendedRecord>

  在 <xref:System.Activities.Tracking.WorkflowInstanceStates> 类中指定了可订阅的状态。

  下面的示例演示使用 `Started` 订阅 <xref:System.Activities.Tracking.WorkflowInstanceQuery> 实例状态的工作流实例级跟踪记录所使用的配置或代码。

  ```xml
  <workflowInstanceQueries>
      <workflowInstanceQuery>
        <states>
          <state name="Started"/>
        </states>
      </workflowInstanceQuery>
  </workflowInstanceQueries>
  ```

  ```csharp
  TrackingProfile sampleTrackingProfile = new TrackingProfile()
  {
      Name = "Sample Tracking Profile",
      Queries =
      {
          new WorkflowInstanceQuery()
          {
              States = { WorkflowInstanceStates.Started}
          }
      }
  };
  ```

- <xref:System.Activities.Tracking.ActivityStateQuery> - 用于跟踪组成工作流实例的活动的生命周期更改。 例如，你可能想要跟踪每次在工作流实例中完成 "发送电子邮件" 活动的时间。 <xref:System.Activities.Tracking.TrackingParticipant> 需要用该查询来订阅 <xref:System.Activities.Tracking.ActivityStateRecord> 对象。 在 <xref:System.Activities.Tracking.ActivityStates> 中指定了要订阅的可用状态。

  下面的示例演示使用 <xref:System.Activities.Tracking.ActivityStateQuery> 订阅 `SendEmailActivity` 活动的活动状态跟踪记录所使用的配置和代码。

  ```xml
  <activityStateQueries>
    <activityStateQuery activityName="SendEmailActivity">
      <states>
        <state name="Closed"/>
      </states>
    </activityStateQuery>
  </activityStateQueries>
  ```

  ```csharp
  TrackingProfile sampleTrackingProfile = new TrackingProfile()
  {
      Name = "Sample Tracking Profile",
      Queries =
      {
          new ActivityStateQuery()
          {
              ActivityName = "SendEmailActivity",
              States = { ActivityStates.Closed }
          }
      }
  };
  ```

  > [!NOTE]
  > 如果多个 activityStateQuery 元素具有相同名称，则只有最后一个元素中的状态用于跟踪配置文件。

- <xref:System.Activities.Tracking.ActivityScheduledQuery> - 该查询用于跟踪安排给父活动来执行的活动。 <xref:System.Activities.Tracking.TrackingParticipant> 需要用该查询来订阅 <xref:System.Activities.Tracking.ActivityScheduledRecord> 对象。

  下面的示例演示使用 `SendEmailActivity` 订阅与安排执行的 <xref:System.Activities.Tracking.ActivityScheduledQuery> 子活动相关的记录所使用的配置和代码。

  ```xml
  <activityScheduledQueries>
    <activityScheduledQuery activityName="ProcessNotificationsActivity" childActivityName="SendEmailActivity" />
  </activityScheduledQueries>
  ```

  ```csharp
  TrackingProfile sampleTrackingProfile = new TrackingProfile()
  {
      Name = "Sample Tracking Profile",
      Queries =
      {
          new ActivityScheduledQuery()
          {
              ActivityName = "ProcessNotificationsActivity",
              ChildActivityName = "SendEmailActivity"
          }
      }
  };
  ```

- <xref:System.Activities.Tracking.FaultPropagationQuery> - 用于跟踪对在活动中出现的错误进行的处理。 <xref:System.Activities.Tracking.TrackingParticipant> 需要用该查询来订阅 <xref:System.Activities.Tracking.FaultPropagationRecord> 对象。

  下面的示例演示使用 <xref:System.Activities.Tracking.FaultPropagationQuery> 订阅与错误传播相关的记录所使用的配置和代码。

  ```xml
  <faultPropagationQueries>
    <faultPropagationQuery faultSourceActivityName="SendEmailActivity" faultHandlerActivityName="NotificationsFaultHandler" />
  </faultPropagationQueries>
  ```

  ```csharp
  TrackingProfile sampleTrackingProfile = new TrackingProfile()
  {
      Name = "Sample Tracking Profile",
      Queries =
      {
          new FaultPropagationQuery()
          {
              FaultSourceActivityName = "SendEmailActivity",
              FaultHandlerActivityName = "NotificationsFaultHandler"
          }
      }
  };
  ```

- <xref:System.Activities.Tracking.CancelRequestedQuery> - 用于跟踪父活动取消子活动的请求。 <xref:System.Activities.Tracking.TrackingParticipant> 需要用该查询来订阅 <xref:System.Activities.Tracking.CancelRequestedRecord> 对象。

  下面的示例演示了使用订阅与活动取消相关的记录所使用的配置和代码 <xref:System.Activities.Tracking.CancelRequestedQuery> 。

  ```xml
  <cancelRequestedQueries>
    <cancelRequestedQuery activityName="ProcessNotificationsActivity" childActivityName="SendEmailActivity" />
  </cancelRequestedQueries>
  ```

  ```csharp
  TrackingProfile sampleTrackingProfile = new TrackingProfile()
  {
      Name = "Sample Tracking Profile",
      Queries =
      {
          new CancelRequestedQuery()
          {
              ActivityName = "ProcessNotificationsActivity",
              ChildActivityName = "SendEmailActivity"
          }
      }
  };
  ```

- <xref:System.Activities.Tracking.CustomTrackingQuery> - 用于跟踪您在代码活动中定义的事件。 <xref:System.Activities.Tracking.TrackingParticipant> 需要用该查询来订阅 <xref:System.Activities.Tracking.CustomTrackingRecord> 对象。

  下面的示例演示使用 <xref:System.Activities.Tracking.CustomTrackingQuery> 订阅与自定义跟踪记录相关的记录所使用的配置和代码。

  ```xml
  <customTrackingQueries>
    <customTrackingQuery name="EmailAddress" activityName="SendEmailActivity" />
  </customTrackingQueries>
  ```

  ```csharp
  TrackingProfile sampleTrackingProfile = new TrackingProfile()
  {
      Name = "Sample Tracking Profile",
      Queries =
      {
          new CustomTrackingQuery()
          {
              Name = "EmailAddress",
              ActivityName = "SendEmailActivity"
          }
      }
  };
  ```

- <xref:System.Activities.Tracking.BookmarkResumptionQuery> - 用于跟踪工作流实例中的书签恢复。 <xref:System.Activities.Tracking.TrackingParticipant> 需要用该查询来订阅 <xref:System.Activities.Tracking.BookmarkResumptionRecord> 对象。

  下面的示例演示使用 <xref:System.Activities.Tracking.BookmarkResumptionQuery> 订阅与书签恢复相关的记录所使用的配置和代码。

  ```xml
  <bookmarkResumptionQueries>
    <bookmarkResumptionQuery name="SentEmailBookmark" />
  </bookmarkResumptionQueries>
  ```

  ```csharp
  TrackingProfile sampleTrackingProfile = new TrackingProfile()
  {
      Name = "Sample Tracking Profile",
      Queries =
      {
          new BookmarkResumptionQuery()
          {
              Name = "sentEmailBookmark"
          }
      }
  };
  ```

### <a name="annotations"></a>批注

通过批注，可以使用可在生成时后配置的某个值任意标记跟踪记录。 例如，您可能希望多个工作流的多个跟踪记录都用 "Mail Server" = = "Mail Server1" 标记。 这样便于在以后查询跟踪记录时查找带有此标记的所有记录。

为此，可以在跟踪查询中添加一个批注，如下面的示例所示。

```xml
<activityStateQuery activityName="SendEmailActivity">
  <states>
    <state name="Closed"/>
  </states>
  <annotations>
    <annotation name="MailServer" value="Mail Server1"/>
  </annotations>
</activityStateQuery>
```

### <a name="how-to-create-a-tracking-profile"></a>如何创建跟踪配置文件

跟踪查询元素用于通过 XML 配置文件或代码创建跟踪配置文件 [!INCLUDE[netfx_current_long](../../../includes/netfx-current-long-md.md)] 。 以下是使用配置文件创建跟踪配置文件的示例。

```xml
<system.serviceModel>
  <tracking>
    <profiles>
      <trackingProfile name="Sample Tracking Profile ">
        <workflow activityDefinitionId="*">
          <!--Specify the tracking profile query elements to subscribe for tracking records-->
        </workflow>
      </trackingProfile>
    </profiles>
  </tracking>
</system.serviceModel>
```

> [!WARNING]
> 对于使用工作流服务主机的 WF，跟踪配置文件通常是使用配置文件创建的。 也可以通过代码使用跟踪配置文件和跟踪查询 API 来创建跟踪配置文件。

配置为 XML 配置文件的配置文件适用于使用行为扩展的跟踪参与者。 这将添加到 WorkflowServiceHost，如后面的为 [工作流配置跟踪](configuring-tracking-for-a-workflow.md)中所述。

主机发出的跟踪记录的详细信息由跟踪配置文件中的配置设置确定。 跟踪参与者通过向跟踪配置文件添加查询来订阅跟踪记录。 若要订阅所有跟踪记录，跟踪配置文件需要 \* 在每个查询的名称字段中使用 "" 指定所有跟踪查询。

下面是跟踪配置文件的一些常见示例。

- 用于获取工作流实例记录和错误的跟踪配置文件。

  ```xml
  <trackingProfile name="Instance and Fault Records">
    <workflow activityDefinitionId="*">
      <workflowInstanceQueries>
        <workflowInstanceQuery>
          <states>
            <state name="*" />
          </states>
        </workflowInstanceQuery>
      </workflowInstanceQueries>
      <activityStateQueries>
        <activityStateQuery activityName="*">
          <states>
            <state name="Faulted"/>
          </states>
        </activityStateQuery>
      </activityStateQueries>
    </workflow>
  </trackingProfile>
  ```

- 用于获取所有自定义跟踪记录的跟踪配置文件。

  ```xml
  <trackingProfile name="Instance_And_Custom_Records">
    <workflow activityDefinitionId="*">
      <customTrackingQueries>
        <customTrackingQuery name="*" activityName="*" />
      </customTrackingQueries>
    </workflow>
  </trackingProfile>
  ```

## <a name="see-also"></a>请参阅

- [SQL 跟踪](./samples/sql-tracking.md)
- [Windows Server App Fabric 监视](/previous-versions/appfabric/ee677251(v=azure.10))
- [用 App Fabric 监视应用程序](/previous-versions/appfabric/ee677276(v=azure.10))
