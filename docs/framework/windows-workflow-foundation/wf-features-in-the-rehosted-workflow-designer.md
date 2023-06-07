---
title: 重新承载的工作流设计器中新 Workflow Foundation 4.5 功能的支持
ms.date: 03/30/2017
ms.assetid: 1a4a4038-d8e6-41dd-99ea-93bd76286772
ms.openlocfilehash: 139215131afa38bc33539fa242a3584eb67d7941
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96293944"
---
# <a name="support-for-new-workflow-foundation-45-features-in-the-rehosted-workflow-designer"></a>重新承载的工作流设计器中新 Workflow Foundation 4.5 功能的支持

.NET Framework 4.5 (WF) Windows Workflow Foundation 引入了许多新功能，包括工作流设计器体验的多项增强功能。 本主题详细介绍重新承载的设计器中支持哪些功能以及当前不支持哪些功能。

> [!NOTE]
> 若要查看 .NET Framework 4.5 中引入的所有新 Windows Workflow Foundation (WF) 功能的列表，包括与设计器重新承载无关的功能，请参阅 [Windows Workflow Foundation 4.5 中 .NET Framework 的新增](whats-new-in-wf-in-dotnet.md)功能。

## <a name="activities"></a>活动

 内置活动库包含新活动和现有活动的新功能。 重新承载的设计器支持所有这些新活动。 有关这些新活动的详细信息，请参阅[.NET Framework 4.5 中 Windows Workflow Foundation 的新增功能](whats-new-in-wf-in-dotnet.md)的[活动](whats-new-in-wf-in-dotnet.md#BKMK_NewActivities)部分。

## <a name="c-expressions"></a>C# 表达式

 在 .NET Framework 4.5 之前，工作流中的所有表达式只能用 Visual Basic 编写。 在 .NET Framework 4.5 中，Visual Basic 表达式仅用于使用 Visual Basic 创建的项目。 Visual C# 项目现在将 C# 用于表达式。 在 Visual Studio 2012 中创作工作流时，将提供一个功能齐全的 c # 表达式编辑器，其中包括语法突出显示和 intellisense 等功能。 在使用 Visual Basic 表达式的以前版本中创建的 C# 工作流项目仍可继续使用。

> [!WARNING]
> 重新承载的设计器不支持 C# 表达式。

## <a name="new-designer-capabilities"></a>新设计器功能

### <a name="designer-search"></a>设计器搜索

 重新承载设计器不支持 .NET Framework 4.5 引入的 " [快速查找](whats-new-in-wf-in-dotnet.md#BKMK_QuickFind) " 和 " [在文件中查找](whats-new-in-wf-in-dotnet.md#BKMK_FindInFiles) " 功能。 重新承载的设计器不支持 `Toolbox` 搜索。 有关这些功能的详细信息，请参阅 [设计器搜索](whats-new-in-wf-in-dotnet.md#BKMK_DesignerSearch)。

> [!WARNING]
> 重新承载设计器不支持[快速查找](whats-new-in-wf-in-dotnet.md#BKMK_QuickFind)和[在文件中查找](whats-new-in-wf-in-dotnet.md#BKMK_FindInFiles)。

### <a name="delete-context-menu-item-in-variable-and-argument-designer"></a>删除变量和自变量设计器中的上下文菜单项

 在 .NET Framework 4 中，只能使用键盘在设计器中删除变量和参数。 从 .NET Framework 4.5 开始，可以使用上下文菜单来删除变量和参数。 重新承载的编辑器支持此功能。

 以下屏幕快照显示了变量和自变量设计器的上下文菜单。

 ![变量和参数设计器上下文菜单](./media/wf-features-in-the-rehosted-workflow-designer/designer-context-menu.png)

### <a name="auto-surround-with-sequence"></a>使用顺序进行自动环绕

 由于工作流或特定容器活动（如 <xref:System.Activities.Statements.NoPersistScope>）只能包含单个主体活动，因此添加第二个活动需要开发人员删除第一个活动，请添加一个 <xref:System.Activities.Statements.Sequence> 活动，然后将两个活动都添加到该顺序活动中。 从 .NET Framework 4.5 开始，在将第二个活动添加到设计器图面时， `Sequence` 将自动创建一个活动来包装这两个活动。 重新承载的编辑器支持此功能。

 以下屏幕快照显示了 `WriteLine` 的 `Body` 中的 `NoPersistScope` 活动。

 ![NoPersistScope 活动正文中的 WriteLine 活动。](./media/wf-features-in-the-rehosted-workflow-designer/auto-surround-write-line-activity.png)

 以下屏幕快照显示了在第一个 `Sequence` 下面丢弃第二个时在 `Body` 中自动创建的 `WriteLine` 活动。

 ![在 NoPersistScope 的正文中自动创建的序列。](./media/wf-features-in-the-rehosted-workflow-designer/auto-surround-sequence-activity.png)

### <a name="pan-mode"></a>平移模式

 若要更轻松地在设计器中浏览大型工作流，可以启用平移模式；通过此模式，开发人员可以单击并拖动工作流的可见部分将其移动，而无需使用滚动条。 用于激活平移模式的按钮位于设计器的右下角。 重新承载的编辑器支持此功能。

 下面的屏幕快照显示了位于工作流设计器右下角的平移按钮。

 ![工作流设计器中突出显示的 "平移" 按钮。](./media/wf-features-in-the-rehosted-workflow-designer/pan-button-workflow-designer.png)

 也可以使用鼠标中键或空格键来平移工作流设计器。

### <a name="multi-select"></a>多重选择

 通过拖动围绕活动的矩形（未启用平移模式时）或通过按住 Ctrl 键并逐一单击所需的活动，可以一次选择多个活动。 重新承载的编辑器支持此功能。

 也可以在设计器中拖动并放置多个选择的活动，并使用上下文菜单与这些活动交互。

### <a name="outline-view-of-workflow-items"></a>工作流项的大纲视图

 为了更加方便地浏览分层工作流，工作流的组件显示在树样式的大纲视图中。 大纲视图显示在 " **文档大纲** " 视图中。 若要在 Visual Studio 中打开此视图，请从顶部菜单中选择 " **视图**"、" **其他窗口**"、" **文档大纲**"，或按 Ctrl W、U。 单击大纲视图中的节点将会定位到工作流设计器中的相应活动，并且该大纲视图将会更新以显示在设计器中选择的活动。 重新承载的编辑器支持此功能。

 [入门教程](getting-started-tutorial.md)中的已完成工作流的以下屏幕截图显示了包含顺序工作流的大纲视图。

 ![使用 Visual Studio 中的顺序工作流显示大纲视图的屏幕截图](./media/wf-features-in-the-rehosted-workflow-designer/outline-view-in-workflow-designer.jpg)

### <a name="more-control-of-visibility-of-shell-bar-and-header-items"></a>shell 栏和标头项的更多可见性控制

 在重新承载的设计器中，某些标准 UI 控件可能对于给定的工作流没有意义，并可能已关闭。 在 .NET Framework 4 中，仅在设计器底部的 shell 栏支持此自定义项。 在 .NET Framework 4.5 中，可以通过设置适当的值来调整设计器顶部 shell 标头项的可见性 <xref:System.Activities.Presentation.View.DesignerView.WorkflowShellHeaderItemsVisibility%2A> <xref:System.Activities.Presentation.View.ShellHeaderItemsVisibility> 。

### <a name="auto-connect-and-auto-insert-in-flowchart-and-state-machine-workflows"></a>在流程图和状态机工作流中自动连接和自动插入

 在 .NET Framework 4 中，必须手动添加流程图工作流中节点之间的连接。 在 .NET Framework 4.5 中，流程图和状态机节点具有在将活动从工具箱拖到设计器图面时变为可见的自动连接点。 将活动放置在这些点中的一个点上会自动添加该活动以及必要的连接。

 下面的屏幕快照显示了从工具箱中拖动活动时变为可见的附属点。

 ![显示自动连接点的流程图开始节点](./media/wf-features-in-the-rehosted-workflow-designer/auto-connect-points-start-node.png)

 也可以将活动拖动到流程图节点和状态之间的连接上，以在两个其他节点之间自动插入该节点。 以下屏幕快照显示了突出显示的连接线，可在此连接线处从工具箱拖动并放置活动。

 ![放置活动的自动插入处理](./media/wf-features-in-the-rehosted-workflow-designer/auto-insert-connecting-line.png)

 重新承载的设计器支持自动连接和自动插入。

### <a name="designer-annotations"></a>设计器批注

 为促进开发较大型的工作流，设计器现在支持添加批注以帮助跟踪设计过程。 可以向活动、状态、流程图节点、变量和自变量添加批注。 以下屏幕快照显示了用于将批注添加到设计器的上下文菜单。

 ![显示用于添加表示法的菜单的屏幕截图。](./media/wf-features-in-the-rehosted-workflow-designer/designer-annotations-context-menu.png)

 重新承载的设计器支持设计器批注。

### <a name="define-and-consume-activitydelegate-objects-in-the-designer"></a>在设计器中定义和使用 ActivityDelegate 对象

 .NET Framework 4 中的活动使用 <xref:System.Activities.ActivityDelegate> 对象公开工作流的其他部分可与工作流的执行交互的执行点，但使用这些执行点通常需要大量的代码。 在此版本中，开发人员可以使用工作流设计器来定义和使用活动委托。 有关详细信息，请参阅 [如何：在工作流设计器中定义和使用活动委托](/visualstudio/workflow-designer/how-to-define-and-consume-activity-delegates-in-the-workflow-designer)。

 重新承载的设计器支持活动委托。

### <a name="build-time-validation"></a>生成时验证

 在 .NET Framework 4 中，工作流验证错误在工作流项目的生成过程中不计为生成错误。 这意味着即使存在工作流验证错误，工作流项目的生成也可能成功。 在 .NET Framework 4.5 中，工作流验证错误将导致生成失败。

> [!WARNING]
> 重新承载的设计器不支持生成时验证。

### <a name="design-time-background-validation"></a>设计时后台验证

 在 .NET Framework 4 中，工作流被验证为前台进程，这可能会在复杂或耗时的验证过程中阻止 UI。 工作流验证现在发生在后台线程上，因此不会阻止 UI。

 重新承载的设计器支持设计时后台验证。

### <a name="view-state-located-in-a-separate-location-in-xaml-files"></a>位于 XAML 文件中的单独位置处的视图状态

 在 .NET Framework 4 中，工作流的视图状态信息存储在多个不同位置的 XAML 文件中。 这对于想直接读取 XAML 或编写用于删除视图状态信息的代码的开发人员来说很不方便。 在 .NET Framework 4.5 中，XAML 文件中的视图状态信息序列化为 XAML 文件中的单独元素。  开发人员可以轻松地查找和编辑活动的视图状态信息，或者完全删除视图状态。

 重新承载的工作流编辑器支持此功能。

### <a name="opt-in-for-workflow-45-features-in-rehosted-designer"></a>重新承载的设计器中 Workflow 4.5 功能的选择性加入

 为了保持向后兼容性，默认情况下，重新承载设计器中不会启用 .NET Framework 4.5 中包含的一些新功能。 这是为了确保使用重新承载的设计器的现有应用程序不会由于更新至最新版本而中断。 若要启用重新承载的设计器中的新功能，可将 <xref:System.Activities.Presentation.DesignerConfigurationService.TargetFrameworkName%2A> 设置为“.Net Framework 4.5”，或设置 <xref:System.Activities.Presentation.DesignerConfigurationService> 的各成员以启用各个功能。

## <a name="new-workflow-development-models"></a>新工作流开发模型

 除流程图和顺序工作流开发模型外，此版本还包括状态机工作流和协定优先工作流服务。

### <a name="state-machine-workflows"></a>状态机工作流

 状态机工作流作为 [Microsoft .NET Framework 4 平台更新 1](/archive/blogs/endpoint/microsoft-net-framework-4-platform-update-1)中 .NET Framework 4.0.1 的一部分引入。 此更新包括多个新类和活动，允许开发人员创建状态机工作流。 这些类和活动已针对 .NET Framework 4.5 进行了更新。 更新包括：

1. 能够对状态设置断点

2. 能够在工作流设计器中复制和粘贴转换

3. 设计器支持共享触发器转换创建

4. 用于创建状态机工作流的活动，包括：<xref:System.Activities.Statements.StateMachine>、<xref:System.Activities.Statements.State> 和 <xref:System.Activities.Statements.Transition>

 以下屏幕截图显示了 [入门教程](getting-started-tutorial.md) 步骤 [如何：创建状态机工作流](how-to-create-a-state-machine-workflow.md)中的已完成状态机工作流。

 ![显示已完成状态机工作流的插图。](./media/wf-features-in-the-rehosted-workflow-designer/complete-state-machine-workflow.jpg)

 有关创建状态机工作流的详细信息，请参阅 [状态机工作流](state-machine-workflows.md)。 重新承载的工作流编辑器状态机工作流。

### <a name="contract-first-workflow-development"></a>协定优先工作流开发

 协定优先工作流开发工具允许开发人员首先在代码中设计协定，然后，只需在 Visual Studio 中单击几下鼠标，就会在工具箱中自动生成表示每个操作的活动模板。 这些活动随后用于创建实现由该协定定义的操作的工作流。 工作流设计器将对该工作流服务进行验证，以确保实现这些操作并且工作流的签名与协定签名匹配。 开发人员还可以将工作流服务与所实现的协定的集合进行关联。 有关协定优先工作流服务开发的详细信息，请参阅 [如何：创建使用现有服务协定的工作流服务](how-to-create-a-workflow-service-that-consumes-an-existing-service-contract.md)。

> [!WARNING]
> 工作流设计器不支持协定优先工作流开发。
