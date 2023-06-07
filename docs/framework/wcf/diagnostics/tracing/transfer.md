---
title: 传输
ms.date: 03/30/2017
ms.assetid: dfcfa36c-d3bb-44b4-aa15-1c922c6f73e6
ms.openlocfilehash: da31dcb24234e750c88383b9f1bea4f088f4ee3d
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96293398"
---
# <a name="transfer"></a>传输

本主题介绍 Windows Communication Foundation (WCF) 活动跟踪模型中的传输。  
  
## <a name="transfer-definition"></a>传输定义  

 活动之间的传输表示终结点内相关活动中的事件之间的因果关系。 当两个活动之间存在控制流（例如方法调用跨越活动边界）时，这两个活动将与传输相关。 在 WCF 中，在服务上传入字节时，侦听活动会传输到接收字节活动，其中创建了消息对象。 有关端到端跟踪方案及其各自的活动和跟踪设计的列表，请参阅 [端到端跟踪方案](end-to-end-tracing-scenarios.md)。  
  
 若要发出传输跟踪，请在跟踪源中使用如下配置代码所示的 `ActivityTracing` 设置。  
  
```xml  
<source name="System.ServiceModel" switchValue="Verbose,ActivityTracing">  
```  
  
## <a name="using-transfer-to-correlate-activities-within-endpoints"></a>使用传输关联终结点内的活动  

 用户可以通过活动和传输找出错误的根本原因。 例如，如果分别在组件 M 和 N 的活动 M 和 N 之间来回传输，并且在传回 M 之后 N 中发生崩溃，则可以得出结论，发生错误很可能是由于 N 将数据传回了 M。  
  
 活动 M 和活动 N 之间存在控制流时，会从 M 向 N 发出传输跟踪。例如，由于存在跨越活动边界的方法调用，因而 N 会为 M 执行某些工作。 N 可能已存在或已创建。 N 由 M 生成，此时 N 是新活动，为 M 执行某些工作。  
  
 从 M 到 N 的传输之后可能不会紧跟一个从 N 到 M 的反向传输。这是因为 M 可能会在 N 中生成一些工作，并且不会跟踪何时 N 将完成这些工作。 实际上，M 可以在 N 完成其任务之前终止。 这种情况出现在 "打开 ServiceHost" 活动 (M) 中，该活动生成侦听器活动 (N) 然后终止。 从 N 传回 M 意味着 N 已完成与 M 相关的工作。  
  
 N 可以继续执行与 M 无关的其他处理，例如继续从不同登录活动接收登录请求 (M) 的现有身份验证器活动 (N)。  
  
 活动 M 与 N 之间并不一定存在嵌套关系。发生这种情况可能有两个原因。 首先，虽然活动 M 启动了 N，但 M 并不监视 N 中执行的实际处理。其次，N 可能已经存在。  
  
## <a name="example-of-transfers"></a>传输示例  

 下面列出了两个传输示例。  
  
- 在创建服务主机时，构造函数将从调用代码获得控制权，或者调用代码将控制权转移给构造函数。 构造函数执行完时，它将控制权返回给调用代码，或者构造函数将控制权转移回调用代码。 这是嵌套关系时的情况。  
  
- 侦听器开始处理传输数据时，它将创建一个新线程，并将相应的上下文传递到“接收字节”活动进行处理，并且传递控制权和数据。 该线程处理完请求后，接收字节活动不向侦听器传回任何内容。 在这种情况下，有传入的新线程活动而没有传出的新线程活动。 这两个活动相关，但不是嵌套关系。  
  
## <a name="activity-transfer-sequence"></a>活动传输序列  

 格式正确的活动传输序列包含下列步骤。  
  
1. 开始一个新活动（包括选择新的 gAId）。  
  
2. 发出从当前活动 ID 到该新 gAId 的传输跟踪  
  
3. 在 TLS 中设置新 ID  
  
4. 发出开始跟踪以指示新活动的开始。  
  
5. 返回到原始活动包括下列步骤：  
  
6. 发出到原始 gAId 的传输跟踪  
  
7. 发出停止跟踪以指示新活动的结束  
  
8. 将 TLS 设置为旧的 gAId。  
  
 下面的代码示例演示如何执行此操作。 此示例假定，在传输到新活动时将进行阻止调用，该调用包括挂起/继续跟踪。  
  
```csharp
// 0. Create a trace source  
TraceSource ts = new TraceSource("myTS");  

// 1. remember existing ("ambient") activity for clean up  
Guid oldGuid = Trace.CorrelationManager.ActivityId;  
// this will be our new activity  
Guid newGuid = Guid.NewGuid();

// 2. call transfer, indicating that we are switching to the new AID  
ts.TraceTransfer(667, "Transferring.", newGuid);  

// 3. Suspend the current activity.  
ts.TraceEvent(TraceEventType.Suspend, 667, "Suspend: Activity " + i-1);  

// 4. set the new AID in TLS  
Trace.CorrelationManager.ActivityId = newGuid;  

// 5. Emit the start trace  
ts.TraceEvent(TraceEventType.Start, 667, "Boundary: Activity " + i);  

// trace something  
ts.TraceEvent(TraceEventType.Information, 667, "Hello from activity " + i);  

// Perform Work  
// some work.  
// Return  
ts.TraceEvent(TraceEventType.Information, 667, "Work complete on activity " + i);

// 6. Emit the transfer returning to the original activity  
ts.TraceTransfer(667, "Transferring Back.", oldGuid);  

// 7. Emit the End trace  
ts.TraceEvent(TraceEventType.Stop, 667, "Boundary: Activity " + i);  

// 8. Change the tls variable to the original AID  
Trace.CorrelationManager.ActivityId = oldGuid;

// 9. Resume the old activity  
ts.TraceEvent(TraceEventType.Resume, 667, "Resume: Activity " + i-1);  
```  
  
## <a name="see-also"></a>另请参阅

- [配置跟踪](configuring-tracing.md)
- [使用服务跟踪查看器查看相关跟踪和进行故障诊断](using-service-trace-viewer-for-viewing-correlated-traces-and-troubleshooting.md)
- [端到端跟踪方案](end-to-end-tracing-scenarios.md)
- [服务跟踪查看器工具 (SvcTraceViewer.exe)](../../service-trace-viewer-tool-svctraceviewer-exe.md)
