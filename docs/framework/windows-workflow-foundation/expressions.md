---
title: 表达式-WF
ms.date: 03/30/2017
ms.assetid: c42341a9-43a1-462c-bffb-c5de004aa428
ms.openlocfilehash: 1dca2090dda981fabb27d3e5f2dff78051d7af24
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96280216"
---
# <a name="expressions"></a>表达式

Windows Workflow Foundation (WF) 表达式是返回结果的任何活动。 从 <xref:System.Activities.Activity%601> 间接派生的所有表达式活动，其中包含名为 <xref:System.Activities.OutArgument> 的 <xref:System.Activities.Activity%601.Result%2A> 属性作为活动的返回值。 [!INCLUDE[wf1](../../../includes/wf1-md.md)] 随附了从简单活动（如 <xref:System.Activities.Expressions.VariableValue%601> 和 <xref:System.Activities.Expressions.VariableReference%601>，它们通过运算符活动提供对单个工作流变量的访问）到复杂活动（如 <xref:Microsoft.VisualBasic.Activities.VisualBasicReference%601> 和 <xref:Microsoft.VisualBasic.Activities.VisualBasicValue%601>，它们提供对 Visual Basic 语言的全面访问，以获得结果）的广泛表达式活动。 可以通过从 <xref:System.Activities.CodeActivity%601> 或 <xref:System.Activities.NativeActivity%601> 派生来创建其他表达式活动。

## <a name="using-expressions"></a>使用表达式

 工作流设计器对 Visual Basic 项目中的所有表达式使用 <xref:Microsoft.VisualBasic.Activities.VisualBasicValue%601> 和 <xref:Microsoft.VisualBasic.Activities.VisualBasicReference%601>，对 C# 工作流项目中的所有表达式使用 <xref:Microsoft.CSharp.Activities.CSharpValue%601> 和 <xref:Microsoft.CSharp.Activities.CSharpReference%601>。

> [!NOTE]
> .NET Framework 4.5 中引入了对工作流项目中的 c # 表达式的支持。 有关详细信息，请参阅 [c # 表达式](csharp-expressions.md)。

 设计器生成的工作流保存在 XAML 中，其中表达式位于方括号中，如以下示例所示。

```xml
<Sequence xmlns="http://schemas.microsoft.com/netfx/2009/xaml/activities" xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
  <Sequence.Variables>
    <Variable x:TypeArguments="x:Int32" Default="1" Name="a" />
    <Variable x:TypeArguments="x:Int32" Default="2" Name="b" />
    <Variable x:TypeArguments="x:Int32" Default="3" Name="c" />
    <Variable x:TypeArguments="x:Int32" Default="0" Name="r" />
  </Sequence.Variables>
  <Assign>
    <Assign.To>
      <OutArgument x:TypeArguments="x:Int32">[r]</OutArgument>
    </Assign.To>
    <Assign.Value>
      <InArgument x:TypeArguments="x:Int32">[a + b + c]</InArgument>
    </Assign.Value>
  </Assign>
</Sequence>
```

 在代码中定义工作流时，可以使用所有表达式活动。 下面的示例演示如何使用运算符活动的组合添加三个数字：

```csharp
Variable<int> a = new Variable<int>("a", 1);
Variable<int> b = new Variable<int>("b", 2);
Variable<int> c = new Variable<int>("c", 3);
Variable<int> r = new Variable<int>("r", 0);

Sequence w = new Sequence
{
    Variables = { a, b, c, r },
    Activities =
    {
        new Assign {
            To = new OutArgument<int>(r),
            Value = new InArgument<int> {
                Expression = new Add<int, int, int> {
                    Left = new Add<int, int, int> {
                        Left = new InArgument<int>(a),
                        Right = new InArgument<int>(b)
                    },
                    Right = new InArgument<int>(c)
                }
            }
        }
    }
};
```

 使用 c # lambda 表达式可以更简洁地地表示相同的工作流，如下面的示例中所示：
  
```csharp
Variable<int> a = new Variable<int>("a", 1);
Variable<int> b = new Variable<int>("b", 2);
Variable<int> c = new Variable<int>("c", 3);
Variable<int> r = new Variable<int>("r", 0);

Sequence w = new Sequence
{
    Variables = { a, b, c, r },
    Activities =
    {
        new Assign {
            To = new OutArgument<int>(r),
            Value = new InArgument<int>((ctx) => a.Get(ctx) + b.Get(ctx) + c.Get(ctx))
        }
    }
};
```

## <a name="extending-available-expressions-with-custom-expression-activities"></a>使用自定义表达式活动扩展可用的表达式

 可以扩展 [!INCLUDE[netfx_current_short](../../../includes/netfx-current-short-md.md)] 中的表达式，从而允许创建其他表达式活动。 下面的示例演示返回三个整数值之和的活动。

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Activities;

namespace ExpressionsDemo
{
    public sealed class AddThreeValues : CodeActivity<int>
    {
        public InArgument<int> Value1 { get; set; }
        public InArgument<int> Value2 { get; set; }
        public InArgument<int> Value3 { get; set; }

        protected override int Execute(CodeActivityContext context)
        {
            return Value1.Get(context) +
                   Value2.Get(context) +
                   Value3.Get(context);
        }
    }
}
```

 利用此新活动，你可以重写之前添加了三个值的工作流，如以下示例中所示：

```csharp
Variable<int> a = new Variable<int>("a", 1);
Variable<int> b = new Variable<int>("b", 2);
Variable<int> c = new Variable<int>("c", 3);
Variable<int> r = new Variable<int>("r", 0);

Sequence w = new Sequence
{
    Variables = { a, b, c, r },
    Activities =
    {
        new Assign {
            To = new OutArgument<int>(r),
            Value = new InArgument<int> {
                Expression = new AddThreeValues() {
                    Value1 = new InArgument<int>(a),
                    Value2 = new InArgument<int>(b),
                    Value3 = new InArgument<int>(c)
                }
            }
        }
    }
};
```

 有关在代码中使用表达式的详细信息，请参阅 [使用命令性代码创作工作流、活动和表达式](authoring-workflows-activities-and-expressions-using-imperative-code.md)。
