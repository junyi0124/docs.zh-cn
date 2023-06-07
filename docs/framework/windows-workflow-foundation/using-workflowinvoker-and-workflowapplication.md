---
title: 使用 WorkflowInvoker 和 WorkflowApplication
description: 本文介绍了如何使用 WorkflowInvoker 和 WorkflowApplication 在 Windows Workflow Foundation 中进行工作流托管。
ms.date: 03/30/2017
ms.assetid: cd0e583c-a3f9-4fa2-b247-c7b3368c48a7
ms.openlocfilehash: 1cc84afe002615c317309054179abd1af5800e9c
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96254605"
---
# <a name="using-workflowinvoker-and-workflowapplication"></a>使用 WorkflowInvoker 和 WorkflowApplication

Windows Workflow Foundation (WF) 提供多种承载工作流的方法。 <xref:System.Activities.WorkflowInvoker> 提供一种简单工作流调用方法，就像方法调用一样，仅可用于不使用持久性的工作流。 <xref:System.Activities.WorkflowApplication> 为执行工作流（包括生命周期事件通知、执行控制、书签恢复和持久性）提供更丰富的模型。 <xref:System.ServiceModel.Activities.WorkflowServiceHost> 为消息传递活动提供支持，主要用于工作流服务。 本主题介绍使用 <xref:System.Activities.WorkflowInvoker> 和 <xref:System.Activities.WorkflowApplication> 的工作流承载。 有关托管工作流的详细信息 <xref:System.ServiceModel.Activities.WorkflowServiceHost> ，请参阅 [工作流服务](../wcf/feature-details/workflow-services.md) 和 [承载工作流服务概述](../wcf/feature-details/hosting-workflow-services-overview.md)。  
  
## <a name="using-workflowinvoker"></a>使用 WorkflowInvoker  

 <xref:System.Activities.WorkflowInvoker> 提供一种执行工作流的模型，如同使用方法调用。 若要使用 <xref:System.Activities.WorkflowInvoker> 调用工作流，请调用 <xref:System.Activities.WorkflowInvoker.Invoke%2A> 方法，并传入要调用的工作流的工作流定义。 在本示例中，使用 <xref:System.Activities.Statements.WriteLine> 调用 <xref:System.Activities.WorkflowInvoker> 活动。  
  
 [!code-csharp[CFX_WorkflowInvokerExample#1](~/samples/snippets/csharp/VS_Snippets_CFX/cfx_workflowinvokerexample/cs/program.cs#1)]  
  
 使用 <xref:System.Activities.WorkflowInvoker> 调用工作流时，该工作流在调用线程上执行，并且在该工作流完成之前（包括任何空闲时间）会阻止 <xref:System.Activities.WorkflowInvoker.Invoke%2A> 方法。 若要配置一个工作流必须在其间完成的超时间隔，请使用一个采用 <xref:System.Activities.WorkflowInvoker.Invoke%2A> 参数的 <xref:System.TimeSpan> 重载。 在本示例中，将使用两个不同的超时间隔来调用两次工作流。 第一个工作流完成，但第二个工作流未完成。  
  
 [!code-csharp[CFX_WorkflowInvokerExample#50](~/samples/snippets/csharp/VS_Snippets_CFX/cfx_workflowinvokerexample/cs/program.cs#50)]  
  
> [!NOTE]
> 仅在达到超时间隔且工作流在执行期间进入空闲状态时才会引发 <xref:System.TimeoutException>。 如果工作流未进入空闲状态，那么完成时间超过指定超时间隔的工作流将会成功完成。  
  
 <xref:System.Activities.WorkflowInvoker> 还提供了调用方法的异步版本。 有关详细信息，请参阅 <xref:System.Activities.WorkflowInvoker.InvokeAsync%2A> 和 <xref:System.Activities.WorkflowInvoker.BeginInvoke%2A>。  
  
### <a name="setting-input-arguments-of-a-workflow"></a>设置工作流的输入自变量  

 使用输入参数字典可将数据传入工作流，其中自变量名作为键，并映射到工作流的输入自变量。 在本示例中，将要调用 <xref:System.Activities.Statements.WriteLine>，并使用输入参数字典指定其 <xref:System.Activities.Statements.WriteLine.Text%2A> 自变量值。  
  
 [!code-csharp[CFX_WorkflowInvokerExample#3](~/samples/snippets/csharp/VS_Snippets_CFX/cfx_workflowinvokerexample/cs/program.cs#3)]  
  
### <a name="retrieving-output-arguments-of-a-workflow"></a>检索工作流的输出自变量  

 使用调用 <xref:System.Activities.WorkflowInvoker.Invoke%2A> 所返回的输出字典，可获得工作流的输出形参。 下面的示例调用一个工作流，该工作流包含有两个输入自变量和两个输出自变量的单个 `Divide` 活动。 调用工作流时，会传递包含每个输入自变量的值的 `arguments` 字典（由自变量名键控）。 当对 `Invoke` 的调用返回时，将在 `outputs` 字典中返回每个输出参数（也由参数名键控）。  
  
 [!code-csharp[CFX_WorkflowInvokerExample#120](~/samples/snippets/csharp/VS_Snippets_CFX/cfx_workflowinvokerexample/cs/program.cs#120)]  
  
 [!code-csharp[CFX_WorkflowInvokerExample#20](~/samples/snippets/csharp/VS_Snippets_CFX/cfx_workflowinvokerexample/cs/program.cs#20)]  
  
 如果工作流派生自 <xref:System.Activities.ActivityWithResult>（例如 `CodeActivity<TResult>` 或 `Activity<TResult>`），并且除了正确定义的 <xref:System.Activities.Activity%601.Result%2A> 输出自变量之外还有其他输出自变量，则必须使用 `Invoke` 的非泛型重载来检索其他自变量。 为此，传递给 `Invoke` 的工作流定义必须为 <xref:System.Activities.Activity> 类型。 在此示例中，`Divide` 活动派生自 `CodeActivity<int>`，但被声明为 <xref:System.Activities.Activity>。因此，将使用 `Invoke` 的非泛型重载，并返回参数字典，而不是单个返回值。  
  
 [!code-csharp[CFX_WorkflowInvokerExample#121](~/samples/snippets/csharp/VS_Snippets_CFX/cfx_workflowinvokerexample/cs/program.cs#121)]  
  
 [!code-csharp[CFX_WorkflowInvokerExample#21](~/samples/snippets/csharp/VS_Snippets_CFX/cfx_workflowinvokerexample/cs/program.cs#21)]  
  
## <a name="using-workflowapplication"></a>使用 WorkflowApplication  

 <xref:System.Activities.WorkflowApplication> 为工作流实例管理提供了丰富的功能集。 <xref:System.Activities.WorkflowApplication> 承担实际 <xref:System.Activities.Hosting.WorkflowInstance> 的线程安全代理任务，可封装运行时，并提供若干方法，用于创建和加载工作流实例、暂停与继续、终止和通知生命周期事件。 若要使用 <xref:System.Activities.WorkflowApplication> 运行工作流，应创建 <xref:System.Activities.WorkflowApplication>，订阅所需的所有生命周期事件，启动工作流，然后等待其完成。 在本示例中，将要创建由 <xref:System.Activities.Statements.WriteLine> 活动组成定义的工作流，并使用指定工作流定义创建 <xref:System.Activities.WorkflowApplication>。 处理 <xref:System.Activities.WorkflowApplication.Completed%2A> 以便在工作流完成时通知宿主，通过调用 <xref:System.Activities.WorkflowApplication.Run%2A> 启动工作流，然后宿主等待工作流完成。 工作流完成时，设置 <xref:System.Threading.AutoResetEvent>，并且主机应用程序可以继续执行，如下例所示。  
  
 [!code-csharp[CFX_WorkflowApplicationExample#31](~/samples/snippets/csharp/VS_Snippets_CFX/cfx_workflowapplicationexample/cs/program.cs#31)]  
  
### <a name="workflowapplication-lifecycle-events"></a>WorkflowApplication 生命周期事件  

 除了 <xref:System.Activities.WorkflowApplication.Completed%2A>，当卸载工作流（<xref:System.Activities.WorkflowApplication.Unloaded%2A>）、中止工作流（<xref:System.Activities.WorkflowApplication.Aborted%2A>）、工作流变空闲（<xref:System.Activities.WorkflowApplication.Idle%2A> 和 <xref:System.Activities.WorkflowApplication.PersistableIdle%2A>）或产生未经处理的异常（<xref:System.Activities.WorkflowApplication.OnUnhandledException%2A>）时，宿主作者会收到通知。 工作流应用程序开发人员可处理这些通知，并执行适当的操作，如下例所示。  
  
 [!code-csharp[CFX_WorkflowApplicationExample#32](~/samples/snippets/csharp/VS_Snippets_CFX/cfx_workflowapplicationexample/cs/program.cs#32)]  
  
### <a name="setting-input-arguments-of-a-workflow"></a>设置工作流的输入自变量  

 启动工作流时可使用形参字典将数据传入工作流，这与使用 <xref:System.Activities.WorkflowInvoker> 时传入数据的方式类似。 字典中的每一项都映射到指定工作流的一个输入自变量。 在本示例中，将要调用由 <xref:System.Activities.Statements.WriteLine> 活动组成的工作流，并使用输入参数字典指定其 <xref:System.Activities.Statements.WriteLine.Text%2A> 自变量。  
  
 [!code-csharp[CFX_WorkflowApplicationExample#30](~/samples/snippets/csharp/VS_Snippets_CFX/cfx_workflowapplicationexample/cs/program.cs#30)]  
  
### <a name="retrieving-output-arguments-of-a-workflow"></a>检索工作流的输出自变量  

 当工作流完成时，可通过访问 <xref:System.Activities.WorkflowApplication.Completed%2A> 字典在 <xref:System.Activities.WorkflowApplicationCompletedEventArgs.Outputs%2A?displayProperty=nameWithType> 处理程序中检索任何输出自变量。 下面的示例使用 <xref:System.Activities.WorkflowApplication> 承载一个工作流。 <xref:System.Activities.WorkflowApplication>实例是使用由单个活动组成的工作流定义构造的 `DiceRoll` 。 `DiceRoll` 活动包含两个表示掷骰子操作结果的输出自变量。 当工作流完成时，将在 <xref:System.Activities.WorkflowApplication.Completed%2A> 处理程序中检索输出。  
  
 [!code-csharp[CFX_WorkflowApplicationExample#130](~/samples/snippets/csharp/VS_Snippets_CFX/cfx_workflowapplicationexample/cs/program.cs#130)]  
  
 [!code-csharp[CFX_WorkflowApplicationExample#21](~/samples/snippets/csharp/VS_Snippets_CFX/cfx_workflowapplicationexample/cs/program.cs#21)]  
  
> [!NOTE]
> <xref:System.Activities.WorkflowApplication> 和 <xref:System.Activities.WorkflowInvoker> 采用输入自变量字典，并返回 `out` 自变量的字典。 这些字典形参、属性及返回值的类型为 `IDictionary<string, object>`。 传入的字典类的实际实例可以是实现了 `IDictionary<string, object>` 的任何类。 在这些示例中使用了 `Dictionary<string, object>`。 有关字典的详细信息，请参阅 <xref:System.Collections.Generic.IDictionary%602> 和 <xref:System.Collections.Generic.Dictionary%602> 。  
  
### <a name="passing-data-into-a-running-workflow-using-bookmarks"></a>使用书签将数据传入运行中的工作流  

 书签是使活动能够被动等待继续的机制，也是将数据传入运行中的工作流实例的机制。 如果某个活动在等待数据，它可以创建 <xref:System.Activities.Bookmark>，并注册一个在继续 <xref:System.Activities.Bookmark> 时要调用的回调方法，如下例所示。  
  
 [!code-csharp[CFX_WorkflowApplicationExample#15](~/samples/snippets/csharp/VS_Snippets_CFX/cfx_workflowapplicationexample/cs/program.cs#15)]  
  
 执行时，`ReadLine` 活动创建 <xref:System.Activities.Bookmark>，注册回调，然后等待 <xref:System.Activities.Bookmark> 继续。 继续后，`ReadLine` 活动将随 <xref:System.Activities.Bookmark> 传递的数据赋给其 <xref:System.Activities.Activity%601.Result%2A> 实参。 在本示例中，将要创建一个工作流，该工作流使用 `ReadLine` 活动来收集用户名称并将其显示在控制台窗口中。  
  
 [!code-csharp[CFX_WorkflowApplicationExample#22](~/samples/snippets/csharp/VS_Snippets_CFX/cfx_workflowapplicationexample/cs/program.cs#22)]  
  
 执行 `ReadLine` 活动时，该活动创建一个名为 <xref:System.Activities.Bookmark> 的 `UserName`，然后等待书签继续。 宿主收集所需的数据，然后继续 <xref:System.Activities.Bookmark>。 工作流继续，显示该名称，然后完成。  
  
 主机应用程序可以检查工作流，以确定其中是否存在任何活动书签。 这可以通过以下方式来完成：调用 <xref:System.Activities.WorkflowApplication.GetBookmarks%2A> 实例的 <xref:System.Activities.WorkflowApplication> 方法，或者，在 <xref:System.Activities.WorkflowApplicationIdleEventArgs> 处理程序中检查 <xref:System.Activities.WorkflowApplication.Idle%2A>。  
  
 下面的代码示例与前一个示例类似，但在继续书签之前会枚举活动书签。 该示例启动工作流，一旦创建了 <xref:System.Activities.Bookmark> 并且工作流进入空闲状态，就调用 <xref:System.Activities.WorkflowApplication.GetBookmarks%2A>。 完成工作流时，会向控制台显示以下输出。  
  
 **你叫什么名字？**  
**BookmarkName：用户名-OwnerDisplayName： ReadLine** 
**Steve** 
**你好，Steve**

[!code-csharp[CFX_WorkflowApplicationExample#14](~/samples/snippets/csharp/VS_Snippets_CFX/cfx_workflowapplicationexample/cs/program.cs#14)]  
  
 下面的代码示例检查传递给 <xref:System.Activities.WorkflowApplicationIdleEventArgs> 实例的 <xref:System.Activities.WorkflowApplication.Idle%2A>  处理程序的 <xref:System.Activities.WorkflowApplication>。 在此示例中，进入空闲状态的工作流包含一个由名为 <xref:System.Activities.Bookmark> 的活动所拥有的名为 `EnterGuess` 的 `ReadInt`。 此代码示例基于 [如何：运行工作流，该工作流](how-to-run-a-workflow.md)是 [入门教程](getting-started-tutorial.md)的一部分。 如果修改了该阶段中的 <xref:System.Activities.WorkflowApplication.Idle%2A> 处理程序以包含此示例中的代码，则将显示以下输出。  
  
 **BookmarkName: EnterGuess - OwnerDisplayName: ReadInt**

 [!code-csharp[CFX_WorkflowApplicationExample#2](~/samples/snippets/csharp/VS_Snippets_CFX/cfx_workflowapplicationexample/cs/program.cs#2)]  
  
## <a name="summary"></a>总结  

 <xref:System.Activities.WorkflowInvoker> 提供了一个简便的方法来调用工作流，尽管提供了在工作流开始时传入数据以及从完成的工作流中提取数据的方法，但不提供使用 <xref:System.Activities.WorkflowApplication> 时可用的更为复杂的方案。
