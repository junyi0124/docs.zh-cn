---
title: 如何：创建活动
description: 本文介绍如何在 Workflow Foundation 中创建两个活动：一个使用代码实现其逻辑，另一个使用其他活动定义。
ms.date: 09/14/2018
dev_langs:
- csharp
- vb
ms.assetid: c09b1e99-21b5-4d96-9c04-ec31db3f4436
ms.openlocfilehash: dae099d102b0c85d09a1ef8bcce56e8a9096bd20
ms.sourcegitcommit: 9a4488a3625866335e83a20da5e9c5286b1f034c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2020
ms.locfileid: "83419585"
---
# <a name="how-to-create-an-activity"></a>如何：创建活动

活动是 [!INCLUDE[wf1](../../../includes/wf1-md.md)] 中的核心行为单元。 活动的执行逻辑可以使用托管代码实现，也可以使用其他活动实现。 本主题演示如何创建两个活动。 第一个活动是简单活动，它使用代码来实现其执行逻辑。 第二个活动的实现是用其他活动定义的。 后续教程步骤会使用这些活动。

> [!NOTE]
> 若要下载完整版教程，请参阅 [Windows Workflow Foundation (WF45) — 入门教程](https://go.microsoft.com/fwlink/?LinkID=248976)。

## <a name="create-the-activity-library-project"></a>创建活动库项目

1. 打开 Visual Studio，然后**New**  >  从 "**文件**" 菜单中选择 "新建**项目**"。

2. 在 "**新建项目**" 对话框中的 "**已安装**" 类别下，选择 " **Visual c #**  >  **工作流**" （或**Visual Basic**  >  **工作流**） "。

    > [!NOTE]
    > 如果看不到**工作流**模板类别，可能需要安装 Visual Studio 的**Windows Workflow Foundation**组件。 选择 "**新建项目**" 对话框左侧的 "**打开 Visual Studio 安装程序**" 链接。 在 Visual Studio 安装程序中，选择 "**单个组件**" 选项卡。然后，在 "**开发活动**" 类别下，选择 " **Windows Workflow Foundation** " 组件。 选择 "**修改**" 以安装组件。

3. 选择 "**活动库**" 项目模板。 在“**名称**”框中键入 `NumberGuessWorkflowActivities`，然后单击“**确定**”。

4. 在 **“解决方案资源管理器”** 中右键单击 **“Activity1.xaml”**，然后选择 **“删除”**。 单击 **“确定”** 进行确认。

## <a name="create-the-readint-activity"></a>创建 ReadInt 活动

1. 从 "**项目**" 菜单中选择 "**添加新项**"。

2. 在 "**已安装**的  >  **常见项**" 节点中，选择 "**工作流**"。 从 "**工作流**" 列表中选择 "**代码活动**"。

3. 在“**名称**”框中键入 `ReadInt`，然后单击“**添加**”。

4. 将现有的 `ReadInt` 定义替换为下面的定义。

     [!code-csharp[CFX_WF_GettingStarted#1](~/samples/snippets/csharp/VS_Snippets_CFX/cfx_wf_gettingstarted/cs/readint.cs#1)]
     [!code-vb[CFX_WF_GettingStarted#1](~/samples/snippets/visualbasic/VS_Snippets_CFX/cfx_wf_gettingstarted/vb/readint.vb#1)]

    > [!NOTE]
    > `ReadInt` 活动派生自 <xref:System.Activities.NativeActivity%601> 而不是 <xref:System.Activities.CodeActivity>，这是默认的代码活动模板。 如果活动提供单个结果，可以使用 <xref:System.Activities.CodeActivity%601>，它是通过 <xref:System.Activities.Activity%601.Result%2A> 自变量公开的，但是 <xref:System.Activities.CodeActivity%601> 不支持使用书签，因此使用 <xref:System.Activities.NativeActivity%601>。

## <a name="create-the-prompt-activity"></a>创建 Prompt 活动

1. 按**Ctrl** + **Shift** + **B**生成项目。 通过生成项目，可以使用此项目中的 `ReadInt` 活动生成此步骤中的自定义活动。

2. 从 "**项目**" 菜单中选择 "**添加新项**"。

3. 在 "**已安装**的  >  **常见项**" 节点中，选择 "**工作流**"。 从 **“工作流”** 列表中选择 **“活动”**。

4. 在“**名称**”框中键入 `Prompt`，然后单击“**添加**”。

5. 双击 "**解决方案资源管理器**中的" **Prompt** "以在设计器中显示它（如果尚未显示）。

6. 单击活动设计器左下角的 **“参数”**，显示 **“参数”** 窗格。

7. 单击 **“创建参数”**。

8. 在 `BookmarkName` "**名称**" 框中键入，从 "**方向**" 下拉列表**中**选择 "输入"，从 "**参数类型**" 下拉列表中选择 "**字符串**"，然后按**enter**保存该参数。

9. 单击 **“创建参数”**。

10. 在 `Result` 新添加的参数下方的 "**名称**" 框中键入 `BookmarkName` ，从 "**方向**" 下拉列表中选择 "向**外**"，从 "**参数类型**" 下拉列表中选择 " **Int32** "，然后按**enter**。

11. 单击 **“创建参数”**。

12. 在 `Text` "**名称**" 框中键入，从 "**方向**" 下拉列表**中**选择 "输入"，从 "**参数类型**" 下拉列表中选择 "**字符串**"，然后按**enter**保存该参数。

     在以下步骤中，这三个参数将绑定到添加到 <xref:System.Activities.Statements.WriteLine> 活动中的 `ReadInt` 和 `Prompt` 活动的相应参数。

13. 单击活动设计器左下角的 **“参数”**，关闭 **“参数”** 窗格。

14. 将 "**序列**" 活动从 "**工具箱**" 的 "**控制流**" 部分拖放到**Prompt**活动设计器的 "在**此处放置活动**" 标签上。

    > [!TIP]
    > 如果未显示 **“工具箱”** 窗口，请从 **“视图”** 菜单中选择 **“工具箱”**。

15. 将 " **WriteLine** " 活动从 "**工具箱**" 的 "**基元**" 部分拖放到 "**序列**" 活动的 "**放置活动位置**" 标签上。

16. 将**WriteLine**活动的**text**参数绑定到**Prompt**活动的**Text**参数，方法是 `Text` 在 "**属性**" 窗口中键入 "**输入 c # 表达式**" 或 "**输入 VB 表达式**" 框，然后按**tab**键两次。 这会关闭 IntelliSense 列表成员窗口，并通过将选择范围移离属性保存属性值。 还可以通过在 `Text` 活动本身的 "**输入 c # 表达式**" 或 "**输入 VB 表达式**" 框中键入来设置此属性。

    > [!TIP]
    > 如果未显示 "**属性" 窗口**，请从 "**视图**" 菜单中选择 "**属性窗口**"。

17. 将 " **ReadInt** " 活动从 "**工具箱**" 的 " **NumberGuessWorkflowActivities** " 部分拖放到 " **Sequence** " 活动中，使其位于 " **WriteLine** " 活动之后。

18. 通过**BookmarkName**在**BookmarkName** "属性" **Prompt** **ReadInt** `BookmarkName` **窗口**中**BookmarkName**参数右侧的 "**输入 VB 表达式**" 框中键入，然后按**tab**键两次以关闭 "IntelliSense 列表成员" 窗口并保存属性，将 ReadInt 活动的 BookmarkName 参数绑定到 Prompt 活动的 BookmarkName 参数。

19. 将**ReadInt**活动的**result**参数绑定到**Prompt**活动的**Result**参数，方法是 `Result` 在 "**属性" 窗口**中的**result**参数右侧的 "**输入 VB 表达式**" 框中键入，然后按**tab**键两次。

20. 按**Ctrl** + **Shift** + **B**生成解决方案。

## <a name="next-steps"></a>后续步骤

有关如何使用这些活动创建工作流的说明，请参阅本教程中的 "[如何：创建工作流](how-to-create-a-workflow.md)" 教程中的下一步。

## <a name="see-also"></a>另请参阅

- <xref:System.Activities.CodeActivity>
- <xref:System.Activities.NativeActivity%601>
- [设计和实现自定义活动](designing-and-implementing-custom-activities.md)
- [入门教程](getting-started-tutorial.md)
- [如何：创建工作流](how-to-create-a-workflow.md)
- [在自定义设计器中使用 ExpressionTextBox](./samples/using-the-expressiontextbox-in-a-custom-activity-designer.md)
