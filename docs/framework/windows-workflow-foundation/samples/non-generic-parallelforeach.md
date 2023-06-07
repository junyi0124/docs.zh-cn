---
title: 非泛型 ParallelForEach
ms.date: 03/30/2017
ms.assetid: de17e7a2-257b-48b3-91a1-860e2e9bf6e6
ms.openlocfilehash: ea7f57b8812dca3dfcb4908730dd788182d50c5c
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/25/2019
ms.locfileid: "75347614"
---
# <a name="non-generic-parallelforeach"></a>非泛型 ParallelForEach

[!INCLUDE[netfx_current_long](../../../../includes/netfx-current-long-md.md)] 的工具箱中附带了一组控制流活动，其中包括可用来循环访问 <xref:System.Activities.Statements.ParallelForEach%601> 集合的 <xref:System.Collections.Generic.IEnumerable%601>。

<xref:System.Activities.Statements.ParallelForEach%601> 要求其 <xref:System.Activities.Statements.ParallelForEach%601.Values%2A> 属性为 <xref:System.Collections.Generic.IEnumerable%601>类型。 这将阻止用户循环访问实现 <xref:System.Collections.Generic.IEnumerable%601> 接口（例如，<xref:System.Collections.ArrayList>）的数据结构。 非泛型版本的 <xref:System.Activities.Statements.ParallelForEach%601> 没有这一需求，不过与此对应的代价是，需要更复杂的运行时来确保集合中的值类型的兼容性。

此示例演示如何实现非泛型的 <xref:System.Activities.Statements.ParallelForEach%601> 活动及其设计器。 此活动可用于循环访问 <xref:System.Collections.ArrayList>。

## <a name="parallelforeach-activity"></a>ParallelForEach 活动

C#/Visual Basic `foreach` 语句枚举集合中的元素，并对集合中的每个元素执行嵌入语句。 [!INCLUDE[wf1](../../../../includes/wf1-md.md)] 等效活动为 <xref:System.Activities.Statements.ForEach%601> 和 <xref:System.Activities.Statements.ParallelForEach%601>。 <xref:System.Activities.Statements.ForEach%601> 活动包含一个值列表和一个主体。 在运行时，将循环访问此列表，并对列表中的每个值执行主体。

<xref:System.Activities.Statements.ParallelForEach%601> 具有一个 <xref:System.Activities.Statements.ParallelForEach%601.CompletionCondition%2A>，以便在 <xref:System.Activities.Statements.ParallelForEach%601> 的计算返回 <xref:System.Activities.Statements.ParallelForEach%601.CompletionCondition%2A> 的情况下，`true` 活动能够及早完成。 在每次迭代完成之后，将计算 <xref:System.Activities.Statements.ParallelForEach%601.CompletionCondition%2A>。

对于大多数情况，此活动的泛型版本应为首选解决方案，因为它涵盖了应使用此活动的大多数方案，并且提供编译时类型检查。 非泛型版本可用于循环访问实现非泛型 <xref:System.Collections.IEnumerable> 接口的类型。

## <a name="class-definition"></a>类定义

下面的代码示例显示非泛型 `ParallelForEach` 活动的定义。

```csharp
[ContentProperty("Body")]
public class ParallelForEach : NativeActivity
{
    [RequiredArgument]
    [DefaultValue(null)]
    InArgument<IEnumerable> Values { get; set; }

    [DefaultValue(null)]
    [DependsOn("Values")]
    public Activity<bool> CompletionCondition
    [DefaultValue(null)]
    [DependsOn("CompletionCondition")]
    ActivityAction<object> Body { get; set; }
}
```

Body （可选） \
<xref:System.Activities.ActivityAction> 类型的 <xref:System.Object>，将对集合中的每个元素执行它。 通过单个元素的 Argument 属性将其传递到 Body。

值（可选） \
循环访问的元素集合。 在运行时确保所有集合元素为兼容类型。

CompletionCondition （可选） \
在任何迭代完成之后，将计算 <xref:System.Activities.Statements.ParallelForEach%601.CompletionCondition%2A> 属性。 如果其计算结果为 `true`，则取消已安排的挂起的迭代。 如果未设置此属性，则 Branches 集合中的所有活动都将执行，直至完成为止。

## <a name="example-of-using-parallelforeach"></a>使用 ParallelForEach 的示例

下面的代码演示如何在应用程序中使用 ParallelForEach 活动。

```csharp
string[] names = { "bill", "steve", "ray" };

DelegateInArgument<object> iterationVariable = new DelegateInArgument<object>() { Name = "iterationVariable" };

Activity sampleUsage =
    new ParallelForEach
    {
       Values = new InArgument<IEnumerable>(c=> names),
       Body = new ActivityAction<object>
       {
           Argument = iterationVariable,
           Handler = new WriteLine
           {
               Text = new InArgument<string>(env => string.Format("Hello {0}",                                                               iterationVariable.Get(env)))
           }
       }
   };
```

## <a name="parallelforeach-designer"></a>ParallelForEach 设计器

此示例的活动设计器的外观与为内置 <xref:System.Activities.Statements.ParallelForEach%601> 活动提供的设计器的外观相似。 设计器将显示在工具箱中的 "**示例**"、"**非泛型" 活动**类别。 设计器在 "工具箱" 中名为 " **ParallelForEachWithBodyFactory** "，因为活动在工具箱中公开了一个 <xref:System.Activities.Presentation.IActivityTemplateFactory>，该活动使用正确配置的 <xref:System.Activities.ActivityAction>创建活动。

```csharp
public sealed class ParallelForEachWithBodyFactory : IActivityTemplateFactory
{
    public Activity Create(DependencyObject target)
    {
        return new Microsoft.Samples.Activities.Statements.ParallelForEach()
        {
            Body = new ActivityAction<object>()
            {
                Argument = new DelegateInArgument<object>()
                {
                    Name = "item"
                }
            }
        };
    }
}
```

## <a name="to-run-the-sample"></a>运行示例

1. 将您选择的项目设置为解决方案的启动项目。

    1. **CodeTestClient**演示如何使用代码使用活动。

    2. **DesignerTestClient**演示如何在设计器中使用活动。

2. 生成并运行该项目。

> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> 如果此目录不存在，请参阅[.NET Framework 4 的 Windows Communication Foundation （wcf）和 Windows Workflow Foundation （WF）示例](https://www.microsoft.com/download/details.aspx?id=21459)以下载所有 WINDOWS COMMUNICATION FOUNDATION （wcf）和 [!INCLUDE[wf1](../../../../includes/wf1-md.md)] 示例。 此示例位于以下目录：
>
> `<InstallDrive>:\WF_WCF_Samples\WF\Scenario\ActivityLibrary\NonGenericParallelForEach`
