---
title: 使用 CacheMetadata 公开数据
ms.date: 03/30/2017
ms.assetid: 34832f23-e93b-40e6-a80b-606a855a00d9
ms.openlocfilehash: a044c896e56541ee954fc33853376eb8293c6ede
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2019
ms.locfileid: "61945699"
---
# <a name="exposing-data-with-cachemetadata"></a>使用 CacheMetadata 公开数据

在执行某活动之前，工作流运行时会获取所需的所有活动信息以保持活动的执行。 工作流运行时在 <xref:System.Activities.Activity.CacheMetadata%2A> 方法执行期间获取这些信息。 在执行此方法的时候，此方法的默认实现为运行时提供活动公开的所有公共参数、变量和子活动；如果活动需要向运行时提供比这更多的信息（如私有成员或将由该活动安排的活动），可以重写此方法以进行提供。

## <a name="default-cachemetadata-behavior"></a>默认 CacheMetadata 行为

<xref:System.Activities.NativeActivity.CacheMetadata%2A> 所派生活动的 <xref:System.Activities.NativeActivity> 的默认实现将通过以下方式来处理以下方法类型：

- <xref:System.Activities.InArgument%601><xref:System.Activities.OutArgument%601>，或<xref:System.Activities.InOutArgument%601>（泛型自变量）：这些自变量公开给运行时作为参数的名称，并键入等于公开的属性名称和类型、 相应的自变量方向和某些验证数据。

- <xref:System.Activities.Variable> 或其中的任何子类：这些成员作为公共变量公开给运行时。

- <xref:System.Activities.Activity> 或其中的任何子类：这些成员作为公共子活动公开给运行时。 可以通过调用显式实现默认行为<xref:System.Activities.ActivityMetadata.AddImportedChild%2A>、 传入子活动。

- <xref:System.Activities.ActivityDelegate> 或其中的任何子类：这些成员作为公共委托公开给运行时。

- <xref:System.Collections.ICollection> 类型的<xref:System.Activities.Variable>:集合中的所有元素都作为公共变量都公开给运行时。

- <xref:System.Collections.ICollection> 类型的<xref:System.Activities.Activity>:集合中的所有元素作为公共子活动都公开给运行时。

- <xref:System.Collections.ICollection> 类型的<xref:System.Activities.ActivityDelegate>:集合中的所有元素都作为公共委托都公开给运行时。

从 <xref:System.Activities.Activity.CacheMetadata%2A>、<xref:System.Activities.Activity> 和 <xref:System.Workflow.Activities.CodeActivity> 派生的活动 <xref:System.Activities.AsyncCodeActivity> 也能起到以上的作用，但有以下差异：

- 从 <xref:System.Activities.Activity> 派生的类不能安排子活动或委托，所以此类成员作为导入的子活动或委托公开。

- 从 <xref:System.Activities.CodeActivity> 和 <xref:System.Activities.AsyncCodeActivity> 派生的类不支持变量、子活动或委托，所以将只公开参数。

## <a name="overriding-cachemetadata-to-provide-information-to-the-runtime"></a>重写 CacheMetadata 以向运行时提供信息

以下代码段演示如何在 <xref:System.Activities.Activity.CacheMetadata%2A> 方法执行期间向活动的元数据添加有关成员的信息。 注意，调用了该方法的基类以缓存有关活动的所有公共数据。

```csharp
protected override void CacheMetadata(NativeActivityMetadata metadata)
{
    base.CacheMetadata(metadata);
    metadata.AddImplementationChild(this._writeLine);
    metadata.AddVariable(this._myVariable);
    metadata.AddImplementationVariable(this._myImplementationVariable);

    RuntimeArgument argument = new RuntimeArgument("MyArgument", ArgumentDirection.In, typeof(SomeType));
    metadata.Bind(argument, this.SomeName);
    metadata.AddArgument(argument);
}
```

## <a name="using-cachemetadata-to-expose-implementation-children"></a>使用 CacheMetadata 公开实现子活动

为了使用变量将数据传递给将由活动安排的子活动，有必要将变量添加为实现变量；不能用这种方式设置公共变量的值。 原因在于，活动将更有可能作为功能的实现（有参数）执行，而不是作为封装类（有属性）执行。 但是，在有些情况下，必须显式设置参数，如使用 <xref:System.Activities.NativeActivityContext.ScheduleActivity%2A> 时，因为安排的活动不能像子活动那样访问父活动的参数。

下面的代码段演示如何使用 <xref:System.Activities.Activity.CacheMetadata%2A> 将自变量从本机活动传入一个安排的活动。

```csharp
public sealed class ChildActivity : NativeActivity
{
    public WriteLine _writeLine;
    public InArgument<string> Message { get; set; }
    private Variable<string> MessageVariable { get; set; }
    public ChildActivity()
    {
        MessageVariable = new Variable<string>();
        _writeLine = new WriteLine
        {
            Text = new InArgument<string>(MessageVariable),
        };
    }
    protected override void CacheMetadata(NativeActivityMetadata metadata)
    {
        base.CacheMetadata(metadata);
        metadata.AddImplementationVariable(this.MessageVariable);
        metadata.AddImplementationChild(this._writeLine);
    }
    protected override void Execute(NativeActivityContext context)
    {
        string configuredMessage = context.GetValue(Message);
        context.SetValue(MessageVariable, configuredMessage);
        context.ScheduleActivity(this._writeLine);
    }
}
```
