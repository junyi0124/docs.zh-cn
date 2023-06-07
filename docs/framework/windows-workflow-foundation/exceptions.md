---
title: 异常
ms.date: 03/30/2017
ms.assetid: 065205cc-52dd-4f30-9578-b17d8d113136
ms.openlocfilehash: 53cc8e3e27e131c5c2a9f8271851a0d8d7e0cbd7
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96280203"
---
# <a name="exceptions"></a>例外

工作流可以使用 <xref:System.Activities.Statements.TryCatch> 活动处理工作流执行期间引发的异常。 可以对这些异常进行处理，或者使用 <xref:System.Activities.Statements.Rethrow> 活动重新引发异常。 <xref:System.Activities.Statements.TryCatch.Finally%2A> 节中的活动在 <xref:System.Activities.Statements.TryCatch.Try%2A> 节或 <xref:System.Activities.Statements.TryCatch.Catches%2A> 节完成时执行。 实例承载的工作流 <xref:System.Activities.WorkflowApplication> 还可以使用 <xref:System.Activities.WorkflowApplication.OnUnhandledException%2A> 事件处理程序处理未由活动处理的异常 <xref:System.Activities.Statements.TryCatch> 。  
  
## <a name="causes-of-exceptions"></a>异常的原因  

 在工作流中，异常可能通过下列方式生成：  
  
- <xref:System.Activities.Statements.TransactionScope> 中的事务超时。  
  
- 工作流通过使用 <xref:System.Activities.Statements.Throw> 活动引发的显式异常。  
  
- 某个活动引发的 [!INCLUDE[netfx_current_short](../../../includes/netfx-current-short-md.md)] 异常。  
  
- 外部代码引发的异常，例如工作流中使用的库、组件或服务。  
  
## <a name="handling-exceptions"></a>处理异常  

 如果异常由某个活动引发并且未经处理，则默认行为是终止该工作流实例。 如果存在自定义 <xref:System.Activities.WorkflowApplication.OnUnhandledException%2A> 处理程序，则它会重写此默认行为。 通过此处理程序，工作流宿主作者能够提供合适的处理操作，例如自定义日志记录、中止工作流、取消工作流或终止工作流。  如果工作流引发未处理的异常，则将调用 <xref:System.Activities.WorkflowApplication.OnUnhandledException%2A> 处理程序。 从 <xref:System.Activities.WorkflowApplication.OnUnhandledException%2A> 返回了三个可能的操作，确定工作流的最后结果。  
  
- **取消** -取消的工作流实例是分支执行的正常退出。 您可以对取消行为进行建模（例如，通过使用 CancellationScope 活动）。 取消进程完成时，将调用“已完成”处理程序。 已取消的工作流处于“已取消”状态。  
  
- **Terminate** -无法恢复或重启终止的工作流实例。  这会触发“已完成”事件，并且可以提供相应的异常来表明终止的原因。 终止进程完成时，将调用“已终止”处理程序。 终止的工作流处于“已出错”状态。  
  
- **中止** -仅当已中止的工作流实例已配置为持久性时，才能恢复该实例。  如果未配置持久性，则工作流无法继续执行。  在工作流中止的时间点上，自上次持久性点以来的所有已完成工作（内存中）都将丢失。 对于已中止的工作流，在中止进程结束时将调用“已中止”处理程序，并且使用相应的异常来表明中止的原因。 不过，与已取消和已终止不同，已中止的工作流不会调用“已完成”处理程序。 中止的工作流处于“已中止”状态。  
  
 下面的示例调用了引发异常的工作流。 工作流未处理异常，并且 <xref:System.Activities.WorkflowApplication.OnUnhandledException%2A> 处理程序已被调用。 将检查 <xref:System.Activities.WorkflowApplicationUnhandledExceptionEventArgs> 以提供有关异常的信息，且终止工作流。  
  
 [!code-csharp[CFX_WorkflowApplicationExample#1](~/samples/snippets/csharp/VS_Snippets_CFX/cfx_workflowapplicationexample/cs/program.cs#1)]  
  
### <a name="handling-exceptions-with-the-trycatch-activity"></a>使用 TryCatch 活动处理异常  

 在工作流内部处理异常是通过 <xref:System.Activities.Statements.TryCatch> 活动执行的。 <xref:System.Activities.Statements.TryCatch> 活动带有 <xref:System.Activities.Statements.TryCatch.Catches%2A> 活动的 <xref:System.Activities.Statements.Catch> 集合，其中每个活动都与一个特定的 <xref:System.Exception> 类型关联。 如果 <xref:System.Activities.Statements.TryCatch.Try%2A> 活动的 <xref:System.Activities.Statements.TryCatch> 节中包含的活动引发的异常与 <xref:System.Activities.Statements.Catch%601> 集合中 <xref:System.Activities.Statements.TryCatch.Catches%2A> 活动的异常匹配，则处理该异常。 如果此异常再次显式引发，或者引发了新异常，则此异常将向上传递到父活动。 下面的代码示例演示处理 <xref:System.Activities.Statements.TryCatch> 的 <xref:System.ApplicationException> 活动，该异常由 <xref:System.Activities.Statements.TryCatch.Try%2A> 活动在 <xref:System.Activities.Statements.Throw> 节中引发。 异常的消息由 <xref:System.Activities.Statements.Catch%601> 活动写入控制台，然后在 <xref:System.Activities.Statements.TryCatch.Finally%2A> 节中将一条消息写入控制台。  
  
 [!code-csharp[CFX_WorkflowApplicationExample#33](~/samples/snippets/csharp/VS_Snippets_CFX/cfx_workflowapplicationexample/cs/program.cs#33)]  
  
 <xref:System.Activities.Statements.TryCatch.Finally%2A> 节中的活动在 <xref:System.Activities.Statements.TryCatch.Try%2A> 节或 <xref:System.Activities.Statements.TryCatch.Catches%2A> 节成功完成时执行。 如果未从中引发任何异常，则 <xref:System.Activities.Statements.TryCatch.Try%2A> 节成功完成；如果未从中引发或重新引发任何异常，则 <xref:System.Activities.Statements.TryCatch.Catches%2A> 节成功完成。 如果在 <xref:System.Activities.Statements.TryCatch.Try%2A> 的 <xref:System.Activities.Statements.TryCatch> 节中引发了异常，并且未由 <xref:System.Activities.Statements.Catch%601> 节中的 <xref:System.Activities.Statements.TryCatch.Catches%2A> 处理或是从 <xref:System.Activities.Statements.TryCatch.Catches%2A> 重新引发，将不执行 <xref:System.Activities.Statements.TryCatch.Finally%2A> 中的活动，除非出现下列情况之一。  
  
- 无论异常是否由该更高级别的 <xref:System.Activities.Statements.TryCatch> 重新引发，均应由工作流中更高级别的 <xref:System.Activities.Statements.TryCatch> 活动捕获该异常。  
  
- 异常未由更高级别的 <xref:System.Activities.Statements.TryCatch> 进行处理，并且排除了工作流的根，此时工作流将配置为取消而不是终止或中止。 使用 <xref:System.Activities.WorkflowApplication> 承载的工作流可以通过处理 <xref:System.Activities.WorkflowApplication.OnUnhandledException%2A> 和返回 <xref:System.Activities.UnhandledExceptionAction.Cancel> 来对此进行配置。 在本主题的前面提供了处理 <xref:System.Activities.WorkflowApplication.OnUnhandledException%2A> 的示例。 工作流服务可以通过使用 <xref:System.ServiceModel.Activities.Description.WorkflowUnhandledExceptionBehavior> 并指定 <xref:System.ServiceModel.Activities.Description.WorkflowUnhandledExceptionAction.Cancel> 来对此进行配置。 有关配置的示例 <xref:System.ServiceModel.Activities.Description.WorkflowUnhandledExceptionBehavior> ，请参阅 [工作流服务主机扩展性](../wcf/feature-details/workflow-service-host-extensibility.md)。  
  
## <a name="exception-handling-versus-compensation"></a>异常处理与补偿  

 异常处理与补偿的不同之处在于：异常处理是在活动的执行期间发生的， 而补偿在活动成功完成后发生。 通过异常处理，可以在活动引发异常之后进行清理，而补偿提供了一种机制，可用于撤消以前完成的活动中所成功完成的工作。 有关详细信息，请参阅 [补偿](compensation.md)。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Activities.Statements.TryCatch>
- <xref:System.Activities.WorkflowApplication.OnUnhandledException%2A>
- <xref:System.Activities.Statements.CompensableActivity>
