---
title: 使用活动委托
ms.date: 03/30/2017
ms.assetid: e33cf876-8979-440b-9b23-4a12d1139960
ms.openlocfilehash: 66a03187336475ed377fda032506cfa66d3daf58
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96293294"
---
# <a name="using-activity-delegates"></a>使用活动委托

通过活动委托，活动作者可以公开具有特定签名的回调，活动的用户可为其提供基于活动的处理程序。 可以使用两种类型的活动委托：<xref:System.Activities.ActivityAction%601> 用于定义没有返回值的活动委托，<xref:System.Activities.ActivityFunc%601> 用于定义有返回值的活动委托。

在必须对子活动进行限制，使其包含特定签名的情况下，活动委托非常有用。 例如，<xref:System.Activities.Statements.While> 活动可包含任何类型的无约束子活动，但 <xref:System.Activities.Statements.ForEach%601> 活动的主体是一个 <xref:System.Activities.ActivityAction%601>，并且 <xref:System.Activities.Statements.ForEach%601> 最终执行的子活动必须具有 <xref:System.Activities.InArgument%601> 与 <xref:System.Activities.Statements.ForEach%601> 所枚举的集合成员相同的类型。

## <a name="using-activityaction"></a>使用 ActivityAction

一些 [!INCLUDE[netfx_current_short](../../../includes/netfx-current-short-md.md)] 活动将使用活动操作，如 <xref:System.Activities.Statements.Catch> 活动和 <xref:System.Activities.Statements.ForEach%601> 活动。 在每种情况下，活动操作都表示一个位置，当使用这些活动之一构造工作流时，工作流作者在此处指定一个活动来提供所需的行为。 在下面的示例中，使用 <xref:System.Activities.Statements.ForEach%601> 活动来向控制台窗口显示文本。 使用一个与 <xref:System.Activities.Statements.ForEach%601> 的类型（此处为 string）相匹配的 <xref:System.Activities.ActivityAction%601> 来指定 <xref:System.Activities.Statements.ForEach%601> 的主体。 在 <xref:System.Activities.Statements.WriteLine> 中指定的 <xref:System.Activities.ActivityDelegate.Handler%2A> 活动将它的 <xref:System.Activities.Statements.WriteLine.Text%2A> 自变量绑定到 <xref:System.Activities.Statements.ForEach%601> 活动迭代的集合中的字符串值。

[!code-csharp[CFX_ActivityExample#6](~/samples/snippets/csharp/VS_Snippets_CFX/CFX_ActivityExample/cs/Program.cs#6)]

actionArgument 用于将集合中的各个项流到 WriteLine。 在调用工作流时，将下面的输出显示到控制台。

```console
HelloWorld.
```

本主题中的示例使用对象初始化语法。 对于用代码创建工作流定义而言，对象初始化语法非常有用，因为它为工作流中的活动提供了分层视图，可以显示活动之间的关系。 通过编程方式创建工作流时，不要求必须使用对象初始化语法。 下面的示例与前面的示例在功能上是等效的。

[!code-csharp[CFX_ActivityExample#7](~/samples/snippets/csharp/VS_Snippets_CFX/CFX_ActivityExample/cs/Program.cs#7)]

有关对象初始值设定项的详细信息，请参阅如何：使用对象初始值设定项将对象 [初始化为不调用构造函数 (c # 编程指南) ](../../csharp/programming-guide/classes-and-structs/how-to-initialize-objects-by-using-an-object-initializer.md) 和 [如何：使用对象初始值设定项 (Visual Basic) 声明对象 ](../../visual-basic/programming-guide/language-features/objects-and-classes/how-to-declare-an-object-by-using-an-object-initializer.md)。

在下面的示例中，在工作流中使用一个 <xref:System.Activities.Statements.TryCatch> 活动。 该工作流引发一个 <xref:System.ApplicationException>，并由 <xref:System.Activities.Statements.Catch%601> 活动对其进行处理。 <xref:System.Activities.Statements.Catch%601>活动活动操作的处理程序是一种 <xref:System.Activities.Statements.WriteLine> 活动，异常详细信息通过使用传递给它 `ex` <xref:System.Activities.DelegateInArgument%601> 。

[!code-csharp[CFX_WorkflowApplicationExample#33](~/samples/snippets/csharp/VS_Snippets_CFX/cfx_workflowapplicationexample/cs/program.cs#33)]

创建定义 <xref:System.Activities.ActivityAction%601> 的自定义活动时，使用 <xref:System.Activities.Statements.InvokeAction%601> 来建立调用该 <xref:System.Activities.ActivityAction%601> 的模型。 在本示例中，定义了自定义 `WriteLineWithNotification` 活动。 此活动由一个 <xref:System.Activities.Statements.Sequence> 组成，它包含一个 <xref:System.Activities.Statements.WriteLine> 活动，该活动后面是一个调用接收一个字符串参数的 <xref:System.Activities.Statements.InvokeAction%601> 的 <xref:System.Activities.ActivityAction%601>。

[!code-csharp[CFX_ActivityExample#1](~/samples/snippets/csharp/VS_Snippets_CFX/CFX_ActivityExample/cs/Program.cs#1)]

使用 `WriteLineWithNotification` 活动创建工作流时，工作流作者在活动操作的 <xref:System.Activities.ActivityDelegate.Handler%2A> 中指定所需的自定义逻辑。 在本示例中，创建一个使用 `WriteLineWithNotification` 活动的工作流，并将 <xref:System.Activities.Statements.WriteLine> 活动用作 <xref:System.Activities.ActivityDelegate.Handler%2A>。

[!code-csharp[CFX_ActivityExample#2](~/samples/snippets/csharp/VS_Snippets_CFX/CFX_ActivityExample/cs/Program.cs#2)]

<xref:System.Activities.Statements.InvokeAction%601> 有多个泛型版本，提供 <xref:System.Activities.ActivityAction%601> 用于传递一个或多个自变量。

## <a name="using-activityfunc"></a>使用 ActivityFunc

当活动中没有结果值时，<xref:System.Activities.ActivityAction%601> 将很有用，当返回结果值时，将使用 <xref:System.Activities.ActivityFunc%601>。 创建定义 <xref:System.Activities.ActivityFunc%601> 的自定义活动时，使用 <xref:System.Activities.Expressions.InvokeFunc%601> 来建立调用该 <xref:System.Activities.ActivityFunc%601> 的模型。 下面的示例定义了一个 `WriteFillerText` 活动。 为了提供填充符文本，将指定一个 <xref:System.Activities.Expressions.InvokeFunc%601>，它接收一个整数参数并具有一个字符串结果。 在检索填充符文本之后，将会使用 <xref:System.Activities.Statements.WriteLine> 活动将其显示到控制台中。

[!code-csharp[CFX_ActivityExample#3](~/samples/snippets/csharp/VS_Snippets_CFX/CFX_ActivityExample/cs/Program.cs#3)]

若要提供文本，必须使用接收一个 `int` 自变量并具有一个字符串结果的活动。 本示例演示满足这些需求的 `TextGenerator` 活动。

[!code-csharp[CFX_ActivityExample#4](~/samples/snippets/csharp/VS_Snippets_CFX/CFX_ActivityExample/cs/Program.cs#4)]

若要将 `TextGenerator` 活动和 `WriteFillerText` 活动一起使用，应将前者指定为 <xref:System.Activities.ActivityDelegate.Handler%2A>。

[!code-csharp[CFX_ActivityExample#5](~/samples/snippets/csharp/VS_Snippets_CFX/CFX_ActivityExample/cs/Program.cs#5)]
