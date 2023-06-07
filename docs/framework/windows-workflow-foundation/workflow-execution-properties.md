---
title: 工作流执行属性
ms.date: 03/30/2017
ms.assetid: a50e088e-3a45-4267-bd51-1a3e6c2d246d
ms.openlocfilehash: be9ae5924786ea1e23cc649034d927789c64e405
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96293788"
---
# <a name="workflow-execution-properties"></a>工作流执行属性

通过线程本地存储区 (TLS)，CLR 可为每个线程维护一个执行上下文。 此执行上下文管理已知的线程属性，例如，线程标识、环境事务、当前权限集以及用户定义的线程属性（如已命名的槽）。  
  
 与直接以 CLR 为目标的程序不同，工作流程序是在线程不可知的环境中执行的以分层形式确定范围的活动树。 这意味着标准的 TLS 机制无法直接用于确定给定工作项范围所在的上下文。 例如，两个并行的执行分支可能使用不同的事务，但计划程序可能会在同一 CLR 线程上交错执行这两个分支。  
  
 工作流执行属性提供向活动环境添加上下文特定属性的机制。 这将允许活动声明哪些属性在其子树范围内，并且还提供了用于设置和关闭 TLS 以与 CLR 对象正确交互的挂钩。  
  
## <a name="creating-and-using-workflow-execution-properties"></a>创建和使用工作流执行属性  

 工作流执行属性通常实现 <xref:System.Activities.IExecutionProperty> 接口，尽管着重于消息传递的属性可能会实现 <xref:System.ServiceModel.Activities.ISendMessageCallback> 和 <xref:System.ServiceModel.Activities.IReceiveMessageCallback>。 若要创建工作流执行属性，请创建一个实现 <xref:System.Activities.IExecutionProperty> 接口并实现成员 <xref:System.Activities.IExecutionProperty.SetupWorkflowThread%2A> 和 <xref:System.Activities.IExecutionProperty.CleanupWorkflowThread%2A> 的类。 通过这些成员，该执行属性可在包含该属性的活动（包括任何子活动）的每个工作脉冲期间正确设置和关闭线程本地存储区。 在本示例中，创建了设置 `ConsoleColorProperty` 的 `Console.ForegroundColor`。  
  
```csharp  
class ConsoleColorProperty : IExecutionProperty  
{  
    public const string Name = "ConsoleColorProperty";  
  
    ConsoleColor original;  
    ConsoleColor color;  
  
    public ConsoleColorProperty(ConsoleColor color)  
    {  
        this.color = color;  
    }  
  
    void IExecutionProperty.SetupWorkflowThread()  
    {  
        original = Console.ForegroundColor;  
        Console.ForegroundColor = color;  
    }  
  
    void IExecutionProperty.CleanupWorkflowThread()  
    {  
        Console.ForegroundColor = original;  
    }  
}  
```  
  
 活动创作者可通过在该活动的执行重写中注册此属性来使用它。 在本示例中，定义了一个 `ConsoleColorScope` 活动，该活动通过将 `ConsoleColorProperty` 添加到当前 <xref:System.Activities.NativeActivityContext.Properties%2A> 的 <xref:System.Activities.NativeActivityContext> 集合来注册该属性。  
  
```csharp  
public sealed class ConsoleColorScope : NativeActivity  
{  
    public ConsoleColorScope()  
        : base()  
    {  
    }  
  
    public ConsoleColor Color { get; set; }  
    public Activity Body { get; set; }  
  
    protected override void Execute(NativeActivityContext context)  
    {  
        context.Properties.Add(ConsoleColorProperty.Name, new ConsoleColorProperty(this.Color));  
  
        if (this.Body != null)  
        {  
            context.ScheduleActivity(this.Body);  
        }  
    }  
}  
```  
  
 当活动的主体开始一个工作脉冲时，会调用该属性的 <xref:System.Activities.IExecutionProperty.SetupWorkflowThread%2A> 方法，并且在完成该工作脉冲之后，调用 <xref:System.Activities.IExecutionProperty.CleanupWorkflowThread%2A>。 在本示例中，创建了一个工作流，该工作流使用具有三个分支的 <xref:System.Activities.Statements.Parallel> 活动。 前两个分支使用 `ConsoleColorScope` 活动，第三个分支不使用该活动。 所有这三个分支都包含两个 <xref:System.Activities.Statements.WriteLine> 活动和一个 <xref:System.Activities.Statements.Delay> 活动。 当执行 <xref:System.Activities.Statements.Parallel> 活动时，分支中包含的活动采用交错的方式执行，但是执行每个子活动时，会通过 `ConsoleColorProperty` 应用正确的控制台颜色。  
  
```csharp  
Activity wf = new Parallel  
{  
    Branches =
    {  
        new ConsoleColorScope  
        {  
            Color = ConsoleColor.Blue,  
            Body = new Sequence  
            {  
                Activities =
                {  
                    new WriteLine  
                    {  
                        Text = "Start blue text."  
                    },  
                    new Delay  
                    {  
                        Duration = TimeSpan.FromSeconds(1)  
                    },  
                    new WriteLine  
                    {  
                        Text = "End blue text."  
                    }  
                }  
            }  
        },  
        new ConsoleColorScope  
        {  
            Color = ConsoleColor.Red,  
            Body = new Sequence  
            {  
                Activities =
                {  
                    new WriteLine  
                    {  
                        Text = "Start red text."  
                    },  
                    new Delay  
                    {  
                        Duration = TimeSpan.FromSeconds(1)  
                    },  
                    new WriteLine  
                    {  
                        Text = "End red text."  
                    }  
                }  
            }  
        },  
        new Sequence  
        {  
            Activities =
            {  
                new WriteLine  
                {  
                    Text = "Start default text."  
                },  
                new Delay  
                {  
                    Duration = TimeSpan.FromSeconds(1)  
                },  
                new WriteLine  
                {  
                    Text = "End default text."  
                }  
            }  
        }  
    }  
};  
  
WorkflowInvoker.Invoke(wf);  
```  
  
 调用该工作流时，会将以下输出写入控制台窗口。  
  
```console  
Start blue text.  
Start red text.  
Start default text.  
End blue text.  
End red text.  
End default text.  
```  
  
> [!NOTE]
> 尽管没有显示在前面的输出中，但是会在控制台窗口中以指定颜色显示每个文本行。  
  
 工作流执行属性可由自定义活动创作者使用，并且这些属性还提供用于处理活动（如 <xref:System.ServiceModel.Activities.CorrelationScope> 和 <xref:System.Activities.Statements.TransactionScope> 活动）的管理的机制。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Activities.IExecutionProperty>
- <xref:System.Activities.IPropertyRegistrationCallback>
- <xref:System.Activities.RegistrationContext>
