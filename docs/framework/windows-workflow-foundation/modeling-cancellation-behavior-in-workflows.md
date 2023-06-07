---
title: 为工作流中的取消行为进行建模
ms.date: 03/30/2017
ms.assetid: d48f6cf3-cdde-4dd3-8265-a665acf32a03
ms.openlocfilehash: 157bff0d24b00f293fd551dd52fe693d36dfad61
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96242053"
---
# <a name="modeling-cancellation-behavior-in-workflows"></a>为工作流中的取消行为进行建模

可以在工作流内部通过一个 <xref:System.Activities.Statements.Parallel> 活动（此活动在其 <xref:System.Activities.Statements.Parallel.CompletionCondition%2A> 的计算结果为 `true` 时将取消不完整的分支）取消活动，也可以从工作流外部取消活动（如果主机调用 <xref:System.Activities.WorkflowApplication.Cancel%2A>）。 若要提供取消处理，工作流作者可以使用 <xref:System.Activities.Statements.CancellationScope> 活动和 <xref:System.Activities.Statements.CompensableActivity> 活动，也可以创建提供取消逻辑的自定义活动。 本主题概述了工作流中的取消。  
  
## <a name="cancellation-compensation-and-transactions"></a>取消、补偿和事务  

 利用事务，应用程序可以在事务进程的任何部分中出现错误时中止（回滚）事务内执行的所有更改。 但并非所有可能需要取消或撤销的工作都适用于事务，如长时间运行的工作或未涉及事务性资源的工作。 如果工作流中发生后续失败，补偿将提供用于撤销之前完成的非事务性工作的模型。 取消将为工作流作者和活动作者提供用于处理未完成的非事务性工作的模型。 如果某个活动的执行过程尚未完成就被取消，则将调用此活动的取消逻辑（如果可用）。  
  
> [!NOTE]
> 有关事务和补偿的详细信息，请参阅 [事务](workflow-transactions.md) 和 [补偿](compensation.md)。  
  
## <a name="using-cancellationscope"></a>使用 CancellationScope  

 <xref:System.Activities.Statements.CancellationScope> 活动包含两个部分，这两个部分可包含的子活动为 <xref:System.Activities.Statements.CancellationScope.Body%2A> 和 <xref:System.Activities.Statements.CancellationScope.CancellationHandler%2A>。 <xref:System.Activities.Statements.CancellationScope.Body%2A> 用于放置构成活动逻辑的活动，<xref:System.Activities.Statements.CancellationScope.CancellationHandler%2A> 用于放置提供活动的取消逻辑的活动。 只能在某个活动尚未完成时取消此活动。 对于 <xref:System.Activities.Statements.CancellationScope> 活动，完成是指完成 <xref:System.Activities.Statements.CancellationScope.Body%2A> 中的活动。 如果安排了取消请求且 <xref:System.Activities.Statements.CancellationScope.Body%2A> 中的活动尚未完成，则将 <xref:System.Activities.Statements.CancellationScope> 标记为 <xref:System.Activities.ActivityInstanceState.Canceled>，并执行 <xref:System.Activities.Statements.CancellationScope.CancellationHandler%2A> 活动。  
  
### <a name="canceling-a-workflow-from-the-host"></a>从主机取消工作流  

 主机可通过调用正在承载某个工作流的 <xref:System.Activities.WorkflowApplication.Cancel%2A> 实例的 <xref:System.Activities.WorkflowApplication> 方法来取消该工作流。 下面的示例创建一个具有 <xref:System.Activities.Statements.CancellationScope> 的工作流。 调用该工作流后，主机将调用 <xref:System.Activities.WorkflowApplication.Cancel%2A>。 停止工作流的主执行，并调用 <xref:System.Activities.Statements.CancellationScope.CancellationHandler%2A> 的 <xref:System.Activities.Statements.CancellationScope>，然后完成工作流，其状态为 <xref:System.Activities.ActivityInstanceState.Canceled>。  
  
 [!code-csharp[CFX_WorkflowApplicationExample#35](~/samples/snippets/csharp/VS_Snippets_CFX/cfx_workflowapplicationexample/cs/program.cs#35)]  
  
 调用该工作流时，会将以下输出显示到控制台。  
  
 **启动工作流。**  
已 **调用 CancellationHandler。** 
**Workflow B30ebb30-df46-4d90-a211-e31c38d8db3c 已取消。**
> [!NOTE]
> 在取消 <xref:System.Activities.Statements.CancellationScope> 活动并调用 <xref:System.Activities.Statements.CancellationScope.CancellationHandler%2A> 时，工作流作者应负责确定活动在完全取消之前的取消进度，以便提供适当的取消逻辑。 <xref:System.Activities.Statements.CancellationScope.CancellationHandler%2A> 不提供有关活动的取消进度的任何信息。  
  
 如果某个未经处理的异常从工作流的根冒出，且 <xref:System.Activities.WorkflowApplication.OnUnhandledException%2A> 处理程序返回 <xref:System.Activities.UnhandledExceptionAction.Cancel>，则也可以从主机取消该工作流。 在此示例中，工作流将启动并引发 <xref:System.ApplicationException>。 由于工作流未处理此异常，因此将调用 <xref:System.Activities.WorkflowApplication.OnUnhandledException%2A> 处理程序。 该处理程序指示运行时取消工作流，并调用当前正在执行的 <xref:System.Activities.Statements.CancellationScope.CancellationHandler%2A> 活动的 <xref:System.Activities.Statements.CancellationScope>。  
  
 [!code-csharp[CFX_WorkflowApplicationExample#36](~/samples/snippets/csharp/VS_Snippets_CFX/cfx_workflowapplicationexample/cs/program.cs#36)]  
  
 调用该工作流时，会将以下输出显示到控制台。  
  
 **启动工作流。**  
**Workflow 6bb2d5d6-f49a-4c6d-a988-478afb86dbe9** 
 中的 OnUnhandledException **引发了 ApplicationException。** 
已 **调用 CancellationHandler。** 
**Workflow 6Bb2d5d6-f49a-4c6d-a988-478afb86dbe9 已取消。**

### <a name="canceling-an-activity-from-inside-a-workflow"></a>从工作流内部取消活动  

 一个活动也可由其父级取消。 例如，如果 <xref:System.Activities.Statements.Parallel> 活动具有多个正在执行的分支，且其 <xref:System.Activities.Statements.Parallel.CompletionCondition%2A> 的计算结果为 `true`，则将取消其不完整的分支。 此示例创建一个具有两个分支的 <xref:System.Activities.Statements.Parallel> 活动。 由于该活动的 <xref:System.Activities.Statements.Parallel.CompletionCondition%2A> 设置为 `true`，因此只要完成其任一分支即完成 <xref:System.Activities.Statements.Parallel>。 在此示例中，完成了分支 2，因此将取消分支 1。  
  
 [!code-csharp[CFX_WorkflowApplicationExample#37](~/samples/snippets/csharp/VS_Snippets_CFX/cfx_workflowapplicationexample/cs/program.cs#37)]  
  
 调用该工作流时，会将以下输出显示到控制台。  
  
 **分支 1 启动。**  
**分支2完成。** 
**分支1已取消。** 
**Workflow E0685e24-18ef-4a47-acf3-5c638732f3be 已完成。**  如果一个异常从活动的根冒出，但在工作流中的较高级别处理该异常，则也可以取消活动。 在此示例中，工作流的主逻辑由一个 <xref:System.Activities.Statements.Sequence> 活动组成。 <xref:System.Activities.Statements.Sequence> 将指定为 <xref:System.Activities.Statements.CancellationScope.Body%2A> 活动所包含的 <xref:System.Activities.Statements.CancellationScope> 活动的 <xref:System.Activities.Statements.TryCatch>。 <xref:System.Activities.Statements.Sequence> 的正文中将引发一个异常，此异常由父 <xref:System.Activities.Statements.TryCatch> 活动处理，并且将取消 <xref:System.Activities.Statements.Sequence>。  
  
 [!code-csharp[CFX_WorkflowApplicationExample#39](~/samples/snippets/csharp/VS_Snippets_CFX/cfx_workflowapplicationexample/cs/program.cs#39)]  
  
 调用该工作流时，会将以下输出显示到控制台。  
  
 **序列启动。**  
**已取消序列。** 
**捕获了异常。** 
**Workflow E3c18939-121e-4c43-af1c-ba1ce977ce55 已完成。**

### <a name="throwing-exceptions-from-a-cancellationhandler"></a>从 CancellationHandler 引发异常  

 对于工作流而言，从 <xref:System.Activities.Statements.CancellationScope.CancellationHandler%2A> 的 <xref:System.Activities.Statements.CancellationScope> 引发的任何异常都是严重异常。 如果存在 <xref:System.Activities.Statements.CancellationScope.CancellationHandler%2A> 无法捕获的异常，请使用 <xref:System.Activities.Statements.TryCatch> 中的 <xref:System.Activities.Statements.CancellationScope.CancellationHandler%2A> 来捕获并处理这些异常。  
  
### <a name="cancellation-using-compensableactivity"></a>使用 CompensableActivity 进行的取消  

 与 <xref:System.Activities.Statements.CancellationScope> 活动一样，<xref:System.Activities.Statements.CompensableActivity> 也具有一个 <xref:System.Activities.Statements.CompensableActivity.CancellationHandler%2A>。 如果取消 <xref:System.Activities.Statements.CompensableActivity>，则将调用其 <xref:System.Activities.Statements.CompensableActivity.CancellationHandler%2A> 中的任何活动。 这对于撤销部分完成的可补偿工作很有用。 有关如何使用 <xref:System.Activities.Statements.CompensableActivity> 进行补偿和取消的信息，请参阅 [补偿](compensation.md)。  
  
## <a name="cancellation-using-custom-activities"></a>使用自定义活动进行的取消  

 自定义活动作者可按照多种不同的方式实现其自定义活动中的取消逻辑。 派生自 <xref:System.Activities.Activity> 的自定义活动可实现取消逻辑，方法是将一个 <xref:System.Activities.Statements.CancellationScope> 或其他包含取消逻辑的自定义活动置于活动正文中。 <xref:System.Activities.AsyncCodeActivity> 和 <xref:System.Activities.NativeActivity> 派生的活动可重写其各自的 <xref:System.Activities.NativeActivity.Cancel%2A> 方法，并在其中提供取消逻辑。 <xref:System.Activities.CodeActivity> 派生的活动不提供任何取消设置，原因是当运行时调用 <xref:System.Activities.CodeActivity.Execute%2A> 方法时，将通过单次执行来执行其所有工作。 如果尚未调用执行方法，且取消了基于 <xref:System.Activities.CodeActivity> 的活动，则该活动将关闭，其状态为 <xref:System.Activities.ActivityInstanceState.Canceled>，并且不调用 <xref:System.Activities.CodeActivity.Execute%2A> 方法。  
  
### <a name="cancellation-using-nativeactivity"></a>使用 NativeActivity 进行的取消  

 <xref:System.Activities.NativeActivity> 派生的活动可重写 <xref:System.Activities.NativeActivity.Cancel%2A> 方法以提供自定义取消逻辑。 如果未重写此方法，则将应用默认工作流取消逻辑。 默认取消是对 <xref:System.Activities.NativeActivity> 不重写 <xref:System.Activities.NativeActivity.Cancel%2A> 方法或其 <xref:System.Activities.NativeActivity.Cancel%2A> 方法调用基 <xref:System.Activities.NativeActivity> 方法的执行的过程 <xref:System.Activities.NativeActivity.Cancel%2A> 。 当取消某个活动时，运行时将标记此活动以进行取消，并自动处理特定清理。 如果此活动仅具有未处理的书签，则移除这些书签，并将此活动标记为 <xref:System.Activities.ActivityInstanceState.Canceled>。 然后将取消已取消活动的任何未处理的子活动。 尝试安排其他子活动将导致尝试被忽略，且此活动将标记为 <xref:System.Activities.ActivityInstanceState.Canceled>。 如果任何未处理的子活动在完成时处于 <xref:System.Activities.ActivityInstanceState.Canceled> 或 <xref:System.Activities.ActivityInstanceState.Faulted> 状态，则此活动将标记为 <xref:System.Activities.ActivityInstanceState.Canceled>。 请注意，可以忽略取消请求。 如果某个活动不具有任何未处理的书签或正在执行的子活动，并且在添加取消标记后不安排任何其他工作项，则将成功完成此活动。 虽然此默认取消可满足多种方案的需求，但如果需要其他取消逻辑，请使用内置取消活动或自定义活动。  
  
 在下面的示例中，将定义基于 <xref:System.Activities.NativeActivity.Cancel%2A> 的自定义 <xref:System.Activities.NativeActivity> 活动的 `ParallelForEach` 重写。 在取消此活动时，该重写将处理此活动的取消逻辑。 此示例是 [非泛型 ParallelForEach](./samples/non-generic-parallelforeach.md) 示例的一部分。  
  
```csharp  
protected override void Cancel(NativeActivityContext context)  
{  
    // If we do not have a completion condition then we can just  
    // use default logic.  
    if (this.CompletionCondition == null)  
    {  
        base.Cancel(context);  
    }  
    else  
    {  
        context.CancelChildren();  
    }  
}  
```  
  
 <xref:System.Activities.NativeActivity> 派生的活动可通过检查 <xref:System.Activities.NativeActivityContext.IsCancellationRequested%2A> 属性来确定是否已请求取消，并可通过调用 <xref:System.Activities.NativeActivityContext.MarkCanceled%2A> 方法将自身标记为已取消。 调用 <xref:System.Activities.NativeActivityContext.MarkCanceled%2A> 不会立刻完成此活动。 通常，运行时将在没有未处理的工作时完成活动，但如果调用 <xref:System.Activities.NativeActivityContext.MarkCanceled%2A>，则最终状态将为 <xref:System.Activities.ActivityInstanceState.Canceled> 而不是 <xref:System.Activities.ActivityInstanceState.Closed>。  
  
### <a name="cancellation-using-asynccodeactivity"></a>使用 AsyncCodeActivity 进行的取消  

 基于 <xref:System.Activities.AsyncCodeActivity> 的活动也可以通过重写 <xref:System.Activities.AsyncCodeActivity.Cancel%2A> 方法来提供自定义取消逻辑。 如果未重写此方法，则在取消活动的情况下不执行任何取消处理。 在下面的示例中，将定义基于 <xref:System.Activities.AsyncCodeActivity.Cancel%2A> 的自定义 <xref:System.Activities.AsyncCodeActivity> 活动的 `ExecutePowerShell` 重写。  在取消此活动时，它将执行所需的取消行为。
  
 [!code-csharp[CFX_WorkflowApplicationExample#1020](~/samples/snippets/csharp/VS_Snippets_CFX/cfx_workflowapplicationexample/cs/program.cs#1020)]  
  
 <xref:System.Activities.AsyncCodeActivity> 派生的活动可通过检查 <xref:System.Activities.AsyncCodeActivityContext.IsCancellationRequested%2A> 属性来确定是否已请求取消，并可通过调用 <xref:System.Activities.AsyncCodeActivityContext.MarkCanceled%2A> 方法将自身标记为已取消。
