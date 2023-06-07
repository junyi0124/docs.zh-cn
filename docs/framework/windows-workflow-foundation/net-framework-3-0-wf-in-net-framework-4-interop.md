---
title: 在 .NET Framework 4 中将 .NET Framework 3.0 WF 活动与 Interop 活动一起使用
ms.date: 03/30/2017
ms.assetid: 71f112ba-abb0-46f7-b05f-a5d2eb9d0c5c
ms.openlocfilehash: 69b3a49f96de4f955b16225864be3ee46b168356
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96268620"
---
# <a name="using-net-framework-30-wf-activities-in-net-framework-4-with-the-interop-activity"></a>在 .NET Framework 4 中将 .NET Framework 3.0 WF 活动与 Interop 活动一起使用

<xref:System.Activities.Statements.Interop>活动是一个 [!INCLUDE[netfx_current_short](../../../includes/netfx-current-short-md.md)] (wf 4.5) 活动，该活动在工作流中包装 .NET Framework 3.5 (WF 3.5) 活动 [!INCLUDE[netfx_current_short](../../../includes/netfx-current-short-md.md)] 。 WF 3 活动可以是单叶活动，也可以是整个活动树。 执行 (包括取消和异常处理) 并且 .NET Framework 3.5 活动的持久性发生在正在 [!INCLUDE[netfx_current_short](../../../includes/netfx-current-short-md.md)] 执行的工作流实例的上下文中。  
  
> [!NOTE]
> <xref:System.Activities.Statements.Interop>除非工作流的项目的 "**目标框架**" 设置设为 **.NET Framework 4.5**，否则活动不会显示在工作流设计器工具箱中。  
  
## <a name="criteria-for-using-a-wf-3-activity-with-an-interop-activity"></a>将 WF 3 活动与 Interop 活动一起使用的条件  

 若要使 WF 3 活动在 <xref:System.Activities.Statements.Interop> 活动中成功执行，必须满足以下条件：  
  
- WF 3 活动必须派生自 <xref:System.Workflow.ComponentModel.Activity?displayProperty=nameWithType>。  
  
- 必须将 WF 3 活动声明为 `public`，不能为 `abstract`。  
  
- WF 3 活动必须具有公共的无参数构造函数。  
  
- 由于 <xref:System.Activities.Statements.Interop> 活动可以支持的接口类型的限制，因此无法直接使用 <xref:System.Workflow.Activities.HandleExternalEventActivity> 和 <xref:System.Workflow.Activities.CallExternalMethodActivity>，但是可以使用通过工作流通信活动工具 (WCA.exe) 创建的派生活动。 有关详细信息，请参阅 [Windows Workflow Foundation 工具](/previous-versions/dotnet/netframework-3.5/ms734408(v=vs.90)) 。  
  
## <a name="configuring-a-wf-3-activity-within-an-interop-activity"></a>在 Interop 活动中配置 WF 3 活动  

 若要跨互操作边界配置 WF 3 活动并将数据传入和传出该活动，则 <xref:System.Activities.Statements.Interop> 活动应公开 WF 3 活动的属性和元数据属性。 WF 3 活动的元数据属性（如 <xref:System.Workflow.ComponentModel.Activity.Name%2A>）通过 <xref:System.Activities.Statements.Interop.ActivityMetaProperties%2A> 集合公开。 这是一个名称/值对的集合，用于为 WF 3 活动的元数据属性定义值。 元数据属性是由设置了 <xref:System.Workflow.ComponentModel.DependencyPropertyOptions.Metadata> 标志的依赖项属性支持的属性。  
  
 WF 3 活动的属性通过 <xref:System.Activities.Statements.Interop.ActivityProperties%2A> 集合公开。 这是一个名称/值对的集合，其中每个值都是一个 <xref:System.Activities.Argument> 对象，用于为 WF 3 活动的属性定义参数。 由于无法推断 WF 3 活动属性的方向，因此每个属性都将呈现为一 <xref:System.Activities.InArgument> / <xref:System.Activities.OutArgument> 对。 根据活动属性的用法的不同，您可能希望提供一个 <xref:System.Activities.InArgument> 项和/或一个 <xref:System.Activities.OutArgument> 项。 集合中 <xref:System.Activities.InArgument> 项的期望名称为在 WF 3 活动上定义的属性的名称。 集合中的项的预期名称 <xref:System.Activities.OutArgument> 是属性名称和字符串 "Out" 的串联。  
  
## <a name="limitations-of-using-a-wf-3-activity-within-an-interop-activity"></a>在 Interop 活动中使用 WF 3 活动的限制  

 不能将 WF 3 系统提供的活动直接包装在 <xref:System.Activities.Statements.Interop> 活动中。 对于某些 WF 3 活动，如 <xref:System.Workflow.Activities.DelayActivity>，这是因为存在类似的 WF 4.5 活动。 对于其他活动，这是因为不支持活动的这个功能。 许多 WF 3 系统提供的活动可在由 <xref:System.Activities.Statements.Interop> 活动包装的工作流中使用，但受到以下限制：  
  
1. 不能在 <xref:System.ServiceModel.Activities.Send> 活动中使用 <xref:System.ServiceModel.Activities.Receive> 和 <xref:System.Activities.Statements.Interop>。  
  
2. 不能在 <xref:System.Workflow.Activities.WebServiceInputActivity> 活动中使用 <xref:System.Workflow.Activities.WebServiceOutputActivity>、<xref:System.Workflow.Activities.WebServiceFaultActivity> 和 <xref:System.Activities.Statements.Interop>。  
  
3. 不能在 <xref:System.Workflow.Activities.InvokeWorkflowActivity> 活动中使用 <xref:System.Activities.Statements.Interop>。  
  
4. 不能在 <xref:System.Workflow.ComponentModel.SuspendActivity> 活动中使用 <xref:System.Activities.Statements.Interop>。  
  
5. 不能在 <xref:System.Activities.Statements.Interop> 活动中使用与补偿相关的活动。  
  
 还有一些行为具体信息，可了解有关在 <xref:System.Activities.Statements.Interop> 活动中使用 WF 3 活动的信息：  
  
1. 执行 <xref:System.Activities.Statements.Interop> 活动时会对 <xref:System.Activities.Statements.Interop> 活动中包含的 WF 3 活动进行初始化。 在 WF 4.5 中，执行之前工作流实例没有初始化阶段。  
  
2. 事务开始后，WF 4.5 运行时没有检查点工作流实例状态，无论事务开始的位置如何（在 <xref:System.Activities.Statements.Interop> 活动内部还是外部）。  
  
3. 将  <xref:System.Activities.Statements.Interop> 活动中活动的 WF 3 跟踪记录以 <xref:System.Activities.Tracking.InteropTrackingRecord> 对象的形式提供给 WF 4.5 跟踪参与者。 <xref:System.Activities.Tracking.InteropTrackingRecord> 是从 <xref:System.Activities.Tracking.CustomTrackingRecord> 派生的。  
  
4. WF 3 自定义活动可以在互操作环境中使用工作流队列访问数据，其访问方式与在 WF 3 工作流运行时中完全相同。 不需要更改任何自定义活动代码。 在宿主上，通过恢复 <xref:System.Activities.Bookmark> 可将数据排入 WF 3 工作流队列。 书签的名称是字符串形式的 <xref:System.IComparable> 工作流队列名称。
