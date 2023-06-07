---
title: 如何：创建顺序工作流
description: 本文将创建一个工作流，该工作流使用内置活动，如 Sequence 活动和自定义活动。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 5280e816-ae17-48c4-8de0-a1e6895dd8f0
ms.openlocfilehash: f80ac471fdcc425504b11b5fb17effa888aa9590
ms.sourcegitcommit: 9a4488a3625866335e83a20da5e9c5286b1f034c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2020
ms.locfileid: "83419689"
---
# <a name="how-to-create-a-sequential-workflow"></a>如何：创建顺序工作流

工作流可基于内置活动以及自定义活动来构造。 本主题介绍如何创建一个工作流，该工作流使用内置活动（如 <xref:System.Activities.Statements.Sequence> 活动）和前面的[如何：创建活动](how-to-create-an-activity.md)主题中的自定义活动。 该工作流模拟猜数游戏。

> [!NOTE]
> 入门教程中的每个主题都依赖于前面的主题。 若要完成本主题，必须先完成[操作方法：创建活动](how-to-create-an-activity.md)。

> [!NOTE]
> 若要下载完整版教程，请参阅 [Windows Workflow Foundation (WF45) — 入门教程](https://go.microsoft.com/fwlink/?LinkID=248976)。

## <a name="to-create-the-workflow"></a>创建工作流

1. 在**解决方案资源管理器**中右键单击 " **NumberGuessWorkflowActivities** "，然后选择 "**添加**"、"**新建项**"。

2. 在 "**已安装**的**公共项**" 节点中，选择 "**工作流**"。 从 **“工作流”** 列表中选择 **“活动”**。

3. `SequentialNumberGuessWorkflow`在 "**名称**" 框中键入，然后单击 "**添加**"。

4. 将 "**序列**" 活动从 "**工具箱**" 的 "**控制流**" 部分拖放到工作流设计图面上的 "在**此处放置活动**" 标签上。

## <a name="to-create-the-workflow-variables-and-arguments"></a>创建工作流变量和自变量

1. 在**解决方案资源管理器**中双击 " **sequentialnumberguessworkflow.xaml** " 以在设计器中显示工作流（如果尚未显示）。

2. 单击工作流设计器左下方的 "**参数**" 以显示 "**参数**" 窗格。

3. 单击 **“创建参数”**。

4. 在 `MaxNumber` "**名称**" 框中键入，从 "**方向**" 下拉列表**中**选择 "输入"，从 "**参数类型**" 下拉列表中选择 " **Int32** "，然后按 enter 保存该参数。

5. 单击 **“创建参数”**。

6. 在 `Turns` 新添加的参数下方的 "**名称**" 框中键入 `MaxNumber` ，从 "**方向**" 下拉列表中选择 "向**外**"，从 "**参数类型**" 下拉列表中选择 " **Int32** "，然后按 enter。

7. 单击活动设计器左下角的 **“参数”**，关闭 **“参数”** 窗格。

8. 单击工作流设计器左下方的 "**变量**"，以显示 "**变量**" 窗格。

9. 单击 **“创建变量”**。

    > [!TIP]
    > 如果未显示 "**创建变量**" 框，请单击工作流设计器图面上的 "**序列**" 活动将其选中。

10. 在 `Guess` "**名称**" 框中键入，从 "**变量类型**" 下拉列表中选择 " **Int32** "，然后按 enter 保存变量。

11. 单击 **“创建变量”**。

12. 在 `Target` "**名称**" 框中键入，从 "**变量类型**" 下拉列表中选择 " **Int32** "，然后按 enter 保存变量。

13. 单击活动设计器左下角的 **“变量”**，关闭 **“变量”** 窗格。

## <a name="to-add-the-workflow-activities"></a>添加工作流活动

1. 将 " **Assign** " 活动从 "**工具箱**" 的 "**基元**" 部分拖放到 " **Sequence** " 活动。 在 " `Target` **到**" 框中键入，然后在 "**输入 c # 表达式**" 或 "**输入 VB 表达式**" 框中键入以下表达式。

    ```vb
    New System.Random().Next(1, MaxNumber + 1)
    ```

    ```csharp
    new System.Random().Next(1, MaxNumber + 1)
    ```

    > [!TIP]
    > 如果未显示 **“工具箱”** 窗口，请从 **“视图”** 菜单中选择 **“工具箱”**。

2. 将 " **DoWhile** " 活动从 "**工具箱**" 的 "**控制流**" 部分拖放到工作流上，使其位于 "**分配**" 活动下。

3. 在 " **DoWhile** " 活动的 " **Condition** " 属性值框中键入以下表达式。

    ```vb
    Guess <> Target
    ```

    ```csharp
    Guess != Target
    ```

     <xref:System.Activities.Statements.DoWhile> 活动执行其子活动，然后计算其 <xref:System.Activities.Statements.DoWhile.Condition%2A>。 如果 <xref:System.Activities.Statements.DoWhile.Condition%2A> 的计算结果为 `True`，则再次执行 <xref:System.Activities.Statements.DoWhile> 中的活动。 在此示例中，将计算用户的猜测值，并且 <xref:System.Activities.Statements.DoWhile> 继续执行，直到猜测值正确。

4. 从 "**工具箱**" 的 " **NumberGuessWorkflowActivities** " 部分拖动 " **Prompt** " 活动，并将其放入上一步中的 " **DoWhile** " 活动。

5. 在 "**属性" 窗口**中，在 `"EnterGuess"` **Prompt**活动的 " **BookmarkName** " 属性值框中键入包含引号。 `Guess`在 "**结果**属性值" 框中键入，然后在 "**文本**" 属性框中键入以下表达式。

    ```vb
    "Please enter a number between 1 and " & MaxNumber
    ```

    ```csharp
    "Please enter a number between 1 and " + MaxNumber
    ```

    > [!TIP]
    > 如果未显示 "**属性" 窗口**，请从 "**视图**" 菜单中选择 "**属性窗口**"。

6. 将 " **Assign** " 活动从 "**工具箱**" 的 "**基元**" 部分拖放到 " **DoWhile** " 活动中，使其跟随 " **Prompt** " 活动。

    > [!NOTE]
    > 删除 "**分配**" 活动时，请注意工作流设计器如何自动添加**序列**活动，以同时包含**Prompt**活动和新添加的 "**分配**" 活动。

7. 在 `Turns` "**到**" 框中键入，然后 `Turns + 1` 在 "**输入 c # 表达式**" 或 "**输入 VB 表达式**" 框中键入。

8. 将 " **If** " 活动从 "工具箱" 的 "**控制流**" 部分拖放到 " **Sequence** " 活动中，使其位于新添加的 **"** **分配**" 活动之后。

9. 在**If**活动的**Condition**属性值框中键入以下表达式。

    ```vb
    Guess <> Target
    ```

    ```csharp
    Guess != Target
    ```

10. 从 "**工具箱**" 的 "**控制流**" 部分中，将另一个 " **if** " 活动拖放到第一个 " **if** " 活动的 " **Then** " 部分。

11. 在新添加的**If**活动的**Condition**属性值框中键入以下表达式。

    ```text
    Guess < Target
    ```

12. 从 "**工具箱**" 的 "**基元**" 部分拖放两个 " **WriteLine** " 活动，将其拖放到新添加的 " **If** " 活动的 " **Then** " 部分，另一个位于 " **Else** " 部分。

13. 单击 " **Then** " 部分中的 " **WriteLine** " 活动将其选中，然后在 "**文本**属性值" 框中键入以下表达式。

    ```text
    "Your guess is too low."
    ```

14. 单击 " **Else** " 部分中的 " **WriteLine** " 活动将其选中，然后在 "**文本**属性值" 框中键入以下表达式。

    ```text
    "Your guess is too high."
    ```

     下面的示例演示了已完成的工作流：

     ![显示已完成的顺序工作流的屏幕截图。](./media/how-to-create-a-sequential-workflow/complete-sequential-workflow.jpg)

## <a name="to-build-the-workflow"></a>生成工作流

1. 按 CTRL+SHIFT+B 生成解决方案。

     有关如何运行工作流的说明，请参阅下一主题[如何：运行工作流](how-to-run-a-workflow.md)。 如果你已经完成了如何：使用不同的工作流样式[运行工作流](how-to-run-a-workflow.md)步骤，并希望使用此步骤中的顺序工作流运行它，请跳到[如何：运行工作流](how-to-run-a-workflow.md)的 "[生成并运行应用程序](how-to-run-a-workflow.md#BKMK_ToRunTheApplication)" 部分。

## <a name="see-also"></a>另请参阅

- <xref:System.Activities.Statements.Flowchart>
- <xref:System.Activities.Statements.FlowDecision>
- [Windows Workflow Foundation 编程](programming.md)
- [设计工作流](designing-workflows.md)
- [入门教程](getting-started-tutorial.md)
- [如何：创建活动](how-to-create-an-activity.md)
- [如何：运行工作流](how-to-run-a-workflow.md)
