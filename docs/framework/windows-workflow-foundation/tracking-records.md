---
title: 跟踪记录
ms.date: 03/30/2017
ms.assetid: 51adbda3-bd8b-4892-a8ea-d343186472d2
ms.openlocfilehash: 0344c802ef779d1f13f58c35c2f0e4fa67a37578
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96238101"
---
# <a name="tracking-records"></a>跟踪记录

检测工作流运行时的目的是为了发出跟踪记录，以便跟踪工作流实例的执行。  
  
## <a name="tracking-records"></a>跟踪记录  

 下表详细介绍工作流运行时发出的跟踪记录。  
  
|跟踪记录|描述|  
|---------------------|-----------------|  
|工作流生命周期记录|在工作流实例生命周期的各个阶段中发出。 例如，当工作流启动或完成时发出一个记录。|  
|活动生命周期记录|详细说明活动执行情况。 这些记录指示工作流活动的状态，例如，当安排活动时、活动完成时，或者发生错误时。|  
|书签恢复记录|当恢复工作流实例中的书签时发出。|  
|自定义跟踪记录|工作流作者可以在自定义活动中创建并发出自定义跟踪记录。|  
  
 WF 运行时发出的所有跟踪相关记录都派生自基类 <xref:System.Activities.Tracking.TrackingRecord>，该类包含一组常用的数据。 跟踪记录显示简单工作流的生命周期。 每条跟踪记录都包含有关关联跟踪事件的详细信息，例如，<xref:System.Activities.Tracking.TrackingRecord.InstanceId%2A>、<xref:System.Activities.Tracking.TrackingRecord.RecordNumber%2A> 以及特定于跟踪记录类型的其他信息。  
  
 工作流运行时发出下列 <xref:System.Activities.Tracking.TrackingRecord> 对象类型：  
  
- **WorkflowInstanceRecord** -此 <xref:System.Activities.Tracking.TrackingRecord> 描述工作流实例的生命周期。 例如，当工作流启动或完成时发出一个记录，该记录包含工作流实例的状态。 该记录的详细信息可以在 <xref:System.Activities.Tracking.WorkflowInstanceRecord> 中找到。  
  
- **WorkflowInstanceAbortedRecord** - <xref:System.Activities.Tracking.TrackingRecord> 在工作流实例中止时发出。 该记录包含中止工作流实例的原因。 该记录的详细信息可以在 <xref:System.Activities.Tracking.WorkflowInstanceAbortedRecord> 中找到。  
  
- **WorkflowInstanceUnhandledExceptionRecord** - <xref:System.Activities.Tracking.TrackingRecord> 如果工作流实例中发生异常，则会发出此操作，并且不会由任何活动处理。 该记录包含异常详细信息。 该记录的详细信息可以在 <xref:System.Activities.Tracking.WorkflowInstanceUnhandledExceptionRecord> 中找到。  
  
- **WorkflowInstanceSuspendedRecord** - <xref:System.Activities.Tracking.TrackingRecord> 每当挂起工作流实例时，都会发出此项。 该记录包含挂起工作流实例的原因。 该记录的详细信息可以在 <xref:System.Activities.Tracking.WorkflowInstanceSuspendedRecord> 中找到。  
  
- **WorkflowInstanceTerminatedRecord** - <xref:System.Activities.Tracking.TrackingRecord> 每当终止工作流实例时，都会发出此项。 该记录包含终止工作流实例的原因。 该记录的详细信息可以在 <xref:System.Activities.Tracking.WorkflowInstanceTerminatedRecord> 中找到。  
  
- **ActivityStateRecord** - <xref:System.Activities.Tracking.TrackingRecord> 当执行工作流中的活动时，将发出此事件。 这些记录指示工作流实例中的活动的状态。 该记录的详细信息可以在 <xref:System.Activities.Tracking.ActivityStateRecord> 中找到。  
  
- **ActivityScheduledRecord** - <xref:System.Activities.Tracking.TrackingRecord> 当活动计划子活动时，将发出此事件。 该记录包含父活动（安排活动）和安排执行的子活动的详细信息。 该记录的详细信息可以在 <xref:System.Activities.Tracking.ActivityScheduledRecord> 中找到。  
  
- **FaultPropagationRecord** -在 <xref:System.Activities.Tracking.TrackingRecord> 处理记录之前，会为每个处理程序发出此项。 它用于表示错误在工作流实例中的发生路径。 该记录的详细信息可以在 <xref:System.Activities.Tracking.FaultPropagationRecord> 中找到。  
  
- **CancelRequestedRecord** - <xref:System.Activities.Tracking.TrackingRecord> 每当活动尝试取消子活动时，都会发出此事件。 该记录包含父活动和取消的子活动的详细信息。 该记录的详细信息可以在 <xref:System.Activities.Tracking.CancelRequestedRecord> 中找到。  
  
- **BookmarkResumptionRecord** -此项 <xref:System.Activities.Tracking.TrackingRecord> 跟踪已成功恢复的任何书签。 该记录的详细信息可以在 <xref:System.Activities.Tracking.BookmarkResumptionRecord> 中找到。  
  
- **CustomTrackingRecord** - <xref:System.Activities.Tracking.TrackingRecord> 由工作流作者在自定义工作流活动中创建和发出。 自定义跟踪记录可以用数据填充，这些数据将随记录一起发出。 该记录的详细信息可以在 <xref:System.Activities.Tracking.CustomTrackingRecord> 中找到。  
  
 例如，可能存在一个简单的 <xref:System.Activities.Statements.Sequence> 活动，该活动包含 <xref:System.Activities.Statements.WriteLine> 操作且按以下顺序发出跟踪记录：  
  
1. <xref:System.Activities.Tracking.WorkflowInstanceRecord> 指示工作流正在启动。  
  
2. <xref:System.Activities.Tracking.ActivityScheduledRecord> 指示已安排某个活动。 在本例中，该活动为 <xref:System.Activities.Statements.Sequence> 活动。  
  
3. <xref:System.Activities.Tracking.ActivityScheduledRecord> 表示 <xref:System.Activities.Statements.WriteLine> 活动。  
  
4. 存在两个 <xref:System.Activities.Tracking.ActivityStateRecord> 记录，表示正在完成两个活动。  
  
5. <xref:System.Activities.Tracking.WorkflowInstanceRecord> 指示正在完成工作流。  
  
## <a name="see-also"></a>另请参阅

- [Windows Server App Fabric 监视](/previous-versions/appfabric/ee677251(v=azure.10))
- [用 App Fabric 监视应用程序](/previous-versions/appfabric/ee677276(v=azure.10))
