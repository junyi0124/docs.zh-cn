---
title: C# 表达式
ms.date: 03/30/2017
ms.assetid: 29110be7-f4e3-407e-8dbe-78102eb21115
ms.openlocfilehash: b0e5d7f2fbb1f7b84c6d8f0110bd111165f0ea52
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96239609"
---
# <a name="c-expressions"></a>C# 表达式

从 .NET Framework 4.5 开始，Windows Workflow Foundation (WF) 支持 c # 表达式。 在 Visual Studio 2012 中创建的新 c # 工作流项目以 .NET Framework 4.5 使用 c # 表达式，Visual Basic 工作流项目使用 Visual Basic 表达式。 使用 Visual Basic 表达式的现有 .NET Framework 4 工作流项目可迁移到， [!INCLUDE[netfx_current_short](../../../includes/netfx-current-short-md.md)] 而不考虑项目语言和受支持。 本主题概述了 [!INCLUDE[wf1](../../../includes/wf1-md.md)] 中的 C# 表达式。

## <a name="using-c-expressions-in-workflows"></a>在工作流中使用 C# 表达式

- [在工作流设计器中使用 C# 表达式](csharp-expressions.md#WFDesigner)

  - [后向兼容性](csharp-expressions.md#BackwardCompat)

- [在代码工作流中使用 C# 表达式](csharp-expressions.md#CodeWorkflows)

- [在 XAML 工作流中使用 C# 表达式](csharp-expressions.md#XamlWorkflows)

  - [编译型 Xaml](csharp-expressions.md#CompiledXaml)

  - [松散 Xaml](csharp-expressions.md#LooseXaml)

- [在 XAMLX 工作流服务中使用 C# 表达式](csharp-expressions.md#WFServices)

### <a name="using-c-expressions-in-the-workflow-designer"></a><a name="WFDesigner"></a> 在工作流设计器中使用 c # 表达式

从 .NET Framework 4.5 开始，Windows Workflow Foundation (WF) 支持 c # 表达式。 在 Visual Studio 2012 中创建的 c # 工作流项目以 .NET Framework 4.5 使用 c # 表达式，而 Visual Basic 工作流项目使用 Visual Basic 表达式。 若要指定所需的 c # 表达式，请在标有 " **输入 c # 表达式**" 的框中键入。 该标签显示在属性窗口中（当在设计器中选中活动时），或显示在工作流设计器的活动上。 下例中，在 `WriteLine` 范围内，一个 `Sequence` 中包含了两个 `NoPersistScope` 活动。

![显示自动创建的序列活动的屏幕截图。](./media/csharp-expressions/auto-surround-sequence-activity.png)

> [!NOTE]
> C # 表达式仅在 Visual Studio 中受支持，并且在重新托管的工作流设计器中不受支持。 有关重新托管的设计器中支持的新 WF45 功能的详细信息，请参阅 [重新承载工作流设计器中对新 Workflow Foundation 4.5 功能的支持](wf-features-in-the-rehosted-workflow-designer.md)。

#### <a name="backwards-compatibility"></a><a name="BackwardCompat"></a> 向后兼容性

支持已迁移到的现有 .NET Framework 4 c # 工作流项目中的 Visual Basic 表达式 [!INCLUDE[netfx_current_short](../../../includes/netfx-current-short-md.md)] 。 当在工作流设计器中查看 Visual Basic 表达式时，除非 Visual Basic 表达式是有效的 c # 语法，否则现有 Visual Basic 表达式的文本将替换为 **在 XAML 中设置的值**。 如果 Visual Basic 表达式符合 C# 语法则予以显示。 若要将 Visual Basic 表达式更新为 C#，可在工作流设计器中编辑这些表达式，指定等效的 C# 表达式。 将 Visual Basic 表达式更新为 C# 不是必须的，但一旦在工作流设计器中进行更新，这些表达式即转换成 C# 并不可重新转换为 Visual Basic。

### <a name="using-c-expressions-in-code-workflows"></a><a name="CodeWorkflows"></a> 在代码工作流中使用 c # 表达式

基于 [!INCLUDE[netfx_current_short](../../../includes/netfx-current-short-md.md)] 代码的工作流支持 C# 表达式，但 C# 表达式须用 <xref:System.Activities.XamlIntegration.TextExpressionCompiler.Compile%2A?displayProperty=nameWithType> 进行编译，然后才能调用工作流。 工作流作者可以用 `CSharpValue` 表示表达式的右值，用 `CSharpReference` 表示表达式的左值。 在下例中，用一个 `Assign` 活动以及 `WriteLine` 活动中所包含的 `Sequence` 活动创建了一个工作流。 为 `CSharpReference` 的 `To` 自变量指定了一个 `Assign`，用来表示表达式的左值。 为 `CSharpValue` 的 `Value` 自变量和 `Assign` 的 `Text` 自变量指定了一个 `WriteLine`，用于表示这两个表达式的右值。

```csharp
Variable<int> n = new Variable<int>
{
    Name = "n"
};

Activity wf = new Sequence
{
    Variables = { n },
    Activities =
    {
        new Assign<int>
        {
            To = new CSharpReference<int>("n"),
            Value = new CSharpValue<int>("new Random().Next(1, 101)")
        },
        new WriteLine
        {
            Text = new CSharpValue<string>("\"The number is \" + n")
        }
    }
};

CompileExpressions(wf);

WorkflowInvoker.Invoke(wf);
```

工作流构造完毕后，通过调用 `CompileExpressions` 帮助器方法编译了 C# 表达式，随后调用工作流。 下面的示例为 `CompileExpressions` 方法。

```csharp
static void CompileExpressions(Activity activity)
{
    // activityName is the Namespace.Type of the activity that contains the
    // C# expressions.
    string activityName = activity.GetType().ToString();

    // Split activityName into Namespace and Type.Append _CompiledExpressionRoot to the type name
    // to represent the new type that represents the compiled expressions.
    // Take everything after the last . for the type name.
    string activityType = activityName.Split('.').Last() + "_CompiledExpressionRoot";
    // Take everything before the last . for the namespace.
    string activityNamespace = string.Join(".", activityName.Split('.').Reverse().Skip(1).Reverse());

    // Create a TextExpressionCompilerSettings.
    TextExpressionCompilerSettings settings = new TextExpressionCompilerSettings
    {
        Activity = activity,
        Language = "C#",
        ActivityName = activityType,
        ActivityNamespace = activityNamespace,
        RootNamespace = null,
        GenerateAsPartialClass = false,
        AlwaysGenerateSource = true,
        ForImplementation = false
    };

    // Compile the C# expression.
    TextExpressionCompilerResults results =
        new TextExpressionCompiler(settings).Compile();

    // Any compilation errors are contained in the CompilerMessages.
    if (results.HasErrors)
    {
        throw new Exception("Compilation failed.");
    }

    // Create an instance of the new compiled expression type.
    ICompiledExpressionRoot compiledExpressionRoot =
        Activator.CreateInstance(results.ResultType,
            new object[] { activity }) as ICompiledExpressionRoot;

    // Attach it to the activity.
    CompiledExpressionInvoker.SetCompiledExpressionRoot(
        activity, compiledExpressionRoot);
}
```

> [!NOTE]
> 如果 c # 表达式未编译，则在 <xref:System.NotSupportedException> 调用工作流时将引发，其中包含类似于以下内容的消息： `Expression Activity type 'CSharpValue` 1 "需要编译才能运行。  请确保已编译工作流。 "

如果基于自定义代码的工作流使用 `DynamicActivity`，则需要对 `CompileExpressions` 方法作一些修改，如下列代码示例所示。

```csharp
static void CompileExpressions(DynamicActivity dynamicActivity)
{
    // activityName is the Namespace.Type of the activity that contains the
    // C# expressions. For Dynamic Activities this can be retrieved using the
    // name property , which must be in the form Namespace.Type.
    string activityName = dynamicActivity.Name;

    // Split activityName into Namespace and Type.Append _CompiledExpressionRoot to the type name
    // to represent the new type that represents the compiled expressions.
    // Take everything after the last . for the type name.
    string activityType = activityName.Split('.').Last() + "_CompiledExpressionRoot";
    // Take everything before the last . for the namespace.
    string activityNamespace = string.Join(".", activityName.Split('.').Reverse().Skip(1).Reverse());

    // Create a TextExpressionCompilerSettings.
    TextExpressionCompilerSettings settings = new TextExpressionCompilerSettings
    {
        Activity = dynamicActivity,
        Language = "C#",
        ActivityName = activityType,
        ActivityNamespace = activityNamespace,
        RootNamespace = null,
        GenerateAsPartialClass = false,
        AlwaysGenerateSource = true,
        ForImplementation = true
    };

    // Compile the C# expression.
    TextExpressionCompilerResults results =
        new TextExpressionCompiler(settings).Compile();

    // Any compilation errors are contained in the CompilerMessages.
    if (results.HasErrors)
    {
        throw new Exception("Compilation failed.");
    }

    // Create an instance of the new compiled expression type.
    ICompiledExpressionRoot compiledExpressionRoot =
        Activator.CreateInstance(results.ResultType,
            new object[] { dynamicActivity }) as ICompiledExpressionRoot;

    // Attach it to the activity.
    CompiledExpressionInvoker.SetCompiledExpressionRootForImplementation(
        dynamicActivity, compiledExpressionRoot);
}
```

在动态活动中编译 C# 表达式的 `CompileExpressions` 重载中有几处不同。

- `CompileExpressions` 的参数是一个 `DynamicActivity`。

- 通过使用 `DynamicActivity.Name` 属性检索类型名称和命名空间。

- `TextExpressionCompilerSettings.ForImplementation` 设置为 `true`。

- 调用 `CompiledExpressionInvoker.SetCompiledExpressionRootForImplementation` 而非 `CompiledExpressionInvoker.SetCompiledExpressionRoot`。

有关在代码中使用表达式的详细信息，请参阅 [使用命令性代码创作工作流、活动和表达式](authoring-workflows-activities-and-expressions-using-imperative-code.md)。

### <a name="using-c-expressions-in-xaml-workflows"></a><a name="XamlWorkflows"></a> 在 XAML 工作流中使用 c # 表达式

XAML 工作流支持 C# 表达式。 编译型 XAML 工作流被编译到类型中，而宽松型 XAML 工作流由运行时加载，并在工作流执行时编译到活动树中。

- [编译型 Xaml](csharp-expressions.md#CompiledXaml)

- [松散 Xaml](csharp-expressions.md#LooseXaml)

#### <a name="compiled-xaml"></a><a name="CompiledXaml"></a> 已编译 Xaml

编译型 XAML 工作流支持 C# 表达式，此种工作流编译成类型，作为面向 [!INCLUDE[netfx_current_short](../../../includes/netfx-current-short-md.md)] 的 C# 工作流项目的组成部分。 在 Visual Studio 中，编译的 XAML 是工作流创作的默认类型，在 Visual Studio 中创建的 c # 工作流项目 [!INCLUDE[netfx_current_short](../../../includes/netfx-current-short-md.md)] 使用 c # 表达式。

#### <a name="loose-xaml"></a><a name="LooseXaml"></a> 松散 Xaml

宽松型 XAML 工作流支持 C# 表达式。 加载并调用宽松型 XAML 工作流的工作流宿主程序必须面向 [!INCLUDE[netfx_current_short](../../../includes/netfx-current-short-md.md)]，而<xref:System.Activities.XamlIntegration.ActivityXamlServicesSettings.CompileExpressions%2A> 必须设为 `true`（默认值为 `false`）。 要将 <xref:System.Activities.XamlIntegration.ActivityXamlServicesSettings.CompileExpressions%2A> 设置为 `true`，请创建一个 <xref:System.Activities.XamlIntegration.ActivityXamlServicesSettings> 实例，创建时将其 <xref:System.Activities.XamlIntegration.ActivityXamlServicesSettings.CompileExpressions%2A> 属性设为 `true`，并作为参数传递给 <xref:System.Activities.XamlIntegration.ActivityXamlServices.Load%2A?displayProperty=nameWithType>。 如果 `CompileExpressions` 未设置为 `true` ，则 <xref:System.NotSupportedException> 将引发，其中包含类似于以下内容的消息： `Expression Activity type 'CSharpValue` 1 "需要编译才能运行。  请确保已编译工作流。 "

```csharp
ActivityXamlServicesSettings settings = new ActivityXamlServicesSettings
{
    CompileExpressions = true
};

DynamicActivity<int> wf = ActivityXamlServices.Load(new StringReader(serializedAB), settings) as DynamicActivity<int>;
```

有关使用 XAML 工作流的详细信息，请参阅 [在 xaml 中序列化工作流和活动](serializing-workflows-and-activities-to-and-from-xaml.md)。

### <a name="using-c-expressions-in-xamlx-workflow-services"></a><a name="WFServices"></a> 在 .XAMLX 工作流服务中使用 c # 表达式

XAMLX 工作流服务支持 C# 表达式。 当工作流服务承载于 IIS 或 WAS 中时，无须执行任何额外步骤，但如果 XAML 工作流服务是自承载的，则 C# 表达式必须经过编译。 若要在自承载的 .XAMLX 工作流服务中编译 c # 表达式，请首先将 .XAMLX 文件加载到 `WorkflowService` 中，然后将 `Body` 的传递 `WorkflowService` 给 `CompileExpressions` 前面在 [代码工作流中使用 c # 表达式](csharp-expressions.md#CodeWorkflows) 部分中所述的方法。 下例加载一个 XAMLX 工作流服务，编译 C# 表达式，随后打开该工作流服务并等待请求。

```csharp
// Load the XAMLX workflow service.
WorkflowService workflow1 =
    (WorkflowService)XamlServices.Load(xamlxPath);

// Compile the C# expressions in the workflow by passing the Body to CompileExpressions.
CompileExpressions(workflow1.Body);

// Initialize the WorkflowServiceHost.
var host = new WorkflowServiceHost(workflow1, new Uri("http://localhost:8293/Service1.xamlx"));

// Enable Metadata publishing/
ServiceMetadataBehavior smb = new ServiceMetadataBehavior();
smb.HttpGetEnabled = true;
smb.MetadataExporter.PolicyVersion = PolicyVersion.Policy15;
host.Description.Behaviors.Add(smb);

// Open the WorkflowServiceHost and wait for requests.
host.Open();
Console.WriteLine("Press enter to quit");
Console.ReadLine();
```

如果 C# 表达式未编译，那么，虽然 `Open` 操作会成功执行，但工作流在调用时将失败。 下面的 `CompileExpressions` 方法与之前在 [代码工作流部分中使用 c # 表达式](csharp-expressions.md#CodeWorkflows) 的方法相同。

```csharp
static void CompileExpressions(Activity activity)
{
    // activityName is the Namespace.Type of the activity that contains the
    // C# expressions.
    string activityName = activity.GetType().ToString();

    // Split activityName into Namespace and Type.Append _CompiledExpressionRoot to the type name
    // to represent the new type that represents the compiled expressions.
    // Take everything after the last . for the type name.
    string activityType = activityName.Split('.').Last() + "_CompiledExpressionRoot";
    // Take everything before the last . for the namespace.
    string activityNamespace = string.Join(".", activityName.Split('.').Reverse().Skip(1).Reverse());

    // Create a TextExpressionCompilerSettings.
    TextExpressionCompilerSettings settings = new TextExpressionCompilerSettings
    {
        Activity = activity,
        Language = "C#",
        ActivityName = activityType,
        ActivityNamespace = activityNamespace,
        RootNamespace = null,
        GenerateAsPartialClass = false,
        AlwaysGenerateSource = true,
        ForImplementation = false
    };

    // Compile the C# expression.
    TextExpressionCompilerResults results =
        new TextExpressionCompiler(settings).Compile();

    // Any compilation errors are contained in the CompilerMessages.
    if (results.HasErrors)
    {
        throw new Exception("Compilation failed.");
    }

    // Create an instance of the new compiled expression type.
    ICompiledExpressionRoot compiledExpressionRoot =
        Activator.CreateInstance(results.ResultType,
            new object[] { activity }) as ICompiledExpressionRoot;

    // Attach it to the activity.
    CompiledExpressionInvoker.SetCompiledExpressionRoot(
        activity, compiledExpressionRoot);
}
```
