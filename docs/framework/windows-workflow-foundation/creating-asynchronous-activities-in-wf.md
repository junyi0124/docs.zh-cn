---
title: 在 WF 中创建异步活动
description: 了解如何使用 AsyncCodeActivity 创建自定义异步活动，该活动允许派生的活动实现异步执行逻辑。
ms.date: 03/30/2017
ms.assetid: 497e81ed-5eef-460c-ba55-fae73c05824f
ms.openlocfilehash: a03fbde1ff27084cc36dffc3a1912220d9bbca7e
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96242079"
---
# <a name="creating-asynchronous-activities-in-wf"></a>在 WF 中创建异步活动

<xref:System.Activities.AsyncCodeActivity> 提供一个可供活动作者使用的基类，该基类允许派生的活动实现异步执行逻辑。 这对如下自定义活动非常有用：必须执行异步工作，而不会保持工作流计划程序线程并阻止可以并行运行的所有活动。 本主题概述了如何使用 <xref:System.Activities.AsyncCodeActivity> 创建自定义异步活动。  
  
## <a name="using-asynccodeactivity"></a>使用 AsyncCodeActivity  

 <xref:System.Activities?displayProperty=nameWithType> 为自定义活动作者提供了不同基类，以满足不同的活动创作需求。 每一种基类都采用特定语义，并为工作流作者（和活动运行时）提供相应协定。 基于 <xref:System.Activities.AsyncCodeActivity> 的活动是指异步执行工作（相对于计划程序线程）且以托管代码表示其执行逻辑的活动。 由于将异步执行，因此 <xref:System.Activities.AsyncCodeActivity> 可能会在执行时引入一个空闲点。 鉴于异步工作灵活多变的性质，<xref:System.Activities.AsyncCodeActivity> 始终创建一个非持久性块来持久执行活动。 这可以防止工作流运行时在异步工作中持久化工作流实例，还可以防止工作流实例在执行异步代码时进行卸载。  
  
### <a name="asynccodeactivity-methods"></a>AsyncCodeActivity 方法  

 从 <xref:System.Activities.AsyncCodeActivity> 派生的活动可以通过用自定义代码重写 <xref:System.Activities.AsyncCodeActivity.BeginExecute%2A> 和 <xref:System.Activities.AsyncCodeActivity.EndExecute%2A> 方法来创建异步执行逻辑。 当运行时调用这些方法时，会向这些方法传递一个 <xref:System.Activities.AsyncCodeActivityContext>。 <xref:System.Activities.AsyncCodeActivityContext>允许活动作者 <xref:System.Activities.AsyncCodeActivity.BeginExecute%2A> /  <xref:System.Activities.AsyncCodeActivity.EndExecute%2A> 在上下文的属性中提供共享状态 <xref:System.Activities.AsyncCodeActivityContext.UserState%2A> 。 在下面的示例中，`GenerateRandom` 活动异步生成一个随机数。  
  
 [!code-csharp[CFX_ActivityExample#8](~/samples/snippets/csharp/VS_Snippets_CFX/CFX_ActivityExample/cs/Program.cs#8)]  
  
 上一个示例活动派生自 <xref:System.Activities.AsyncCodeActivity%601>，且具有一个名为 `OutArgument<int>` 的提升 `Result`。 `GetRandom` 方法返回的值由 <xref:System.Activities.AsyncCodeActivity%601.EndExecute%2A> 重写提取和返回，并将此值设置为 `Result` 值。 未返回结果的异步活动应派生自 <xref:System.Activities.AsyncCodeActivity>。 在下面的示例中，定义了一个派生自 `DisplayRandom` 的 <xref:System.Activities.AsyncCodeActivity> 活动。 此活动与 `GetRandom` 活动类似，只不过它在返回结果时会向控制台显示一条消息。  
  
 [!code-csharp[CFX_ActivityExample#11](~/samples/snippets/csharp/VS_Snippets_CFX/CFX_ActivityExample/cs/Program.cs#11)]  
  
 请注意，由于没有返回值，`DisplayRandom` 将使用 <xref:System.Action> 而非 <xref:System.Func%602> 来调用其委托，并且该委托不返回任何值。  
  
 <xref:System.Activities.AsyncCodeActivity> 还提供 <xref:System.Activities.AsyncCodeActivity.Cancel%2A> 重写。 <xref:System.Activities.AsyncCodeActivity.BeginExecute%2A> 和 <xref:System.Activities.AsyncCodeActivity.EndExecute%2A> 是必需重写，而 <xref:System.Activities.AsyncCodeActivity.Cancel%2A> 是可选重写且可被覆盖，因此当取消或中止活动时，该活动可以清除其未处理的异步状态。 如果可以清除，并且 `AsyncCodeActivity.ExecutingActivityInstance.IsCancellationRequested` 为 `true`，则活动应调用 <xref:System.Activities.AsyncCodeActivityContext.MarkCanceled%2A>。 对于工作流实例而言，该方法引发的任何异常都是致命的。  
  
 [!code-csharp[CFX_ActivityExample#10](~/samples/snippets/csharp/VS_Snippets_CFX/CFX_ActivityExample/cs/Program.cs#10)]  
  
### <a name="invoking-asynchronous-methods-on-a-class"></a>对类调用异步方法  

 .NET Framework 中的许多类都提供了异步功能，可以使用基于的活动来异步调用此功能 <xref:System.Activities.AsyncCodeActivity> 。 在下面的示例中，创建了一个活动，该活动使用类异步创建一个文件 <xref:System.IO.FileStream> 。  
  
 [!code-csharp[CFX_ActivityExample#12](~/samples/snippets/csharp/VS_Snippets_CFX/CFX_ActivityExample/cs/Program.cs#12)]  
  
### <a name="sharing-state-between-the-beginexecute-and-endexecute-methods"></a>在 BeginExecute 和 EndExecute 方法之间共享状态  

 在前面的示例中，在 <xref:System.IO.FileStream> 中访问了在 <xref:System.Activities.AsyncCodeActivity.BeginExecute%2A> 中创建的 <xref:System.Activities.AsyncCodeActivity.EndExecute%2A> 对象。 这很可能是因为 `file` 变量被传递到 <xref:System.Activities.AsyncCodeActivityContext.UserState%2A?displayProperty=nameWithType> 中的 <xref:System.Activities.AsyncCodeActivity.BeginExecute%2A> 属性。 这是用于在 <xref:System.Activities.AsyncCodeActivity.BeginExecute%2A> 和 <xref:System.Activities.AsyncCodeActivity.EndExecute%2A> 之间共享状态的正确方法。 使用派生类（在这个例子中为 `FileWriter`）中的成员变量在 <xref:System.Activities.AsyncCodeActivity.BeginExecute%2A> 和 <xref:System.Activities.AsyncCodeActivity.EndExecute%2A> 之间共享状态是不正确的做法，因为该活动对象可能被多个活动实例所引用。 如果尝试使用成员变量共享状态，可能导致来自一个 <xref:System.Activities.ActivityInstance> 中的值覆盖或使用来自另一个 <xref:System.Activities.ActivityInstance> 中的值。  
  
### <a name="accessing-argument-values"></a>访问自变量值  

 <xref:System.Activities.AsyncCodeActivity> 的环境由在活动中定义的自变量组成。 可以 <xref:System.Activities.AsyncCodeActivity.BeginExecute%2A> / <xref:System.Activities.AsyncCodeActivity.EndExecute%2A> 使用参数从重写访问这些参数 <xref:System.Activities.AsyncCodeActivityContext> 。 无法在委托中访问实参，但是可以使用其形参将实参值或任何其他所需的数据传入到委托。 下面的示例定义了随机数生成活动，该活动从其 `Max` 参数中获取其上界（随机数可以取该上界值）。 当调用委托时，会将自变量的值传入到异步代码中。  
  
 [!code-csharp[CFX_ActivityExample#9](~/samples/snippets/csharp/VS_Snippets_CFX/CFX_ActivityExample/cs/Program.cs#9)]  
  
### <a name="scheduling-actions-or-child-activities-using-asynccodeactivity"></a>使用 AsyncCodeActivity 安排操作或子活动  

 <xref:System.Activities.AsyncCodeActivity> 派生的自定义活动提供了以异步方式执行与工作流线程相关的工作的方法，但不提供安排子活动或操作的功能。  但是，可以通过组合方式将异步行为纳入子活动的安排。 可以创建一个异步活动，然后将其与 <xref:System.Activities.Activity> 或 <xref:System.Activities.NativeActivity> 派生活动组合在一起，以便提供异步行为以及子活动或操作的安排。 例如，可以创建一个活动，该活动派生自 <xref:System.Activities.Activity>，并且作为其实现令 <xref:System.Activities.Statements.Sequence> 包含异步活动以及实现该活动的逻辑的其他活动。 有关使用和编写活动的更多示例 <xref:System.Activities.Activity> <xref:System.Activities.NativeActivity> ，请参阅 [如何：创建活动](how-to-create-an-activity.md) 和 [活动创作选项](activity-authoring-options-in-wf.md)。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Action>
- <xref:System.Func%602>
