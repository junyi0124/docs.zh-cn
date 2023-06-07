---
title: 如何：创建 Flowchart 工作流
description: 本文介绍如何创建一个工作流，该工作流使用内置活动和前一篇文章中的自定义活动。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 185d7aea-68a6-4bd8-adde-45050f33170a
ms.openlocfilehash: f8e0703591629a72e0a8f6eeb05dd9d19c8c4c91
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96275822"
---
# <a name="how-to-create-a-flowchart-workflow"></a>如何：创建 Flowchart 工作流

工作流可基于内置活动以及自定义活动来构造。 本主题介绍如何创建一个工作流，该工作流使用内置活动（如 <xref:System.Activities.Statements.Flowchart> 活动）和前面的 [如何：创建活动](how-to-create-an-activity.md) 主题中的自定义活动。 该工作流模拟猜数游戏。  
  
> [!NOTE]
> 入门教程中的每个主题都依赖于前面的主题。 若要完成本主题，必须先完成 [操作方法：创建活动](how-to-create-an-activity.md)。  
  
> [!NOTE]
> 若要下载完整版教程，请参阅 [Windows Workflow Foundation (WF45) — 入门教程](https://go.microsoft.com/fwlink/?LinkID=248976)。  
  
### <a name="to-create-the-workflow"></a>创建工作流  
  
1. 在 **解决方案资源管理器** 中右键单击 " **NumberGuessWorkflowActivities** "，然后选择 "**添加**"、"**新建项**"。  
  
2. 在 " **已安装** 的 **公共项** " 节点中，选择 " **工作流**"。 从 **“工作流”** 列表中选择 **“活动”**。  
  
3. `FlowchartNumberGuessWorkflow`在 "**名称**" 框中键入，然后单击 "**添加**"。  
  
4. 将 "**流程图**" 活动从 "**工具箱**" 的 "**流程图**" 部分拖放到工作流设计图面上的 "在 **此处放置活动**" 标签上。  
  
### <a name="to-create-the-workflow-variables-and-arguments"></a>创建工作流变量和自变量  
  
1. 在 **解决方案资源管理器** 中双击 " **flowchartnumberguessworkflow.xaml** " 以在设计器中显示工作流（如果尚未显示）。  
  
2. 单击工作流设计器左下方的 " **参数** " 以显示 " **参数** " 窗格。  
  
3. 单击 **“创建参数”**。  
  
4. 在 `MaxNumber` "**名称**" 框中键入，从 "**方向**" 下拉列表 **中** 选择 "输入"，从 "**参数类型**" 下拉列表中选择 " **Int32** "，然后按 enter 保存该参数。  
  
5. 单击 **“创建参数”**。  
  
6. 在 `Turns` 新添加的参数下方的 "**名称**" 框中键入 `MaxNumber` ，从 "**方向**" 下拉列表中选择 "向 **外**"，从 "**参数类型**" 下拉列表中选择 " **Int32** "，然后按 enter。  
  
7. 单击活动设计器左下角的 **“参数”**，关闭 **“参数”** 窗格。  
  
8. 单击工作流设计器左下方的 " **变量** "，以显示 " **变量** " 窗格。  
  
9. 单击 **“创建变量”**。  
  
    > [!TIP]
    > 如果未显示 " **创建变量** " 框，请单击 <xref:System.Activities.Statements.Flowchart> 工作流设计器图面上的活动以选择它。  
  
10. 在 `Guess` "**名称**" 框中键入，从 "**变量类型**" 下拉列表中选择 " **Int32** "，然后按 enter 保存变量。  
  
11. 单击 **“创建变量”**。  
  
12. 在 `Target` "**名称**" 框中键入，从 "**变量类型**" 下拉列表中选择 " **Int32** "，然后按 enter 保存变量。  
  
13. 单击活动设计器左下角的 **“变量”**，关闭 **“变量”** 窗格。  
  
### <a name="to-add-the-workflow-activities"></a>添加工作流活动  
  
1. 将 " **Assign** " 活动从 "**工具箱**" 的 "**基元**" 部分拖放到 "**开始**" 节点上，该节点位于流程图的顶部。 当 **Assign** 活动位于 **开始** 节点上时， **开始** 节点附近会出现三个三角形。 删除位于 **开始** 节点正下方的三角形上的 "**分配**" 活动。 这会将这两个项链接在一起，并将 **分配** 活动指定为流程图中的第一个活动。  
  
    > [!NOTE]
    > 也还可以通过手动将活动链接到开始节点而将活动指示为流程图中的起始活动。 为此，请将鼠标悬停在 " **开始** " 节点上，单击在鼠标位于 " **开始** " 节点上时出现的矩形之一，然后将连接线向下拖至所需的活动，并将其放置在出现的某个矩形上。 还可以通过右键单击某一活动，并选择 " **设为开始节点**"，将该活动指定为启动活动。  
  
2. 在 " `Target` **到** " 框中键入，然后在 " **输入 c # 表达式** " 或 " **输入 VB 表达式** " 框中键入以下表达式。  
  
    ```vb  
    New System.Random().Next(1, MaxNumber + 1)  
    ```  
  
    ```csharp  
    new System.Random().Next(1, MaxNumber + 1)  
    ```  
  
    > [!TIP]
    > 如果未显示 **“工具箱”** 窗口，请从 **“视图”** 菜单中选择 **“工具箱”**。  
  
3. 从 "**工具箱**" 的 " **NumberGuessWorkflowActivities** " 部分拖动 " **prompt** " 活动，将其拖放到上一步骤中的 " **Assign** " 活动下面，并将 " **prompt** " 活动连接到 " **assign** " 活动。 可通过三种方法来连接这两个活动。 第一种方法是在将 **Prompt** 活动放置在工作流上时连接它们。 将 " **提示** " 活动拖到工作流中时，将其悬停在 " **assign** " 活动上，并将其放置到 " **提示** " 活动超过 " **assign** " 活动时显示的四个三角形之一。 第二种方法是将 **提示** 活动放到工作流上所需的位置。 然后，将鼠标悬停在 " **Assign** " 活动上，然后将出现的一个矩形拖到 " **Prompt** " 活动。 拖动鼠标，使 " **Assign** " 活动中的连接线连接到 **Prompt** 活动的一个矩形，然后松开鼠标按钮。 第三种方法与第一种方法非常相似，不同之处在于，它不是从 "**工具箱**" 拖动 " **Prompt** " 活动，而是将其从 "工作流" 设计图面上的位置拖动，将其悬停在 " **Assign** " 活动上，然后将其放到显示的某个三角形上。  
  
4. 在 **Prompt** 活动的 "**属性" 窗口** 中，在 `"EnterGuess"` " **BookmarkName** " 属性值框中键入包括引号。 `Guess`在 "**结果** 属性值" 框中键入，然后在 "**文本**" 属性框中键入以下表达式。  
  
    ```vb  
    "Please enter a number between 1 and " & MaxNumber  
    ```  
  
    ```csharp  
    "Please enter a number between 1 and " + MaxNumber  
    ```  
  
    > [!TIP]
    > 如果未显示 "**属性" 窗口**，请从 "**视图**" 菜单中选择 "**属性窗口**"。  
  
5. 从 **工具箱** 的 "**基元**" 部分拖动 " **Assign** " 活动，并使用上一步骤中所述的方法之一来连接该活动，使其位于 **Prompt** 活动下面。  
  
6. 在 `Turns` " **到** " 框中键入，然后 `Turns + 1` 在 " **输入 c # 表达式**  " 或 " **输入 VB 表达式** " 框中键入。  
  
7. 拖动 "**工具箱**" 的 "**流程图**" 部分中的 **FlowDecision** ，并将其连接到 " **Assign** " 活动下。 在 " **属性" 窗口** 中，在 " **Condition** " 属性值框中键入以下表达式。  
  
    ```vb  
    Guess = Target  
    ```  
  
    ```csharp  
    Guess == Target  
    ```  
  
8. 将另一个 " **FlowDecision** " 活动从 " **工具箱** " 拖放到第一个活动下。 通过将顶部 **FlowDecision** 活动中标记 **为 "False** " 的矩形拖到第二个 **FlowDecision** 活动顶部的矩形来连接这两个活动。  
  
    > [!TIP]
    > 如果在 **FlowDecision** 上看不到 " **True** " 和 " **False** " 标签，则将鼠标悬停在 **FlowDecision** 上。  
  
9. 单击第二个 " **FlowDecision** " 活动将其选中。 在 " **属性" 窗口** 中，在 " **Condition** " 属性值框中键入以下表达式。  
  
    ```text
    Guess < Target
    ```  
  
10. 将两个 " **WriteLine** " 活动从 "**工具箱**" 的 "**基元**" 部分拖放到两个 **FlowDecision** 活动下。 将底部 **FlowDecision** 活动的 **真正** 操作连接到最左端的 **Writeline** 活动，并将 **False** 操作连接到最右侧的 **writeline** 活动。  
  
11. 单击最左边的 " **WriteLine** " 活动将其选中，然后在 "**属性" 窗口** 的 " **Text** " 属性值框中键入以下表达式。  
  
    ```text
    "Your guess is too low."  
    ```  
  
12. 将 **WriteLine** 连接到它上面的 **Prompt** 活动的左侧。  
  
13. 单击最右边的 " **WriteLine** " 活动将其选中，然后在 "**属性" 窗口** 的 " **Text** " 属性值框中键入以下表达式。  
  
    ```text
    "Your guess is too high."  
    ```  
  
14. 将 **WriteLine** 活动连接到其上面的 **Prompt** 活动的右侧。  
  
     下面的示例阐释已完成的工作流。  
  
     ![显示已完成的 Windows Workflow Foundation 流程图的关系图。](./media/how-to-create-a-flowchart-workflow/completed-windows-workflow-flowchart.png)  
  
### <a name="to-build-the-workflow"></a>生成工作流  
  
1. 按 CTRL+SHIFT+B 生成解决方案。  
  
     有关如何运行工作流的说明，请参阅下一主题 [如何：运行工作流](how-to-run-a-workflow.md)。 如果你已经完成了如何：使用不同的工作流样式[运行工作流](how-to-run-a-workflow.md)步骤，并希望使用此步骤中的流程图工作流运行它，请跳到[如何：运行工作流](how-to-run-a-workflow.md)中的 "[生成并运行应用程序](how-to-run-a-workflow.md#BKMK_ToRunTheApplication)" 部分。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Activities.Statements.Flowchart>
- <xref:System.Activities.Statements.FlowDecision>
- [Windows Workflow Foundation 编程](programming.md)
- [设计工作流](designing-workflows.md)
- [入门教程](getting-started-tutorial.md)
- [如何：创建活动](how-to-create-an-activity.md)
- [如何：运行工作流](how-to-run-a-workflow.md)
