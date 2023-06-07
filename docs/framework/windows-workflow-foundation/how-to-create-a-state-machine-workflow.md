---
title: 如何：创建状态机工作流
description: 本文创建一个工作流，该工作流使用内置活动，如 StateMachine 活动和自定义活动。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 3ec60e8f-fad4-493e-a426-e7962d7aee8c
ms.openlocfilehash: 9df911779422ca2710686963a040a95258db8891
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96248914"
---
# <a name="how-to-create-a-state-machine-workflow"></a>如何：创建状态机工作流

工作流可基于内置活动以及自定义活动来构造。 本主题介绍如何创建一个工作流，该工作流使用内置活动（如 <xref:System.Activities.Statements.StateMachine> 活动）和前面的 [如何：创建活动](how-to-create-an-activity.md) 主题中的自定义活动。 该工作流模拟猜数游戏。  
  
> [!NOTE]
> 入门教程中的每个主题都依赖于前面的主题。 若要完成本主题，必须先完成 [操作方法：创建活动](how-to-create-an-activity.md)。  
  
> [!NOTE]
> 若要下载完整版教程，请参阅 [Windows Workflow Foundation (WF45) — 入门教程](https://go.microsoft.com/fwlink/?LinkID=248976)。  
  
### <a name="to-create-the-workflow"></a>创建工作流  
  
1. 在 **解决方案资源管理器** 中右键单击 " **NumberGuessWorkflowActivities** "，然后选择 "**添加**"、"**新建项**"。  
  
2. 在 " **已安装** 的 **公共项** " 节点中，选择 " **工作流**"。 从 **“工作流”** 列表中选择 **“活动”**。  
  
3. `StateMachineNumberGuessWorkflow`在 "**名称**" 框中键入，然后单击 "**添加**"。  
  
4. 将 " **StateMachine** " 活动从 "**工具箱**" 的 "**状态机**" 部分拖放到工作流设计图面上的 "在 **此处放置活动**" 标签上。  
  
### <a name="to-create-the-workflow-variables-and-arguments"></a>创建工作流变量和自变量  
  
1. 在 **解决方案资源管理器** 中双击 " **statemachinenumberguessworkflow.xaml** " 以在设计器中显示工作流（如果尚未显示）。  
  
2. 单击工作流设计器左下方的 " **参数** " 以显示 " **参数** " 窗格。  
  
3. 单击 **“创建参数”**。  
  
4. 在 `MaxNumber` "**名称**" 框中键入，从 "**方向**" 下拉列表 **中** 选择 "输入"，从 "**参数类型**" 下拉列表中选择 " **Int32** "，然后按 enter 保存该参数。  
  
5. 单击 **“创建参数”**。  
  
6. 在 `Turns` 新添加的参数下方的 "**名称**" 框中键入 `MaxNumber` ，从 "**方向**" 下拉列表中选择 "向 **外**"，从 "**参数类型**" 下拉列表中选择 " **Int32** "，然后按 enter。  
  
7. 单击活动设计器左下角的 **“参数”**，关闭 **“参数”** 窗格。  
  
8. 单击工作流设计器左下方的 " **变量** "，以显示 " **变量** " 窗格。  
  
9. 单击 **“创建变量”**。  
  
    > [!TIP]
    > 如果未显示 " **创建变量** " 框，请单击 <xref:System.Activities.Statements.StateMachine> 工作流设计器图面上的活动以选择它。  
  
10. 在 `Guess` "**名称**" 框中键入，从 "**变量类型**" 下拉列表中选择 " **Int32** "，然后按 enter 保存变量。  
  
11. 单击 **“创建变量”**。  
  
12. 在 `Target` "**名称**" 框中键入，从 "**变量类型**" 下拉列表中选择 " **Int32** "，然后按 enter 保存变量。  
  
13. 单击活动设计器左下角的 **“变量”**，关闭 **“变量”** 窗格。  
  
### <a name="to-add-the-workflow-activities"></a>添加工作流活动  
  
1. 单击 **State1** 以将其选中。 在 " **属性" 窗口** 中，将 **DisplayName** 更改为 `Initialize Target` 。  
  
    > [!TIP]
    > 如果未显示 "**属性" 窗口**，请从 "**视图**" 菜单中选择 "**属性窗口**"。  
  
2. 在工作流设计器中，双击新重命名的 " **初始化目标** " 状态以将其展开。  
  
3. 将 " **Assign** " 活动从 "**工具箱**" 的 "**基元**" 部分拖放到 "状态" 的 "**输入**" 部分。 在 " `Target` **到** " 框中键入，然后在 " **输入 c # 表达式** " 或 " **输入 VB 表达式** " 框中键入以下表达式。  
  
    ```vb  
    New System.Random().Next(1, MaxNumber + 1)  
    ```  
  
    ```csharp  
    new System.Random().Next(1, MaxNumber + 1)  
    ```  
  
    > [!TIP]
    > 如果未显示 **“工具箱”** 窗口，请从 **“视图”** 菜单中选择 **“工具箱”**。  
  
4. 通过单击工作流设计器顶部显示的痕迹导航中的 "状态机"，返回到工作流设计 **器中的** "总体状态机" 视图。  
  
5. 将 "**状态**" 活动从 "**工具箱**" 的 "**状态机**" 部分拖到工作流设计器上，并将其悬停在 "**初始化目标**" 状态。 请注意，当新状态位于其上时，将在 **初始化目标** 状态周围显示四个三角形。 将新状态置于紧靠 " **初始化目标** " 状态下的三角形上。 这会将新状态置于工作流上，并创建从 **初始化目标** 状态到新状态的转换。  
  
6. 单击 " **State1** " 将其选中，将 " **DisplayName** " 更改为 `Enter Guess` ，然后在工作流设计器中双击该状态以将其展开。  
  
7. 将 " **WriteLine** " 活动从 "**工具箱**" 的 "**基元**" 部分拖放到 "状态" 的 "**输入**" 部分。  
  
8. 在 **WriteLine** 的 "**文本**" 属性框中键入以下表达式。  
  
    ```vb  
    "Please enter a number between 1 and " & MaxNumber  
    ```  
  
    ```csharp  
    "Please enter a number between 1 and " + MaxNumber  
    ```  
  
9. 将 " **Assign** " 活动从 "**工具箱**" 的 "**基元**" 部分拖放到状态的 "**退出**" 部分。  
  
10. 在 `Turns` " **到** " 框中键入，然后 `Turns + 1` 在 " **输入 c # 表达式** " 或 " **输入 VB 表达式** " 框中键入。  
  
11. 通过单击工作流设计器顶部显示的痕迹导航中的 "状态机"，返回到工作流设计 **器中的** "总体状态机" 视图。  
  
12. 从 "**工具箱**" 的 "**状态机**" 部分拖放 " **FinalState** " 活动，将其悬停在 "**输入推测** 状态" 上，并将其放到 "**输入推测**" 状态右侧显示的三角形上，以便在 "**输入推测**" 和 " **FinalState**" 之间创建转换。  
  
13. 转换的默认名称为 **T2**。 单击工作流设计器中的转换以将其选中，并将其 **DisplayName** 设置为 " **猜测正确**"。 然后，单击并选择 " **FinalState**"，将其拖到右侧，以便在不覆盖两种状态之一的情况下显示完整转换名称的空间。 这样可以方便完成本教程中的剩余步骤。  
  
14. 双击工作流设计器中新重命名的 " **猜测正确** 转换"，将其展开。  
  
15. 将 " **ReadInt** " 活动从 "**工具箱**" 的 " **NumberGuessWorkflowActivities** " 部分拖放到转换的 "**触发器**" 部分。  
  
16. 在 " **ReadInt** " 活动的 "**属性" 窗口** 中，在 `"EnterGuess"` " **BookmarkName** " 属性值框中键入包含引号，然后在 `Guess` "**结果** 属性值" 框中键入。  
  
17. 在 " **推测正确** 转换的 **条件** " 属性值框中键入以下表达式。  
  
    ```vb  
    Guess = Target  
    ```  
  
    ```csharp  
    Guess == Target  
    ```  
  
18. 通过单击工作流设计器顶部显示的痕迹导航中的 "状态机"，返回到工作流设计 **器中的** "总体状态机" 视图。  
  
    > [!NOTE]
    > 当收到触发器事件和 <xref:System.Activities.Statements.Transition.Condition%2A>（如果存在）时，将发生转换，计算结果为 `True`。 对于此转换，如果用户 `Guess` 与随机生成的匹配 `Target` ，则控制将传递到 **FinalState** ，工作流完成。  
  
19. 根据推测是否正确，工作流应转换为 **FinalState** 或返回到另一个 Try 的 **进入推测** 状态。 这两个转换共享等待用户推测通过 **ReadInt** 活动接收的相同触发器。 这称为共享转换。 若要创建共享转换，请单击指示 **推测正确** 转换开始的圆圈，然后将其拖到所需状态。 在这种情况下，转换是一种自转换，因此拖动 **推测正确** 转换的开始点，并将其放回 **Enter 推测** 状态的底部。 创建转换后，在工作流设计器中选择该转换，并将其 **DisplayName** 属性设置为 " **猜测错误**"。  
  
    > [!NOTE]
    > 还可以从转换设计器中创建共享的转换，方法是单击 "转换设计器" 底部的 " **添加共享触发器转换** "，然后从 " **可用状态到连接** " 下拉选择所需的目标状态。  
  
    > [!NOTE]
    > 注意，如果转换的 <xref:System.Activities.Statements.Transition.Condition%2A> 计算结果为 `false`（或所有共享触发转换条件的计算结果均为 `false`），转换将不发生，并且此状态下的所有转换的所有触发将被重新计划。 在本教程中，由于配置条件的方式，这种情况不会发生（我们针对猜测是正确或者错误提供了具体的操作）。  
  
20. 在工作流设计器中双击 **推测错误** 的转换，以将其展开。 请注意，**触发器** 已设置为 **推测正确** 转换使用的相同 **ReadInt** 活动。  
  
21. 在 " **Condition** " 属性值框中键入以下表达式。  
  
    ```vb  
    Guess <> Target  
    ```  
  
    ```csharp  
    Guess != Target  
    ```  
  
22. 将 " **If** " 活动从 "**工具箱**" 的 "**控制流**" 部分拖放到转换的 "**操作**" 部分。  
  
23. 在 **If** 活动的 **Condition** 属性值框中键入以下表达式。  
  
    ```text
    Guess < Target
    ```  
  
24. 将两个 " **WriteLine** " 活动从 "**工具箱**" 的 "**基元**" 部分拖放到 " **If** " 活动的 " **Then** " 部分，另一个处于 " **Else** " 部分。  
  
25. 单击 " **Then** " 部分中的 " **WriteLine** " 活动将其选中，然后在 "**文本** 属性值" 框中键入以下表达式。  
  
    ```text
    "Your guess is too low."  
    ```  
  
26. 单击 " **Else** " 部分中的 " **WriteLine** " 活动将其选中，然后在 "**文本** 属性值" 框中键入以下表达式。  
  
    ```text
    "Your guess is too high."  
    ```  
  
27. 通过单击工作流设计器顶部显示的痕迹导航中的 "状态机"，返回到工作流设计 **器中的** "总体状态机" 视图。  
  
     下面的示例阐释已完成的工作流。  
  
     ![显示已完成状态机工作流的插图。](./media/how-to-create-a-state-machine-workflow/complete-state-machine-workflow.jpg)  
  
### <a name="to-build-the-workflow"></a>生成工作流  
  
1. 按 CTRL+SHIFT+B 生成解决方案。  
  
     有关如何运行工作流的说明，请参阅下一主题 [如何：运行工作流](how-to-run-a-workflow.md)。 如果你已经完成了如何：使用不同的工作流样式[运行工作流](how-to-run-a-workflow.md)步骤，并希望使用此步骤中的状态机工作流运行它，请跳到[如何：运行工作流](how-to-run-a-workflow.md)的 "[生成并运行应用程序](how-to-run-a-workflow.md#BKMK_ToRunTheApplication)" 部分。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Activities.Statements.Flowchart>
- <xref:System.Activities.Statements.FlowDecision>
- [Windows Workflow Foundation 编程](programming.md)
- [设计工作流](designing-workflows.md)
- [入门教程](getting-started-tutorial.md)
- [如何：创建活动](how-to-create-an-activity.md)
- [如何：运行工作流](how-to-run-a-workflow.md)
