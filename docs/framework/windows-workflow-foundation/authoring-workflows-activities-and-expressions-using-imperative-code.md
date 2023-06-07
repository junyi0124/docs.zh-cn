---
title: 使用命令性代码创作工作流、活动和表达式
description: Workflow Foundation 工作流定义是已配置活动对象的树。 使用代码创建工作流定义、活动和表达式。
ms.date: 03/30/2017
ms.assetid: cefc9cfc-2882-4eb9-8c94-7a6da957f2b2
ms.openlocfilehash: 5355f2090317a0bce3fec0f46550b202bd0af095
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96289121"
---
# <a name="authoring-workflows-activities-and-expressions-using-imperative-code"></a>使用命令性代码创作工作流、活动和表达式

工作流定义是已配置活动对象的树。 这种活动树有多种定义方法，包括手动编辑 XAML 或使用工作流设计器来生成 XAML。 但是，并非必须使用 XAML。 工作流定义也可以通过编程方式来创建。 本主题概述如何通过使用代码创建工作流定义、活动和表达式。 有关使用代码处理 XAML 工作流的示例，请参阅在 [xaml 中序列化工作流和活动](serializing-workflows-and-activities-to-and-from-xaml.md)。  
  
## <a name="creating-workflow-definitions"></a>创建工作流定义  

 通过实例化活动类型的实例以及配置活动对象的属性可创建工作流定义。 对于不包含子活动的活动而言，可使用若干代码行来完成。  
  
 [!code-csharp[CFX_WorkflowApplicationExample#47](~/samples/snippets/csharp/VS_Snippets_CFX/cfx_workflowapplicationexample/cs/program.cs#47)]  
  
> [!NOTE]
> 本主题中的示例使用 <xref:System.Activities.WorkflowInvoker> 来运行示例工作流。 有关调用工作流、传递参数以及可用的不同宿主选项的详细信息，请参阅 [使用 WorkflowInvoker 和 WorkflowApplication](using-workflowinvoker-and-workflowapplication.md)。  
  
 在本示例中，将要创建由单个 <xref:System.Activities.Statements.WriteLine> 活动组成的工作流。 还要设置 <xref:System.Activities.Statements.WriteLine> 活动的 <xref:System.Activities.Statements.WriteLine.Text%2A> 参数，并调用该工作流。 如果活动包含子活动，构造方法类似。 以下示例使用一个包含两个 <xref:System.Activities.Statements.Sequence> 活动的 <xref:System.Activities.Statements.WriteLine> 活动。  
  
 [!code-csharp[CFX_WorkflowApplicationExample#48](~/samples/snippets/csharp/VS_Snippets_CFX/cfx_workflowapplicationexample/cs/program.cs#48)]  
  
### <a name="using-object-initializers"></a>使用对象初始值设定项  

 本主题中的示例使用对象初始化语法。 对于用代码创建工作流定义而言，对象初始化语法非常有用，因为它为工作流中的活动提供了分层视图，可以显示活动之间的关系。 通过编程方式创建工作流时，不要求必须使用对象初始化语法。 下面的示例与前面的示例在功能上是等效的。  
  
 [!code-csharp[CFX_WorkflowApplicationExample#49](~/samples/snippets/csharp/VS_Snippets_CFX/cfx_workflowapplicationexample/cs/program.cs#49)]  
  
 有关对象初始值设定项的详细信息，请参阅如何：使用对象初始值设定项 [在不调用构造函数的情况下初始化对象 (c # 编程指南) ](../../csharp/programming-guide/classes-and-structs/how-to-initialize-objects-by-using-an-object-initializer.md) 和 [如何：使用对象初始值设定项声明对象](../../visual-basic/programming-guide/language-features/objects-and-classes/how-to-declare-an-object-by-using-an-object-initializer.md)。  
  
### <a name="working-with-variables-literal-values-and-expressions"></a>使用变量、文本值和表达式  

 使用代码创建工作流定义时，请注意哪些代码是作为创建工作流定义的一部分来执行，哪些代码是作为该工作流实例执行的一部分来执行。 例如，以下工作流将生成一个随机数，并将其写入控制台。  
  
 [!code-csharp[CFX_WorkflowApplicationExample#50](~/samples/snippets/csharp/VS_Snippets_CFX/cfx_workflowapplicationexample/cs/program.cs#50)]  
  
 执行此工作流定义代码时，将会调用 `Random.Next` 并将结果以文本值的形式保存在工作流定义中。 可以调用此工作流的多个实例，全部实例将显示相同的数字。 若要在工作流执行过程中生成随机数，必需使用一个表达式，以便在每次运行工作流时计算该表达式。 在下例中，Visual Basic 表达式与 <xref:Microsoft.VisualBasic.Activities.VisualBasicValue%601> 一起使用。  
  
 [!code-csharp[CFX_WorkflowApplicationExample#51](~/samples/snippets/csharp/VS_Snippets_CFX/cfx_workflowapplicationexample/cs/program.cs#51)]  
  
 前例中的表达式还可以使用 <xref:Microsoft.CSharp.Activities.CSharpValue%601> 和 C# 表达式实现。  
  
```csharp  
new Assign<int>  
{  
    To = n,  
    Value = new CSharpValue<int>("new Random().Next(1, 101)")  
}  
```  
  
 在调用包含 C# 表达式的工作流之前，必须先编译这些表达式。 如果 c # 表达式未编译，则在 <xref:System.NotSupportedException> 调用工作流时将引发，并显示类似于以下内容的消息： ``Expression Activity type 'CSharpValue`1' requires compilation in order to run.  Please ensure that the workflow has been compiled.`` 在大多数情况下，涉及在 Visual Studio 中创建的工作流，c # 表达式会自动编译，但在某些情况下（例如代码工作流），必须手动编译 c # 表达式。 有关如何编译 c # 表达式的示例，请参阅[c # 表达式](csharp-expressions.md)主题的[在代码工作流中使用 c # 表达式](csharp-expressions.md#CodeWorkflows)部分。  
  
 <xref:Microsoft.VisualBasic.Activities.VisualBasicValue%601> 在 Visual Basic 语法中表示可用作表达式右值的表达式，<xref:Microsoft.CSharp.Activities.CSharpValue%601> 在 C# 语法中表示可用作表达式右值的表达式。 每次执行包含活动时，都会计算这些表达式。 将表达式的结果赋给工作流变量 `n`，工作流中的下一个活动将使用这些结果。 若要在运行时访问工作流变量 `n` 的值，需要 <xref:System.Activities.ActivityContext>。 可使用以下 lambda 表达式进行访问。  
  
> [!NOTE]
> 请注意，这些代码都是以 C# 为编程语言的示例，但是一个使用 <xref:Microsoft.VisualBasic.Activities.VisualBasicValue%601>，另一个使用 <xref:Microsoft.CSharp.Activities.CSharpValue%601>。 <xref:Microsoft.VisualBasic.Activities.VisualBasicValue%601> 和 <xref:Microsoft.CSharp.Activities.CSharpValue%601> 可以在 Visual Basic 和 C# 项目中使用。 默认情况下，在工作流设计器中创建的表达式匹配承载项目的语言。 在代码中创建工作流时，所需的语言由工作流编写者决定。  
  
 这些示例中，将表达式的结果赋给工作流变量 `n`，工作流中的下一个活动将使用这些结果。 若要在运行时访问工作流变量 `n` 的值，需要 <xref:System.Activities.ActivityContext>。 可使用以下 lambda 表达式进行访问。  
  
 [!code-csharp[CFX_WorkflowApplicationExample#52](~/samples/snippets/csharp/VS_Snippets_CFX/cfx_workflowapplicationexample/cs/program.cs#52)]  
  
 有关 lambda 表达式的详细信息，请参阅 [ (c # reference) ](../../csharp/language-reference/operators/lambda-expressions.md) 或 [Lambda 表达式的 lambda 表达式 (Visual Basic) ](../../visual-basic/programming-guide/language-features/procedures/lambda-expressions.md)。  
  
 Lambda 表达式不可序列化为 XAML 格式。 如果尝试用 lambda 表达式序列化工作流，会引发 <xref:System.Activities.Expressions.LambdaSerializationException>，并带有消息：“此工作流包含以代码形式指定的 lambda 表达式。 这些表达式不可序列化为 XAML。 若要使工作流可序列化为 XAML，请使用 VisualBasicValue/VisualBasicReference 或 ExpressionServices.Convert(lambda)。 这会将 lambda 表达式转换为表达式活动。” 若要使此表达式与 XAML 相兼容，请使用 <xref:System.Activities.Expressions.ExpressionServices> 和 <xref:System.Activities.Expressions.ExpressionServices.Convert%2A>，如下例所示。  
  
 [!code-csharp[CFX_WorkflowApplicationExample#53](~/samples/snippets/csharp/VS_Snippets_CFX/cfx_workflowapplicationexample/cs/program.cs#53)]  
  
 还可以使用 <xref:Microsoft.VisualBasic.Activities.VisualBasicValue%601>。 请注意，在使用 Visual Basic 表达式时无需 lambda 表达式。  
  
 [!code-csharp[CFX_WorkflowApplicationExample#54](~/samples/snippets/csharp/VS_Snippets_CFX/cfx_workflowapplicationexample/cs/program.cs#54)]  
  
 Visual Basic 表达式在运行时编译为 LINQ 表达式。 前面两个示例都可序列化为 XAML，但是，如果序列化后的 XAML 用于在工作流设计器中查看和编辑，则对表达式使用 <xref:Microsoft.VisualBasic.Activities.VisualBasicValue%601>。 使用 `ExpressionServices.Convert` 的序列化工作流可在设计器中打开，但是表达式的值将为空。 有关将工作流序列化为 XAML 的详细信息，请参阅 [在 xaml 中序列化工作流和活动](serializing-workflows-and-activities-to-and-from-xaml.md)。  
  
#### <a name="literal-expressions-and-reference-types"></a>文本表达式和引用类型  

 在工作流中，通过 <xref:System.Activities.Expressions.Literal%601> 活动表示文本表达式。 下面的 <xref:System.Activities.Statements.WriteLine> 活动在功能上是等效的。  
  
```csharp  
new WriteLine  
{  
    Text = "Hello World."  
},  
new WriteLine  
{  
    Text = new Literal<string>("Hello World.")  
}  
```  
  
 使用 <xref:System.String> 外的其他任何引用类型初始化文本表达式都无效。 在下例中，<xref:System.Activities.Statements.Assign> 活动的 <xref:System.Activities.Statements.Assign.Value%2A> 属性是通过 `List<string>` 用文本表达式初始化的。  
  
```csharp  
new Assign  
{  
    To = new OutArgument<List<string>>(items),  
    Value = new InArgument<List<string>>(new List<string>())  
},  
```  
  
 在验证包含此活动的工作流时，返回以下验证错误：“文本仅支持值类型和不可变类型 System.String。 类型 System.Collections.Generic.List`1[System.String] 不能用作文本。” 如果调用工作流，会引发包含验证错误文本的 <xref:System.Activities.InvalidWorkflowException>。 这是验证错误，因为使用引用类型创建文本表达式不会创建每个工作流实例的引用类型的新实例。 要解决此问题，请将文本表达式替换为创建和返回引用类型新实例的表达式。  
  
```csharp  
new Assign  
{  
    To = new OutArgument<List<string>>(items),  
    Value = new InArgument<List<string>>(new VisualBasicValue<List<string>>("New List(Of String)"))  
},  
```  
  
 有关表达式的详细信息，请参阅 [表达式](expressions.md)。  
  
#### <a name="invoking-methods-on-objects-using-expressions-and-the-invokemethod-activity"></a>使用表达式和 InvokeMethod 活动调用对象上的方法  

 <xref:System.Activities.Expressions.InvokeMethod%601> 活动可用于调用 .NET Framework 中的类的静态和实例方法。 在此主题中的上一个示例中，使用 <xref:System.Random> 类生成随机数。  
  
 [!code-csharp[CFX_WorkflowApplicationExample#51](~/samples/snippets/csharp/VS_Snippets_CFX/cfx_workflowapplicationexample/cs/program.cs#51)]  
  
 <xref:System.Activities.Expressions.InvokeMethod%601> 活动还可以用于调用 <xref:System.Random.Next%2A> 类的 <xref:System.Random> 方法。  
  
```csharp  
new InvokeMethod<int>  
{  
    TargetObject = new InArgument<Random>(new VisualBasicValue<Random>("New Random()")),  
    MethodName = "Next",  
    Parameters =
    {  
        new InArgument<int>(1),  
        new InArgument<int>(101)  
    },  
    Result = n  
}  
```  
  
 <xref:System.Random.Next%2A> 不是静态方法，因此为 <xref:System.Random> 属性提供 <xref:System.Activities.Expressions.InvokeMethod%601.TargetObject%2A> 类的实例。 在此示例中，使用 Visual Basic 表达式创建一个新实例，但是它可能已在之前创建并存储在工作流变量中。 在此示例中，更易于使用 <xref:System.Activities.Statements.Assign%601> 活动，而不是 <xref:System.Activities.Expressions.InvokeMethod%601>活动。 如果最终通过长时间运行的 <xref:System.Activities.Statements.Assign%601> 或 <xref:System.Activities.Expressions.InvokeMethod%601> 活动调用该方法，因为有 <xref:System.Activities.Expressions.InvokeMethod%601> 属性，所以 <xref:System.Activities.Expressions.InvokeMethod%601.RunAsynchronously%2A> 具有优势。 如果此属性设置为 `true`，对于工作流，调用的方法将异步运行。 如果存在其他并行的活动，则在方法异步执行时将不会阻止这些活动。 此外，如果要调用的方法没有返回值，则 <xref:System.Activities.Expressions.InvokeMethod%601> 是调用该方法的适当方法。  
  
## <a name="arguments-and-dynamic-activities"></a>参数与动态活动  

 通过将活动组合到活动树中并配置各自变量和属性的方式可创建以代码编写的工作流定义。 可绑定现有自变量，但无法将新自变量添加到活动中。 这包括传递给根活动的工作流自变量。 在命令性代码中，工作流自变量被指定为新 CLR 类型的属性，而在 XAML 中，它们将使用 `x:Class` 和 `x:Member` 来声明。 将工作流定义创建为内存中对象的树时，由于没有创建新的 CLR 类型，因此无法添加参数。 但是，可将参数添加到 <xref:System.Activities.DynamicActivity>。 在本示例中，将要创建一个 <xref:System.Activities.DynamicActivity%601>，它接受两个整型自变量并将它们相加，然后返回结果。 为每个自变量创建一个 <xref:System.Activities.DynamicActivityProperty>，并将运算结果赋给 <xref:System.Activities.Activity%601.Result%2A> 的 <xref:System.Activities.DynamicActivity%601> 自变量。  
  
 [!code-csharp[CFX_WorkflowApplicationExample#55](~/samples/snippets/csharp/VS_Snippets_CFX/cfx_workflowapplicationexample/cs/program.cs#55)]  
  
 有关动态活动的详细信息，请参阅 [在运行时创建活动](creating-an-activity-at-runtime-with-dynamicactivity.md)。  
  
## <a name="compiled-activities"></a>编译活动  

 动态活动是使用代码定义包含参数的活动的一种方法，但是活动还可以通过代码创建和编译为类型。 可以创建派生自 <xref:System.Activities.CodeActivity> 的简单活动和派生自 <xref:System.Activities.AsyncCodeActivity> 的异步活动。 这些活动可以有自变量、返回值，可以使用命令性代码对其逻辑进行定义。 有关创建这些类型的活动的示例，请参阅 [CodeActivity 基类](workflow-activity-authoring-using-the-codeactivity-class.md) 和 [创建异步活动](creating-asynchronous-activities-in-wf.md)。  
  
 派生自 <xref:System.Activities.NativeActivity> 的活动可以使用命令性代码对其逻辑进行定义，其中可以包含定义逻辑的子活动。 它们还具有对运行时的功能（如创建书签）的完全访问权限。 有关创建基于的活动的示例 <xref:System.Activities.NativeActivity> ，请参阅 [NativeActivity 基类](nativeactivity-base-class.md)、 [如何：创建活动](how-to-create-an-activity.md)和 [使用本机活动的自定义复合](./samples/custom-composite-using-native-activity.md) 示例。  
  
 派生自 <xref:System.Activities.Activity> 的活动仅通过使用子活动定义其逻辑。 这些活动通常使用工作流设计器进行创建，但是也可以使用代码进行定义。 在下面的示例中，定义了一个派生自 `Square` 的 `Activity<int>` 活动。 `Square` 活动具有名为 <xref:System.Activities.InArgument%601> 的单一 `Value`，并且通过使用 <xref:System.Activities.Statements.Sequence> 属性指定 <xref:System.Activities.Activity.Implementation%2A> 活动来定义其逻辑。 <xref:System.Activities.Statements.Sequence> 活动包含 <xref:System.Activities.Statements.WriteLine> 活动和 <xref:System.Activities.Statements.Assign%601> 活动。 这三个活动一起实现 `Square` 活动的逻辑。  
  
```csharp  
class Square : Activity<int>  
{  
    [RequiredArgument]  
    public InArgument<int> Value { get; set; }  
  
    public Square()  
    {  
        this.Implementation = () => new Sequence  
        {  
            Activities =  
            {  
                new WriteLine  
                {  
                    Text = new InArgument<string>((env) => "Squaring the value: " + this.Value.Get(env))  
                },  
                new Assign<int>  
                {  
                    To = new OutArgument<int>((env) => this.Result.Get(env)),  
                    Value = new InArgument<int>((env) => this.Value.Get(env) * this.Value.Get(env))  
                }  
            }  
        };  
    }  
}  
```  
  
 在下面的示例中，使用 `Square` 调用由单个 <xref:System.Activities.WorkflowInvoker> 活动组成的工作流定义。  
  
```csharp  
Dictionary<string, object> inputs = new Dictionary<string, object> {{ "Value", 5}};  
int result = WorkflowInvoker.Invoke(new Square(), inputs);  
Console.WriteLine("Result: {0}", result);  
```  
  
 在调用工作流时，下面的输出显示到控制台：  
  
 **Squaring the value: 5**  
**Result: 25**
