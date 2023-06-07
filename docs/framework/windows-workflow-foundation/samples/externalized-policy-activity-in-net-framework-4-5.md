---
title: .NET Framework 4.5 中的外部化策略活动
ms.date: 03/30/2017
ms.assetid: 92fd6f92-23a1-4adf-b96a-2754ea93ad3e
ms.openlocfilehash: 00b671f169696728610e8ee32f874b44fbff9e33
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90556910"
---
# <a name="externalized-policy-activity-in-net-framework-45"></a>.NET Framework 4.5 中的外部化策略活动

此示例演示了 ExternalizedPolicy4 活动如何 <xref:System.Workflow.Activities.Rules.RuleSet> [!INCLUDE[netfx_current_long](../../../../includes/netfx-current-long-md.md)] 通过使用在 wf 3.5 中随附的规则引擎直接执行 WINDOWS WORKFLOW FOUNDATION (WF 4.5) 中的现有 .NET Framework 3.5 WINDOWS WORKFLOW FOUNDATION (WF 3.5) 对象。 通过使用此活动，可以打开并执行任何现有 WF 3.5 <xref:System.Workflow.Activities.Rules.RuleSet>。 有关 Windows Workflow Foundation 中包含的 WF 3.5 规则引擎的详细信息，请阅读 [Windows Workflow Foundation 规则引擎简介](/previous-versions/dotnet/articles/aa480193(v=msdn.10))。 有关将规则迁移到中的详细信息 [!INCLUDE[wf1](../../../../includes/wf1-md.md)] [!INCLUDE[netfx_current_short](../../../../includes/netfx-current-short-md.md)] ，请参阅 [迁移指南](../migration-guidance.md)。

## <a name="projects-in-this-sample"></a>此示例中的项目

|项目名称|说明|主要文件|
|-|-|-|
|ExternalizedPolicy4|包含 ExternalizedPolicy4 活动及其 WF 4.5 设计器。|**ExternalizedPolicy4.cs**：活动定义。<br /><br /> **ExternalizedPolicy4Designer**： ExternalizedPolicy4 活动的自定义设计器。 它使用来自 WF 3.5 规则引擎的规则编辑器 (<xref:System.Workflow.Activities.Rules.Design.RuleSetDialog>)。|
|ImperativeCodeClientSample|一个示例客户端应用程序，它使用命令性 C# 代码（未使用设计器）配置和运行使用 ExternalizedPolicy4 应用程序的工作流。|**Applydiscount.rules**：带有 [!INCLUDE[wf1](../../../../includes/wf1-md.md)] 规则定义的文件。<br /><br /> **Order.cs**：表示客户订单的类型。 规则适用于此类型的对象。<br /><br /> **Program.cs**：配置和运行具有 Policy4 活动的工作流，以将 applydiscount.rules 中定义的规则应用到 Order 对象的实例。<br /><br /> App.config：带有规则文件的路径的配置文件。|
|DesignerClientSample|一个示例客户端应用程序，它在 [!INCLUDE[wf1](../../../../includes/wf1-md.md)] 设计器中配置和运行使用 ExternalPolicy4 应用程序的工作流。|**Sequence1.xaml**：使用 Policy4 活动执行规则评估的顺序工作流。<br /><br /> **Program.cs**：运行在 sequence1.xaml 中定义的工作流的实例。|

## <a name="the-externalizedpolicy4-activity"></a>ExternalizedPolicy4 活动

ExternalizedPolicy4 活动是一个 <xref:System.Activities.NativeActivity>，它允许在 WF 4.5 工作流中执行 WF 3.5 <xref:System.Workflow.Activities.Rules.RuleSet> 对象。 下面的示例是活动的公共对象模型的简化定义。

```csharp
public class ExternalizedPolicy4Activity<TResult>: CodeActivity
{
    public string RulesFilePath

    public string RuleSetName

    [RequiredArgument]
    public InArgument<T> TargetObject

    [RequiredArgument]
    public OutArgument<T> ResultObject

    public OutArgument<ValidationErrorCollection> ValidationErrors
}
```

|properties|说明|
|-|-|
|RuleSetFilePath|执行活动时要计算的 .NET Framework 3.5 <xref:System.Workflow.Activities.Rules.RuleSet> 文件的路径。|
|RuleSetName|要在 .rules 文件中使用的 <xref:System.Workflow.Activities.Rules.RuleSet> 的名称。|
|TargetObject|对其计算 <xref:System.Workflow.Activities.Rules.Rule> 中的 <xref:System.Workflow.Activities.Rules.RuleSet> 对象的对象。|
|ResultObject|应用规则后的结果对象（例如，对 Input 自变量应用规则并将结果存储在 Result 自变量中）。|
|ValidationError|在执行前对目标对象验证 <xref:System.Workflow.Activities.Rules.RuleSet> 时由 WF 3.5 规则引擎返回的验证错误的列表。|

## <a name="externalizedpolicy4-activity-designer"></a>ExternalizedPolicy4 活动设计器

利用 ExternalizedPolicy4 设计器，可以将活动配置为使用现有 RuleSet 而不编写代码。 仅设置 .rules 文件所在的路径并指定要使用的 <xref:System.Workflow.Activities.Rules.RuleSet> 名称。 还可以使用此设计器修改 <xref:System.Workflow.Activities.Rules.RuleSet>。 生成解决方案之后，可以在工具箱中的“Microsoft.Samples.Activities.Rules”部分找到此功能。  可以利用此设计器选择 .rules 文件和 <xref:System.Workflow.Activities.Rules.RuleSet>。 单击 " **编辑规则集** " 按钮时，将显示 WF 3.5 <xref:System.Workflow.Activities.Rules.Design.RuleSetDialog> 。 此对话框是重新承载的 WF 3.5 规则编辑器，它可用于查看和编辑 ExternalizedPolicy4 活动执行的规则。

## <a name="policy4-and-externalpolicy4"></a>Policy4 和 ExternalPolicy4

策略活动允许在 WF 4.5 工作流中创建和执行 .NET Framework 3.5 规则集。 <xref:System.Workflow.Activities.Rules.RuleSet> 是 Policy4 活动 XAML 定义中的序列化内联。 ExternalizedPolicy4 示例演示如何使用现有外部 <xref:System.Workflow.Activities.Rules.RuleSet>（包含在 .rules 文件中）。

## <a name="use-this-sample"></a>使用此示例

无需进行特殊设置即可运行此示例。 在 Visual Studio 中打开解决方案，然后按 **F5** 运行该应用程序。

此示例包含两个客户端应用程序，即 ImperativeCodeClientSample 和 DesignerClientSample。 ImperativeCodeClientSample 客户端演示如何使用 C# 命令性代码来配置和运行 ExternalizedPolicy4 活动。 DesignerClientSample 演示如何使用设计器配置和运行 ExternalizedPolicy4 活动。

### <a name="run-the-imperativecodeclientsample-application"></a>运行 ImperativeCodeClientSample 应用程序

1. 使用 Visual Studio 打开 *policy4sample.sln* 解决方案文件。

2. 在 **解决方案资源管理器**中，右键单击 **ImperativeCodeClientSample** 项目，然后选择 " **设为启动项目**"。

3. 若要运行项目，请按**Ctrl** + **F5**。

### <a name="run-the-designerclientsample-application"></a>运行 DesignerClientSample 应用程序

1. 使用 Visual Studio 打开 *policy4sample.sln* 解决方案文件。

2. 在 **解决方案资源管理器**中，右键单击 **DesignerClientSample** 项目，然后选择 " **设为启动项目**"。

3. 按**Ctrl** + **Shift** + **B**来编译项目。

4. 按**Ctrl** + **F5**运行项目。

> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> 如果此目录不存在，请参阅[Windows Communication Foundation (wcf) ，并 Windows Workflow Foundation (的 WF](https://www.microsoft.com/download/details.aspx?id=21459)) .NET Framework Windows Communication Foundation ([!INCLUDE[wf1](../../../../includes/wf1-md.md)]
>
> 此示例位于以下目录：
>
> `<InstallDrive>:\WF_WCF_Samples\WF\Scenario\ActivityLibrary\Rules-ExternalizedPolicy4`
