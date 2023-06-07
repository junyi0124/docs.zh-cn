---
title: .NET Framework 4.5 中 Windows Workflow Foundation 的新增功能
description: .NET Framework 4.5 中的 Windows Workflow Foundation 引入了许多新功能，例如新的活动、设计器功能和工作流开发模型。
ms.date: 03/30/2017
ms.assetid: 195c43a8-e0a8-43d9-aead-d65a9e6751ec
ms.openlocfilehash: 0d7086fd5ff9bfa410568a74092be3e29f732e23
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95697904"
---
# <a name="whats-new-in-windows-workflow-foundation-in-net-framework-45"></a>.NET Framework 4.5 中 Windows Workflow Foundation 的新增功能

.NET Framework 4.5 (WF) Windows Workflow Foundation 引入了许多新功能，例如新的活动、设计器功能和工作流开发模型。 在重新托管的工作流设计器中支持 .NET Framework 4.5 中引入的许多（但不是全部）新工作流功能。 有关支持的新功能的详细信息，请参阅 [重新承载工作流设计器中对新 Workflow Foundation 4.5 功能的支持](wf-features-in-the-rehosted-workflow-designer.md)。 有关迁移 .NET Framework 3.0 和 .NET Framework 3.5 工作流应用程序以使用最新版本的详细信息，请参阅 [迁移指南](migration-guidance.md)。 本文概述了 .NET Framework 4.5 中引入的新工作流功能。

> [!WARNING]
> .NET Framework 4.5 中引入的新 Windows Workflow Foundation 功能对于面向以前版本 Framework 的项目不可用。 如果面向 .NET Framework 4.5 的项目将重定目标为以前版本的框架，则可能会出现几个问题。
>
> - 在设计器中，c # 表达式将替换为 **XAML** 中的消息值。
> - 将发生许多生成错误，包括以下错误。
>
> **文件格式与当前目标框架不兼容。若要转换文件格式，请显式保存文件。保存文件并重新打开设计器后，此错误消息将消失。**

## <a name="workflow-versioning"></a><a name="BKMK_Versioning"></a> 工作流版本控制

.NET Framework 4.5 基于新类引入了几个新的版本控制功能 <xref:System.Activities.WorkflowIdentity> 。 <xref:System.Activities.WorkflowIdentity> 为工作流应用程序提供了一种通过定义来映射持久化工作流实例的机制。

- 使用 <xref:System.Activities.WorkflowApplication> 承载的开发人员可以使用 <xref:System.Activities.WorkflowIdentity> 来启用并行承载多个版本的工作流。 可以使用新的 <xref:System.Activities.WorkflowApplicationInstance> 类来加载持久化工作流实例，然后，主机可以使用 <xref:System.Activities.WorkflowApplicationInstance.DefinitionIdentity%2A> 在实例化 <xref:System.Activities.WorkflowApplication> 时提供工作流定义的正确版本。 有关详细信息，请参阅 [使用 WorkflowIdentity 和版本控制](using-workflowidentity-and-versioning.md) 和 [如何：并行承载多个版本的工作流](how-to-host-multiple-versions-of-a-workflow-side-by-side.md)。

- <xref:System.ServiceModel.WorkflowServiceHost> 现在是多版本主机。 在部署工作流服务的新版本时，将使用新的服务创建新实例，但会使用以前的版本来完成现有实例。 有关详细信息，请参阅 [WorkflowServiceHost 中的并行版本控制](../wcf/feature-details/side-by-side-versioning-in-workflowservicehost.md)。

- 引入了动态更新，其提供了一种更新持久化工作流实例定义的机制。 有关详细信息，请参阅 [动态更新](dynamic-update.md) 和 [如何：更新正在运行的工作流实例的定义](how-to-update-the-definition-of-a-running-workflow-instance.md)。

- 提供 Sqlworkflowinstancestoreschemaupgrade.sql 数据库脚本来升级使用 .NET Framework 4 数据库脚本创建的持久性数据库。 此脚本更新 .NET Framework 4 持久性数据库，以支持 .NET Framework 4.5 中引入的新版本控制功能。 数据库中的持久化工作流实例将获得默认的版本控制值，然后就可以参与并行执行和动态更新。 有关详细信息，请参阅 [升级 .NET Framework 4 持久性数据库以支持工作流版本控制](using-workflowidentity-and-versioning.md#UpdatingWF4PersistenceDatabases)。

## <a name="activities"></a><a name="BKMK_NewActivities"></a> Activity

内置活动库包含新活动和现有活动的新功能。

### <a name="nopersist-scope"></a><a name="BKMK_NoPersistScope"></a> NoPersist 范围

<xref:System.Activities.Statements.NoPersistScope> 是一个在执行 NoPersistScope 的子活动时防止工作流持久保存的新容器活动。 此活动在工作流不适合持久保存的方案中十分有用，例如，当工作流使用计算机特定资源（如文件句柄）时或在执行数据库事务期间。 以前，为防止在活动执行期间出现持久性，需要一个使用 <xref:System.Activities.NativeActivity> 的自定义 <xref:System.Activities.NoPersistHandle>。

### <a name="new-flowchart-capabilities"></a><a name="BKMK_NewFlowchartCapabilities"></a> 新的流程图功能

流程图针对 .NET Framework 4.5 进行了更新，并具有以下新功能：

- `DisplayName` 或 <xref:System.Activities.Statements.FlowSwitch%601> 活动的 <xref:System.Activities.Statements.FlowDecision> 属性是可编辑的。 这样，活动设计器就可以显示有关该活动的用途的详细信息。

- 流程图具有一个名为 <xref:System.Activities.Statements.Flowchart.ValidateUnconnectedNodes%2A> 的新属性；此属性的默认值为 `False`。 如果此属性设置为 `True`，则未连接的流程图节点将产生验证错误。

## <a name="support-for-partial-trust"></a>支持部分信任

.NET Framework 4 的工作流需要完全受信任的应用程序域。 在 .NET Framework 4.5 中，工作流可在部分信任环境中运行。 在部分信任环境中，可以使用第三方组件而无需为其授予主机资源的完全访问权限。 关于在部分信任中运行的工作流，请注意以下几点：

1. 在部分信任下，不支持使用 <xref:System.Activities.Statements.Interop> 活动中包含的旧组件（包括规则）。

2. 不支持 <xref:System.ServiceModel.WorkflowServiceHost> 中在部分信任下运行的工作流。

3. 在部分信任方案中持久保留异常是一种潜在安全威胁。 若要禁用异常的持久保留，必须将扩展类型 <xref:System.Activities.ExceptionPersistenceExtension> 添加至项目，以便取消持久化异常。 下面的代码示例演示如何实现此类型。

    ```csharp
    public class ExceptionPersistenceExtension
    {
        public ExceptionPersistenceExtension()
        {
            this.PersistExceptions = false;
        }
        public bool PersistExceptions { get; set; }
    }
    ```

     如果无需对异常进行序列化，请确保在 <xref:System.Activities.Statements.NoPersistScope> 内使用异常。

4. 活动作者应重写 <xref:System.Activities.Activity.CacheMetadata%2A> 以避免让工作流运行时针对该类型自动执行反射。 参数和子活动必须为非 null，并且 <xref:System.Activities.ActivityMetadata.Bind%2A> 必须被显式调用。 有关重写的详细信息 <xref:System.Activities.Activity.CacheMetadata%2A> ，请参阅 [用 CacheMetadata 公开数据](exposing-data-with-cachemetadata.md)。 此外， `internal` 必须在中显式创建作为或 **私有** 类型的参数的实例，  <xref:System.Activities.Activity.CacheMetadata%2A> 以避免通过反射进行创建。

5. 这些类型不会使用 <xref:System.Runtime.Serialization.ISerializable> 或 <xref:System.SerializableAttribute> 来进行序列化；要进行序列化的类型必须支持 <xref:System.Runtime.Serialization.DataContractSerializer>。

6. 使用 <xref:System.Activities.Expressions.LambdaValue%601> 的表达式需要 <xref:System.Security.Permissions.ReflectionPermissionAttribute.RestrictedMemberAccess%2A>，因此不能在部分信任下使用。 使用 <xref:System.Activities.Expressions.LambdaValue%601> 的工作流应将这些表达式替换为派生自 <xref:System.Activities.CodeActivity%601> 的活动。 .

7. 在部分信任下，不能使用 <xref:System.Activities.XamlIntegration.TextExpressionCompiler> 或 Visual Basic 承载的编译器来编译表达式，但是可以运行以前编译的表达式。

8. 使用 [2 级透明度](../misc/security-transparent-code-level-2.md) 的单个程序集不能在 .NET Framework 4、 [!INCLUDE[netfx_current_short](../../../includes/netfx-current-short-md.md)] 完全信任和部分信任模式下使用 [!INCLUDE[netfx_current_short](../../../includes/netfx-current-short-md.md)] 。

## <a name="new-designer-capabilities"></a><a name="BKMK_NewDesignerCapabilites"></a> 新设计器功能

### <a name="designer-search"></a><a name="BKMK_DesignerSearch"></a> 设计器搜索

为了使较大的工作流程更易于管理，现在可以按关键字来搜索工作流。 此功能仅在 Visual Studio 中提供;此功能在重新承载设计器中不可用。 提供了两种搜索：

- "快速查找"，通过 **Ctrl + F** 或 " **编辑**"、" **查找和替换**"、" **快速查找**" 启动。

- "在文件中查找"，通过按 **Ctrl + Shift + F** 或 " **编辑**"、" **查找和替换**"、" **在文件中查找**" 启动。

请注意，不支持替换。

#### <a name="quick-find"></a><a name="BKMK_QuickFind"></a> 快速查找

在工作流中搜索的关键字将匹配下列设计器项：

- <xref:System.Activities.Activity> 对象、<xref:System.Activities.Statements.FlowNode> 对象、<xref:System.Activities.Statements.State> 对象、<xref:System.Activities.Statements.Transition> 对象以及其他自定义流控制项的属性。

- 变量

- 自变量

- 表达式

在设计器上的 <xref:System.Activities.Presentation.Model.ModelItem> 树上执行快速查找。 快速查找不会查找在工作流定义中导入的命名空间。

#### <a name="find-in-files"></a><a name="BKMK_FindInFiles"></a> 在文件中查找

在工作流中搜索的关键字将匹配工作流文件的实际内容。 搜索结果将显示在 Visual Studio 查找结果视图窗格中。 双击结果项将会定位到包含工作流设计器中的匹配项的活动。

### <a name="delete-context-menu-item-in-variable-and-argument-designer"></a><a name="BKMK_VariableDeleteContextMenu"></a> 删除变量和参数设计器中的上下文菜单项

在 .NET Framework 4 中，只能使用键盘在设计器中删除变量和参数。 从 .NET Framework 4.5 开始，可以使用上下文菜单来删除变量和参数。

以下屏幕快照显示了变量和自变量设计器的上下文菜单。

![变量和参数设计器上下文菜单](./media/whats-new-in-wf-in-dotnet/designer-context-menu.png)

### <a name="auto-surround-with-sequence"></a><a name="BKMK_AutoSurround"></a> 序列自动环绕

由于工作流或特定容器活动（如 <xref:System.Activities.Statements.NoPersistScope>）只能包含单个主体活动，因此添加第二个活动需要开发人员删除第一个活动，请添加一个 <xref:System.Activities.Statements.Sequence> 活动，然后将两个活动都添加到该顺序活动中。 从 .NET Framework 4.5 开始，在将第二个活动添加到设计器图面时， `Sequence` 将自动创建一个活动来包装这两个活动。

以下屏幕快照显示了 `WriteLine` 的 `Body` 中的 `NoPersistScope` 活动。

![NoPersistScope 活动正文中的 WriteLine 活动。](./media/whats-new-in-wf-in-dotnet/auto-surround-write-line-activity.png)

以下屏幕快照显示了在第一个 `Sequence` 下面丢弃第二个时在 `Body` 中自动创建的 `WriteLine` 活动。

![在 NoPersistScope 的正文中自动创建的序列。](./media/whats-new-in-wf-in-dotnet/auto-surround-sequence-activity.png)

### <a name="pan-mode"></a><a name="BKMK_PanMode"></a> 平移模式

若要更轻松地在设计器中浏览大型工作流，可以启用平移模式；通过此模式，开发人员可以单击并拖动工作流的可见部分将其移动，而无需使用滚动条。 用于激活平移模式的按钮位于设计器的右下角。

下面的屏幕快照显示了位于工作流设计器右下角的平移按钮。

![工作流设计器中突出显示的 "平移" 按钮。](./media/whats-new-in-wf-in-dotnet/pan-button-workflow-designer.png)

也可以使用鼠标中键或空格键来平移工作流设计器。

### <a name="multi-select"></a><a name="BKMK_MultiSelect"></a> 多重选择

通过拖动围绕活动的矩形（未启用平移模式时）或通过按住 Ctrl 键并逐一单击所需的活动，可以一次选择多个活动。

也可以在设计器中拖动并放置多个选择的活动，并使用上下文菜单与这些活动交互。

### <a name="outline-view-of-workflow-items"></a><a name="BKMK_DocumentOutline"></a> 工作流项的大纲视图

为了更加方便地浏览分层工作流，工作流的组件显示在树样式的大纲视图中。 大纲视图显示在 " **文档大纲** " 视图中。 若要打开此视图，请从顶部菜单中选择 " **视图**"、" **其他窗口**"、" **文档大纲**"，或按 Ctrl W、U。 单击大纲视图中的节点将会定位到工作流设计器中的相应活动，并且该大纲视图将会更新以显示在设计器中选择的活动。

[入门教程](getting-started-tutorial.md)中的已完成工作流的以下屏幕截图显示了包含顺序工作流的大纲视图。

![使用 Visual Studio 中的顺序工作流的大纲视图的屏幕截图。](./media/whats-new-in-wf-in-dotnet/outline-view-in-workflow-designer.jpg)

### <a name="c-expressions"></a><a name="BKMK_CSharpExpressions"></a> C # 表达式

在 .NET Framework 4.5 之前，工作流中的所有表达式只能用 Visual Basic 编写。 在 .NET Framework 4.5 中，Visual Basic 表达式仅用于使用 Visual Basic 创建的项目。 Visual C# 项目现在将 C# 用于表达式。 提供了功能完全的 C# 表达式编辑器，包括语法突出显示和 Intellisense 等功能。 在使用 Visual Basic 表达式的以前版本中创建的 C# 工作流项目仍可继续使用。

将在设计时对 C# 表达式进行验证。 C# 表达式中的错误将标有红色波浪形下划线。

有关 c # 表达式的详细信息，请参阅 [c # 表达式](csharp-expressions.md)。

### <a name="more-control-of-visibility-of-shell-bar-and-header-items"></a><a name="BKMK_Visibility"></a> 更好地控制 shell 栏和标题项的可见性

在重新承载的设计器中，某些标准 UI 控件可能对于给定的工作流没有意义，并可能已关闭。 在 .NET Framework 4 中，仅在设计器底部的 shell 栏支持此自定义项。 在 .NET Framework 4.5 中，可以通过设置适当的值来调整设计器顶部 shell 标头项的可见性 <xref:System.Activities.Presentation.View.DesignerView.WorkflowShellHeaderItemsVisibility%2A> <xref:System.Activities.Presentation.View.ShellHeaderItemsVisibility> 。

### <a name="auto-connect-and-auto-insert-in-flowchart-and-state-machine-workflows"></a><a name="BKMK_AutoConnect"></a> 流程图和状态机工作流中的自动连接和自动插入

在 .NET Framework 4 中，必须手动添加流程图工作流中节点之间的连接。 在 .NET Framework 4.5 中，流程图和状态机节点具有在将活动从工具箱拖到设计器图面时变为可见的自动连接点。 将活动放置在这些点中的一个点上会自动添加该活动以及必要的连接。

下面的屏幕快照显示了从工具箱中拖动活动时变为可见的附属点。

![显示自动连接点的流程图开始节点](./media/whats-new-in-wf-in-dotnet/auto-connect-points-start-node.png)

也可以将活动拖动到流程图节点和状态之间的连接上，以在两个其他节点之间自动插入该节点。 以下屏幕快照显示了突出显示的连接线，可在此连接线处从工具箱拖动并放置活动。

![放置活动的自动插入处理](./media/whats-new-in-wf-in-dotnet/auto-insert-connecting-line.png)

### <a name="designer-annotations"></a><a name="BKMK_Annotations"></a> 设计器批注

为促进开发较大型的工作流，设计器现在支持添加批注以帮助跟踪设计过程。 可以向活动、状态、流程图节点、变量和自变量添加批注。 以下屏幕快照显示了用于将批注添加到设计器的上下文菜单。

![显示用于添加批注的菜单的屏幕截图。](./media/whats-new-in-wf-in-dotnet/designer-annotations-context-menu.png)

### <a name="debugging-states"></a>调试状态

在 .NET Framework 4 中，非活动元素不支持调试断点，因为它们不是执行单元。 此版本提供了一种向 <xref:System.Activities.Statements.State> 对象添加断点的机制。 若在 <xref:System.Activities.Statements.State> 上设置了断点，则执行将在计划了状态入口活动或触发器之前、状态发生转换时中断。

### <a name="define-and-consume-activitydelegate-objects-in-the-designer"></a><a name="BKMK_ActivityDelegates"></a> 在设计器中定义和使用 ActivityDelegate 对象

.NET Framework 4 中的活动使用 <xref:System.Activities.ActivityDelegate> 对象公开工作流的其他部分可与工作流的执行交互的执行点，但使用这些执行点通常需要大量的代码。 在此版本中，开发人员可以使用工作流设计器来定义和使用活动委托。 有关详细信息，请参阅 [如何：在工作流设计器中定义和使用活动委托](/visualstudio/workflow-designer/how-to-define-and-consume-activity-delegates-in-the-workflow-designer)。

### <a name="build-time-validation"></a><a name="BKMK_BuildTimeValidation"></a> 生成时验证

在 .NET Framework 4 中，工作流验证错误在工作流项目的生成过程中不计为生成错误。 这意味着即使存在工作流验证错误，工作流项目的生成也可能成功。 在 .NET Framework 4.5 中，工作流验证错误将导致生成失败。

### <a name="design-time-background-validation"></a><a name="BKMK_DesignTimeValidation"></a> 设计时后台验证

在 .NET Framework 4 中，工作流被验证为前台进程，这可能会在复杂或耗时的验证过程中阻止 UI。 工作流验证现在发生在后台线程上，因此不会阻止 UI。

### <a name="view-state-located-in-a-separate-location-in-xaml-files"></a><a name="BKMK_ViewState"></a> 位于 XAML 文件中单独位置的视图状态

在 .NET Framework 4 中，工作流的视图状态信息存储在多个不同位置的 XAML 文件中。 这对于想直接读取 XAML 或编写用于删除视图状态信息的代码的开发人员来说很不方便。 在 .NET Framework 4.5 中，XAML 文件中的视图状态信息序列化为 XAML 文件中的单独元素。 开发人员可以轻松地查找和编辑活动的视图状态信息，或者完全删除视图状态。

### <a name="expression-extensibility"></a><a name="BKMK_ExpressionExtensibility"></a> 表达式扩展性

在 .NET Framework 4.5 中，我们为开发人员提供了一种方法来创建可插入工作流设计器中的表达式和表达式创作体验。

### <a name="opt-in-for-workflow-45-features-in-rehosted-designer"></a><a name="BKMK_BackwardCompatRehostedDesigner"></a> 在重新承载设计器中选择加入工作流4.5 功能

为了保持向后兼容性，默认情况下，重新承载设计器中不会启用 .NET Framework 4.5 中包含的一些新功能。 这是为了确保使用重新承载的设计器的现有应用程序不会由于更新至最新版本而中断。 若要启用重新承载的设计器中的新功能，可将 <xref:System.Activities.Presentation.DesignerConfigurationService.TargetFrameworkName%2A> 设置为“.NET Framework 4.5”，或设置 <xref:System.Activities.Presentation.DesignerConfigurationService> 的各成员以启用各个功能。

## <a name="new-workflow-development-models"></a><a name="BKMK_NewWFModels"></a> 新工作流开发模型

除流程图和顺序工作流开发模型外，此版本还包括状态机工作流和协定优先工作流服务。

### <a name="state-machine-workflows"></a><a name="BKMK_StateMachine"></a> 状态机工作流

状态机工作流是在 [Microsoft .NET Framework 4 平台更新 1](/archive/blogs/endpoint/microsoft-net-framework-4-platform-update-1)中作为 .NET Framework 4 版本4.0.1 的一部分引入的。 此更新包括多个新类和活动，允许开发人员创建状态机工作流。 这些类和活动已针对 .NET Framework 4.5 进行了更新。 更新包括：

1. 能够对状态设置断点

2. 能够在工作流设计器中复制和粘贴转换

3. 设计器支持共享触发器转换创建

4. 用于创建状态机工作流的活动，包括：<xref:System.Activities.Statements.StateMachine>、<xref:System.Activities.Statements.State> 和 <xref:System.Activities.Statements.Transition>

以下屏幕截图显示了 [入门教程](getting-started-tutorial.md) 步骤 [如何：创建状态机工作流](how-to-create-a-state-machine-workflow.md)中的已完成状态机工作流。

![显示已完成状态机工作流的插图。](./media/whats-new-in-wf-in-dotnet/complete-state-machine-workflow.jpg)

有关创建状态机工作流的详细信息，请参阅 [状态机工作流](state-machine-workflows.md)。

### <a name="contract-first-workflow-development"></a><a name="BKMK_ContractFirst"></a> 协定优先工作流开发

协定优先工作流开发工具允许开发人员首先在代码中设计协定，然后，只需在 Visual Studio 中单击几下鼠标，就会在工具箱中自动生成表示每个操作的活动模板。 这些活动随后用于创建实现由该协定定义的操作的工作流。 工作流设计器将对该工作流服务进行验证，以确保实现这些操作并且工作流的签名与协定签名匹配。 开发人员还可以将工作流服务与所实现的协定的集合进行关联。 有关协定优先工作流服务开发的详细信息，请参阅 [如何：创建使用现有服务协定的工作流服务](how-to-create-a-workflow-service-that-consumes-an-existing-service-contract.md)。
