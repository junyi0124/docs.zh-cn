---
title: 将工作流和活动序列化为 XAML 和从 XAML 序列化工作流和活动
description: 本文概述了如何在 Workflow Foundation 中序列化工作流定义以及如何使用 XAML 工作流定义。
ms.date: 03/30/2017
ms.assetid: 37685b32-24e3-4d72-88d8-45d5fcc49ec2
ms.openlocfilehash: 168dbc9a36cfa80c15fddc7481e986d1ce383adb
ms.sourcegitcommit: 9a4488a3625866335e83a20da5e9c5286b1f034c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2020
ms.locfileid: "83421340"
---
# <a name="serialize-workflows-and-activities-to-and-from-xaml"></a>在 XAML 中序列化工作流和活动

除了可将工作流定义编译为程序集中包含的类型之外，还可将工作流定义序列化为 XAML。 这些已序列化的定义可以重新加载以供编辑或检测，可以传递给生成系统以供编译，也可以加载并调用。 本主题概述如何序列化工作流定义以及如何使用 XAML 工作流定义。

## <a name="work-with-xaml-workflow-definitions"></a>使用 XAML 工作流定义

若要创建工作流定义以进行序列化，请使用 <xref:System.Activities.ActivityBuilder> 类。 <xref:System.Activities.ActivityBuilder> 的创建方式与 <xref:System.Activities.DynamicActivity> 的创建方式极其类似。 指定任何所需的参数，并配置构成行为的活动。 在下面的示例中，将要创建 `Add` 活动，此活动使用两个输入参数并将它们相加，然后返回结果。 由于此活动返回一个结果，因此将使用泛型 <xref:System.Activities.ActivityBuilder%601> 类。

[!code-csharp[CFX_WorkflowApplicationExample#41](~/samples/snippets/csharp/VS_Snippets_CFX/cfx_workflowapplicationexample/cs/program.cs#41)]

每个 <xref:System.Activities.DynamicActivityProperty> 实例均表示工作流的一个输入自变量，并且 <xref:System.Activities.ActivityBuilder.Implementation%2A> 包含构成工作流逻辑的活动。 请注意，此示例中的右值表达式是 Visual Basic 表达式。 Lambda 表达式不可序列化为 XAML，除非使用 <xref:System.Activities.Expressions.ExpressionServices.Convert%2A>。 如果要在工作流设计器中打开或编辑序列化的工作流，则应使用 Visual Basic 表达式。 有关详细信息，请参阅[使用命令性代码创作工作流、活动和表达式](authoring-workflows-activities-and-expressions-using-imperative-code.md)。

若要将由 <xref:System.Activities.ActivityBuilder> 实例表示的工作流定义序列化为 XAML，应使用 <xref:System.Activities.XamlIntegration.ActivityXamlServices> 创建 <xref:System.Xaml.XamlWriter>，然后通过 <xref:System.Xaml.XamlServices>，使用 <xref:System.Xaml.XamlWriter> 对工作流定义进行序列化。 <xref:System.Activities.XamlIntegration.ActivityXamlServices>具有一些方法，用于将 <xref:System.Activities.ActivityBuilder> 实例映射到 xaml 和从 xaml 映射，以及加载 xaml 工作流以及返回 <xref:System.Activities.DynamicActivity> 可调用的。 在下面的示例中， <xref:System.Activities.ActivityBuilder> 上一示例中的实例序列化为字符串并保存到文件中。

```csharp
// Serialize the workflow to XAML and store it in a string.
StringBuilder sb = new StringBuilder();
StringWriter tw = new StringWriter(sb);
XamlWriter xw = ActivityXamlServices.CreateBuilderWriter(new XamlXmlWriter(tw, new XamlSchemaContext()));
XamlServices.Save(xw, ab);
string serializedAB = sb.ToString();

// Display the XAML to the console.
Console.WriteLine(serializedAB);

// Serialize the workflow to XAML and save it to a file.
StreamWriter sw = File.CreateText(@"C:\Workflows\add.xaml");
XamlWriter xw2 = ActivityXamlServices.CreateBuilderWriter(new XamlXmlWriter(sw, new XamlSchemaContext()));
XamlServices.Save(xw2, ab);
sw.Close();
```

下面的示例表示序列化的工作流。

```xaml
<Activity
  x:TypeArguments="x:Int32"
  x:Class="Add"
  xmlns="http://schemas.microsoft.com/netfx/2009/xaml/activities"
  xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
  <x:Members>
    <x:Property Name="Operand1" Type="InArgument(x:Int32)" />
    <x:Property Name="Operand2" Type="InArgument(x:Int32)" />
  </x:Members>
  <Sequence>
    <WriteLine Text="[Operand1.ToString() + " + " + Operand2.ToString()]" />
    <Assign x:TypeArguments="x:Int32" Value="[Operand1 + Operand2]">
      <Assign.To>
        <OutArgument x:TypeArguments="x:Int32">
          <ArgumentReference x:TypeArguments="x:Int32" ArgumentName="Result" />
          </OutArgument>
      </Assign.To>
    </Assign>
  </Sequence>
</Activity>
```

若要加载序列化的工作流，请使用 <xref:System.Activities.XamlIntegration.ActivityXamlServices> <xref:System.Activities.XamlIntegration.ActivityXamlServices.Load%2A> 方法。 这将使用序列化的工作流定义，并返回一个表示该工作流定义的 <xref:System.Activities.DynamicActivity>。 请注意，在验证过程中对 <xref:System.Activities.Activity.CacheMetadata%2A> 的正文调用 <xref:System.Activities.DynamicActivity> 之前，不对 XAML 进行反序列化。 如果未显式调用验证，则在调用工作流时执行验证。 如果 XAML 工作流定义无效，则会引发 <xref:System.ArgumentException> 异常。 从 <xref:System.Activities.Activity.CacheMetadata%2A> 引发的所有异常均不会导致调用 <xref:System.Activities.Validation.ActivityValidationServices.Validate%2A>，必须由调用方进行处理。 在下面的示例中，将使用 <xref:System.Activities.WorkflowInvoker> 加载并调用上一示例中的序列化的工作流。

[!code-csharp[CFX_WorkflowApplicationExample#43](~/samples/snippets/csharp/VS_Snippets_CFX/cfx_workflowapplicationexample/cs/program.cs#43)]

调用该工作流时，会将以下输出显示到控制台。

**25 + 15**\
**40**

> [!NOTE]
> 有关使用输入参数和输出参数调用工作流的详细信息，请参阅[使用 WorkflowInvoker 和 WorkflowApplication](using-workflowinvoker-and-workflowapplication.md)和 <xref:System.Activities.WorkflowInvoker.Invoke%2A> 。

如果序列化的工作流包含 c # 表达式，则将 <xref:System.Activities.XamlIntegration.ActivityXamlServicesSettings> 其 <xref:System.Activities.XamlIntegration.ActivityXamlServicesSettings.CompileExpressions%2A> 属性设置为的实例 `true` 必须作为参数传递给 <xref:System.Activities.XamlIntegration.ActivityXamlServices.Load%2A?displayProperty=nameWithType> ，否则， <xref:System.NotSupportedException> 将引发，其中包含类似于以下内容的消息： **Expression Activity 类型 "CSharpValue" 1 "需要编译才能运行。 请确保已编译工作流。**

```csharp
ActivityXamlServicesSettings settings = new ActivityXamlServicesSettings
{
    CompileExpressions = true
};

DynamicActivity<int> wf = ActivityXamlServices.Load(new StringReader(serializedAB), settings) as DynamicActivity<int>;
```

有关详细信息，请参阅[c # 表达式](csharp-expressions.md)。

还可以使用方法将序列化的工作流定义加载到 <xref:System.Activities.ActivityBuilder> 实例中 <xref:System.Activities.XamlIntegration.ActivityXamlServices> <xref:System.Activities.XamlIntegration.ActivityXamlServices.CreateBuilderReader%2A> 。 将序列化的工作流加载到 <xref:System.Activities.ActivityBuilder> 实例中后，可以对其进行检查和修改。 这对自定义工作流设计器作者很有用，并将提供用于在设计过程中保存和重新加载工作流定义的机制。 在下面的示例中，将加载上一示例中的序列化的工作流定义并检查其属性。

[!code-csharp[CFX_WorkflowApplicationExample#44](~/samples/snippets/csharp/VS_Snippets_CFX/cfx_workflowapplicationexample/cs/program.cs#44)]
