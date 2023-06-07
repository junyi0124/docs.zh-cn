---
title: 如何：并行承载多个版本的工作流
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 09c575df-e0a3-4f3b-9e01-a7ac59d65287
ms.openlocfilehash: 820ed324c8095e2f9f2823513a37965099f42c48
ms.sourcegitcommit: 005980b14629dfc193ff6cdc040800bc75e0a5a5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/14/2019
ms.locfileid: "70989652"
---
# <a name="how-to-host-multiple-versions-of-a-workflow-side-by-side"></a>如何：并行承载多个版本的工作流

`WorkflowIdentity` 为工作流应用程序开发人员提供了一种将名称和版本与工作流定义关联的方法，这种方法还可用于将此信息与持久化工作流实例相关联。 工作流应用程序开发人员可以使用这些标识信息，为一些情景（如并行执行一个工作流定义的多个版本）提供支持，并为其他功能（如动态更新）提供基础。 该教程中的此步骤演示了如何使用 `WorkflowIdentity` 来同时承载多个版本的工作流。

> [!NOTE]
> 若要下载完整版本或观看教程的视频演练，请参阅[Windows Workflow Foundation （WF45）-入门教程](https://go.microsoft.com/fwlink/?LinkID=248976)。

## <a name="in-this-topic"></a>在本主题中

在该教程的此步骤中，工作流中的 `WriteLine` 活动已得到修改以提供其他信息，并且添加了一个新的 `WriteLine` 活动。 将存储原始工作流程序集的副本，并更新主机应用程序以便其可同时在原始工作流和更新后的工作流中运行。

- [创建 NumberGuessWorkflowActivities 项目的副本](how-to-host-multiple-versions-of-a-workflow-side-by-side.md#BKMK_BackupCopy)

- [更新工作流](how-to-host-multiple-versions-of-a-workflow-side-by-side.md#BKMK_UpdateWorkflows)

  - [更新 StateMachine 工作流](how-to-host-multiple-versions-of-a-workflow-side-by-side.md#BKMK_UpdateStateMachine)

  - [更新流程图工作流](how-to-host-multiple-versions-of-a-workflow-side-by-side.md#BKMK_UpdateFlowchart)

  - [更新顺序工作流](how-to-host-multiple-versions-of-a-workflow-side-by-side.md#BKMK_UpdateSequential)

- [更新 WorkflowVersionMap 以包含以前的工作流版本](how-to-host-multiple-versions-of-a-workflow-side-by-side.md#BKMK_UpdateWorkflowVersionMap)

- [生成并运行应用程序](how-to-host-multiple-versions-of-a-workflow-side-by-side.md#BKMK_BuildAndRun)

> [!NOTE]
> 在执行本主题中的步骤之前，请运行该应用程序，启动每个类型的多个工作流，并针对每个工作流进行一个或两个猜测。 此步骤和下面的步骤中将使用这些保留的[工作流：更新正在运行的工作流实例](how-to-update-the-definition-of-a-running-workflow-instance.md)的定义。

> [!NOTE]
> 入门教程中的每个步骤都依赖于前面的步骤。 如果未完成前面的步骤，则可以从[Windows Workflow Foundation （WF45）-入门教程](https://go.microsoft.com/fwlink/?LinkID=248976)中下载本教程的完整版本。

### <a name="BKMK_BackupCopy"></a>创建 NumberGuessWorkflowActivities 项目的副本

1. 如果未打开，则在 Visual Studio 2012 中打开**WF45GettingStartedTutorial**解决方案。

2. 按 Ctrl+Shift+B 生成解决方案。

3. 关闭**WF45GettingStartedTutorial**解决方案。

4. 打开 Windows 资源管理器并定位到该教程解决方案文件和项目文件夹所在的文件夹。

5. 在与**NumberGuessWorkflowHost**和**NumberGuessWorkflowActivities**相同的文件夹中创建名为**PreviousVersions**的新文件夹。 此文件夹用于放置包含在后续教程步骤中使用的不同版本工作流的程序集。

6. 导航到**NumberGuessWorkflowActivities\bin\debug**文件夹（或**bin\release** ，具体取决于项目设置）。 复制**NumberGuessWorkflowActivities**并将其粘贴到**PreviousVersions**文件夹。

7. 将**PreviousVersions**文件夹中的**NumberGuessWorkflowActivities**重命名为**NumberGuessWorkflowActivities_v1**。

    > [!NOTE]
    > 本主题中的步骤演示了一种对用于包含多个版本的工作流的程序集进行管理的方法。 也可以使用其他方法，例如，对程序集进行强命名，并在全局程序集缓冲中注册这些程序集。

8. 在与**NumberGuessWorkflowHost**、 **NumberGuessWorkflowActivities**相同的文件夹中创建名为**NumberGuessWorkflowActivities_du**的**新文件夹，** 并复制所有文件和子文件夹从**NumberGuessWorkflowActivities**文件夹到新的**NumberGuessWorkflowActivities_du**文件夹。 此项目的备份副本用于初始版本的活动，在[如何：更新正在运行的工作流实例](how-to-update-the-definition-of-a-running-workflow-instance.md)的定义。

9. 重新打开 Visual Studio 2012 中的**WF45GettingStartedTutorial**解决方案。

### <a name="BKMK_UpdateWorkflows"></a>更新工作流

在本节中，将会更新工作流定义。 将会更新针对用户猜测提供反馈的两个 `WriteLine` 活动，并添加一个新的 `WriteLine` 活动，此活动提供猜测了数字之后有关该游戏的其他信息。

#### <a name="BKMK_UpdateStateMachine"></a>更新 StateMachine 工作流

1. 在**解决方案资源管理器**的**NumberGuessWorkflowActivities**项目下，双击**statemachinenumberguessworkflow.xaml**。

2. 双击状态机上的**推测错误**转换。

3. 更新 `Text` 活动中最左侧 `WriteLine` 的 `If`。

    ```vb
    Guess & " is too low."
    ```

    ```csharp
    Guess + " is too low."
    ```

4. 更新 `Text` 活动中最右侧 `WriteLine` 的 `If`。

    ```vb
    Guess & " is too high."
    ```

    ```csharp
    Guess + " is too high."
    ```

5. 通过单击工作流设计器顶部显示的痕迹导航中的 "状态机"，返回到工作流设计**器中的**"总体状态机" 视图。

6. 双击状态机上的 "**猜测正确**转换"。

7. 将 " **WriteLine** " 活动从 "**工具箱**" 的 "**基元**" 部分拖放到转换的 "在**此处放置操作活动**" 标签上。

8. 在 `Text` 属性框中键入以下表达式。

    ```vb
    Guess & " is correct. You guessed it in " & Turns & " turns."
    ```

    ```csharp
    Guess + " is correct. You guessed it in " + Turns + " turns."
    ```

#### <a name="BKMK_UpdateFlowchart"></a>更新流程图工作流

1. 在**解决方案资源管理器**的**NumberGuessWorkflowActivities**项目下，双击**flowchartnumberguessworkflow.xaml**。

2. 更新最左侧 `Text` 活动的 `WriteLine`。

    ```vb
    Guess & " is too low."
    ```

    ```csharp
    Guess + " is too low."
    ```

3. 更新最右侧 `Text` 活动的 `WriteLine`。

    ```vb
    Guess & " is too high."
    ```

    ```csharp
    Guess + " is too high."
    ```

4. 将 " **WriteLine** " 活动从 "**工具箱**" 的 " `True` **基元**" 部分拖放到最顶部`FlowDecision`操作的下拉点上。 `WriteLine` 活动将添加到流程图并链接到 `True` 的 `FlowDecision` 操作。

5. 在 `Text` 属性框中键入以下表达式。

    ```vb
    Guess & " is correct. You guessed it in " & Turns & " turns."
    ```

    ```csharp
    Guess + " is correct. You guessed it in " + Turns + " turns."
    ```

#### <a name="BKMK_UpdateSequential"></a>更新顺序工作流

1. 在**解决方案资源管理器**的**NumberGuessWorkflowActivities**项目下，双击**sequentialnumberguessworkflow.xaml**。

2. 更新 `Text` 活动中最左侧 `WriteLine` 的 `If`。

    ```vb
    Guess & " is too low."
    ```

    ```csharp
    Guess + " is too low."
    ```

3. 更新 `Text` 活动中最右侧 `WriteLine` 活动的 `If`。

    ```vb
    Guess & " is too high."
    ```

    ```csharp
    Guess + " is too high."
    ```

4. 将 " **writeline** " 活动从 "**工具箱**" 的 "**基元**" 部分拖放到 " **DoWhile** " 活动后面，使**WriteLine**成为根`Sequence`活动中的最后一个活动。

5. 在 `Text` 属性框中键入以下表达式。

    ```vb
    Guess & " is correct. You guessed it in " & Turns & " turns."
    ```

    ```csharp
    Guess + " is correct. You guessed it in " + Turns + " turns."
    ```

### <a name="BKMK_UpdateWorkflowVersionMap"></a>更新 WorkflowVersionMap 以包含以前的工作流版本

1. 双击**NumberGuessWorkflowHost**项目下的**WorkflowVersionMap.cs** （或**WorkflowVersionMap**）将其打开。

2. 向包含其他 `using`（或 `Imports`）语句的文件的顶部添加以下 `using`（或 `Imports`）语句。

    ```vb
    Imports System.Reflection
    Imports System.IO
    ```

    ```csharp
    using System.Reflection;
    using System.IO;
    ```

3. 紧接在现有的三个工作流标识声明下面添加三个新工作流标识。 这些新的 `v1` 工作流标识用于为在进行更新前启动的工作流提供正确的工作流定义。

    ```vb
    'Current version identities.
    Public StateMachineNumberGuessIdentity As WorkflowIdentity
    Public FlowchartNumberGuessIdentity As WorkflowIdentity
    Public SequentialNumberGuessIdentity As WorkflowIdentity

    'v1 Identities.
    Public StateMachineNumberGuessIdentity_v1 As WorkflowIdentity
    Public FlowchartNumberGuessIdentity_v1 As WorkflowIdentity
    Public SequentialNumberGuessIdentity_v1 As WorkflowIdentity
    ```

    ```csharp
    // Current version identities.
    static public WorkflowIdentity StateMachineNumberGuessIdentity;
    static public WorkflowIdentity FlowchartNumberGuessIdentity;
    static public WorkflowIdentity SequentialNumberGuessIdentity;

    // v1 identities.
    static public WorkflowIdentity StateMachineNumberGuessIdentity_v1;
    static public WorkflowIdentity FlowchartNumberGuessIdentity_v1;
    static public WorkflowIdentity SequentialNumberGuessIdentity_v1;
    ```

4. 在 `WorkflowVersionMap` 构造函数中，将三个当前工作流标识的 `Version` 属性更新为 `2.0.0.0`。

    ```vb
    'Add the current workflow version identities.
    StateMachineNumberGuessIdentity = New WorkflowIdentity With
    {
        .Name = "StateMachineNumberGuessWorkflow",
        .Version = New Version(2, 0, 0, 0)
    }

    FlowchartNumberGuessIdentity = New WorkflowIdentity With
    {
        .Name = "FlowchartNumberGuessWorkflow",
        .Version = New Version(2, 0, 0, 0)
    }

    SequentialNumberGuessIdentity = New WorkflowIdentity With
    {
        .Name = "SequentialNumberGuessWorkflow",
        .Version = New Version(2, 0, 0, 0)
    }

    map.Add(StateMachineNumberGuessIdentity, New StateMachineNumberGuessWorkflow())
    map.Add(FlowchartNumberGuessIdentity, New FlowchartNumberGuessWorkflow())
    map.Add(SequentialNumberGuessIdentity, New SequentialNumberGuessWorkflow())
    ```

    ```csharp
    // Add the current workflow version identities.
    StateMachineNumberGuessIdentity = new WorkflowIdentity
    {
        Name = "StateMachineNumberGuessWorkflow",
        // Version = new Version(1, 0, 0, 0),
        Version = new Version(2, 0, 0, 0)
    };

    FlowchartNumberGuessIdentity = new WorkflowIdentity
    {
        Name = "FlowchartNumberGuessWorkflow",
        // Version = new Version(1, 0, 0, 0),
        Version = new Version(2, 0, 0, 0)
    };

    SequentialNumberGuessIdentity = new WorkflowIdentity
    {
        Name = "SequentialNumberGuessWorkflow",
        // Version = new Version(1, 0, 0, 0),
        Version = new Version(2, 0, 0, 0)
    };

    map.Add(StateMachineNumberGuessIdentity, new StateMachineNumberGuessWorkflow());
    map.Add(FlowchartNumberGuessIdentity, new FlowchartNumberGuessWorkflow());
    map.Add(SequentialNumberGuessIdentity, new SequentialNumberGuessWorkflow());
    ```

    向字典添加当前版本的工作流的代码使用项目中引用的当前版本，因此，无需对初始化工作流定义的代码进行更新。

5. 紧接在向字典添加当前版本的代码的后面添加以下代码。

    ```vb
    'Initialize the previous workflow version identities.
    StateMachineNumberGuessIdentity_v1 = New WorkflowIdentity With
    {
        .Name = "StateMachineNumberGuessWorkflow",
        .Version = New Version(1, 0, 0, 0)
    }

    FlowchartNumberGuessIdentity_v1 = New WorkflowIdentity With
    {
        .Name = "FlowchartNumberGuessWorkflow",
        .Version = New Version(1, 0, 0, 0)
    }

    SequentialNumberGuessIdentity_v1 = New WorkflowIdentity With
    {
        .Name = "SequentialNumberGuessWorkflow",
        .Version = New Version(1, 0, 0, 0)
    }
    ```

    ```csharp
    // Initialize the previous workflow version identities.
    StateMachineNumberGuessIdentity_v1 = new WorkflowIdentity
    {
        Name = "StateMachineNumberGuessWorkflow",
        Version = new Version(1, 0, 0, 0)
    };

    FlowchartNumberGuessIdentity_v1 = new WorkflowIdentity
    {
        Name = "FlowchartNumberGuessWorkflow",
        Version = new Version(1, 0, 0, 0)
    };

    SequentialNumberGuessIdentity_v1 = new WorkflowIdentity
    {
        Name = "SequentialNumberGuessWorkflow",
        Version = new Version(1, 0, 0, 0)
    };
    ```

    这些工作流标识与相应工作流定义的初始版本相关联。

6. 接下来，加载包含初始版本的工作流定义的程序集，然后创建相应工作流定义，并将这些定义添加到字典。

    ```vb
    'Add the previous version workflow identities to the dictionary along with
    'the corresponding workflow definitions loaded from the v1 assembly.
    'Assembly.LoadFile requires an absolute path so convert this relative path
    'to an absolute path.
    Dim v1AssemblyPath As String = "..\..\..\PreviousVersions\NumberGuessWorkflowActivities_v1.dll"
    v1AssemblyPath = Path.GetFullPath(v1AssemblyPath)
    Dim v1Assembly As Assembly = Assembly.LoadFile(v1AssemblyPath)

    map.Add(StateMachineNumberGuessIdentity_v1,
        v1Assembly.CreateInstance("NumberGuessWorkflowActivities.StateMachineNumberGuessWorkflow"))

    map.Add(SequentialNumberGuessIdentity_v1,
        v1Assembly.CreateInstance("NumberGuessWorkflowActivities.SequentialNumberGuessWorkflow"))

    map.Add(FlowchartNumberGuessIdentity_v1,
        v1Assembly.CreateInstance("NumberGuessWorkflowActivities.FlowchartNumberGuessWorkflow"))
    ```

    ```csharp
    // Add the previous version workflow identities to the dictionary along with
    // the corresponding workflow definitions loaded from the v1 assembly.
    // Assembly.LoadFile requires an absolute path so convert this relative path
    // to an absolute path.
    string v1AssemblyPath = @"..\..\..\PreviousVersions\NumberGuessWorkflowActivities_v1.dll";
    v1AssemblyPath = Path.GetFullPath(v1AssemblyPath);
    Assembly v1Assembly = Assembly.LoadFile(v1AssemblyPath);

    map.Add(StateMachineNumberGuessIdentity_v1,
        v1Assembly.CreateInstance("NumberGuessWorkflowActivities.StateMachineNumberGuessWorkflow") as Activity);

    map.Add(SequentialNumberGuessIdentity_v1,
        v1Assembly.CreateInstance("NumberGuessWorkflowActivities.SequentialNumberGuessWorkflow") as Activity);

    map.Add(FlowchartNumberGuessIdentity_v1,
        v1Assembly.CreateInstance("NumberGuessWorkflowActivities.FlowchartNumberGuessWorkflow") as Activity);
    ```

    下面的示例完整列出了已更新的 `WorkflowVersionMap` 类。

    ```vb
    Public Module WorkflowVersionMap
        Dim map As Dictionary(Of WorkflowIdentity, Activity)

        'Current version identities.
        Public StateMachineNumberGuessIdentity As WorkflowIdentity
        Public FlowchartNumberGuessIdentity As WorkflowIdentity
        Public SequentialNumberGuessIdentity As WorkflowIdentity

        'v1 Identities.
        Public StateMachineNumberGuessIdentity_v1 As WorkflowIdentity
        Public FlowchartNumberGuessIdentity_v1 As WorkflowIdentity
        Public SequentialNumberGuessIdentity_v1 As WorkflowIdentity

        Sub New()
            map = New Dictionary(Of WorkflowIdentity, Activity)

            'Add the current workflow version identities.
            StateMachineNumberGuessIdentity = New WorkflowIdentity With
            {
                .Name = "StateMachineNumberGuessWorkflow",
                .Version = New Version(2, 0, 0, 0)
            }

            FlowchartNumberGuessIdentity = New WorkflowIdentity With
            {
                .Name = "FlowchartNumberGuessWorkflow",
                .Version = New Version(2, 0, 0, 0)
            }

            SequentialNumberGuessIdentity = New WorkflowIdentity With
            {
                .Name = "SequentialNumberGuessWorkflow",
                .Version = New Version(2, 0, 0, 0)
            }

            map.Add(StateMachineNumberGuessIdentity, New StateMachineNumberGuessWorkflow())
            map.Add(FlowchartNumberGuessIdentity, New FlowchartNumberGuessWorkflow())
            map.Add(SequentialNumberGuessIdentity, New SequentialNumberGuessWorkflow())

            'Initialize the previous workflow version identities.
            StateMachineNumberGuessIdentity_v1 = New WorkflowIdentity With
            {
                .Name = "StateMachineNumberGuessWorkflow",
                .Version = New Version(1, 0, 0, 0)
            }

            FlowchartNumberGuessIdentity_v1 = New WorkflowIdentity With
            {
                .Name = "FlowchartNumberGuessWorkflow",
                .Version = New Version(1, 0, 0, 0)
            }

            SequentialNumberGuessIdentity_v1 = New WorkflowIdentity With
            {
                .Name = "SequentialNumberGuessWorkflow",
                .Version = New Version(1, 0, 0, 0)
            }

            'Add the previous version workflow identities to the dictionary along with
            'the corresponding workflow definitions loaded from the v1 assembly.
            'Assembly.LoadFile requires an absolute path so convert this relative path
            'to an absolute path.
            Dim v1AssemblyPath As String = "..\..\..\PreviousVersions\NumberGuessWorkflowActivities_v1.dll"
            v1AssemblyPath = Path.GetFullPath(v1AssemblyPath)
            Dim v1Assembly As Assembly = Assembly.LoadFile(v1AssemblyPath)

            map.Add(StateMachineNumberGuessIdentity_v1,
                v1Assembly.CreateInstance("NumberGuessWorkflowActivities.StateMachineNumberGuessWorkflow"))

            map.Add(SequentialNumberGuessIdentity_v1,
                v1Assembly.CreateInstance("NumberGuessWorkflowActivities.SequentialNumberGuessWorkflow"))

            map.Add(FlowchartNumberGuessIdentity_v1,
                v1Assembly.CreateInstance("NumberGuessWorkflowActivities.FlowchartNumberGuessWorkflow"))
        End Sub

        Public Function GetWorkflowDefinition(identity As WorkflowIdentity) As Activity
            Return map(identity)
        End Function

        Public Function GetIdentityDescription(identity As WorkflowIdentity) As String
            Return identity.ToString()
        End Function
    End Module
    ```

    ```csharp
    public static class WorkflowVersionMap
    {
        static Dictionary<WorkflowIdentity, Activity> map;

        // Current version identities.
        static public WorkflowIdentity StateMachineNumberGuessIdentity;
        static public WorkflowIdentity FlowchartNumberGuessIdentity;
        static public WorkflowIdentity SequentialNumberGuessIdentity;

        // v1 identities.
        static public WorkflowIdentity StateMachineNumberGuessIdentity_v1;
        static public WorkflowIdentity FlowchartNumberGuessIdentity_v1;
        static public WorkflowIdentity SequentialNumberGuessIdentity_v1;

        static WorkflowVersionMap()
        {
            map = new Dictionary<WorkflowIdentity, Activity>();

            // Add the current workflow version identities.
            StateMachineNumberGuessIdentity = new WorkflowIdentity
            {
                Name = "StateMachineNumberGuessWorkflow",
                // Version = new Version(1, 0, 0, 0),
                Version = new Version(2, 0, 0, 0)
            };

            FlowchartNumberGuessIdentity = new WorkflowIdentity
            {
                Name = "FlowchartNumberGuessWorkflow",
                // Version = new Version(1, 0, 0, 0),
                Version = new Version(2, 0, 0, 0)
            };

            SequentialNumberGuessIdentity = new WorkflowIdentity
            {
                Name = "SequentialNumberGuessWorkflow",
                // Version = new Version(1, 0, 0, 0),
                Version = new Version(2, 0, 0, 0)
            };

            map.Add(StateMachineNumberGuessIdentity, new StateMachineNumberGuessWorkflow());
            map.Add(FlowchartNumberGuessIdentity, new FlowchartNumberGuessWorkflow());
            map.Add(SequentialNumberGuessIdentity, new SequentialNumberGuessWorkflow());

            // Initialize the previous workflow version identities.
            StateMachineNumberGuessIdentity_v1 = new WorkflowIdentity
            {
                Name = "StateMachineNumberGuessWorkflow",
                Version = new Version(1, 0, 0, 0)
            };

            FlowchartNumberGuessIdentity_v1 = new WorkflowIdentity
            {
                Name = "FlowchartNumberGuessWorkflow",
                Version = new Version(1, 0, 0, 0)
            };

            SequentialNumberGuessIdentity_v1 = new WorkflowIdentity
            {
                Name = "SequentialNumberGuessWorkflow",
                Version = new Version(1, 0, 0, 0)
            };

            // Add the previous version workflow identities to the dictionary along with
            // the corresponding workflow definitions loaded from the v1 assembly.
            // Assembly.LoadFile requires an absolute path so convert this relative path
            // to an absolute path.
            string v1AssemblyPath = @"..\..\..\PreviousVersions\NumberGuessWorkflowActivities_v1.dll";
            v1AssemblyPath = Path.GetFullPath(v1AssemblyPath);
            Assembly v1Assembly = Assembly.LoadFile(v1AssemblyPath);

            map.Add(StateMachineNumberGuessIdentity_v1,
                v1Assembly.CreateInstance("NumberGuessWorkflowActivities.StateMachineNumberGuessWorkflow") as Activity);

            map.Add(SequentialNumberGuessIdentity_v1,
                v1Assembly.CreateInstance("NumberGuessWorkflowActivities.SequentialNumberGuessWorkflow") as Activity);

            map.Add(FlowchartNumberGuessIdentity_v1,
                v1Assembly.CreateInstance("NumberGuessWorkflowActivities.FlowchartNumberGuessWorkflow") as Activity);
        }

        public static Activity GetWorkflowDefinition(WorkflowIdentity identity)
        {
            return map[identity];
        }

        public static string GetIdentityDescription(WorkflowIdentity identity)
        {
            return identity.ToString();
        }
    }
    ```

### <a name="BKMK_BuildAndRun"></a> 生成并运行应用程序

1. 按 Ctrl+Shift+B 以生成应用程序，然后按 Ctrl+F5 启动该应用程序。

2. 通过单击 "**新游戏**" 启动新工作流。 工作流的版本显示在状态窗口下面，它反映了关联 `WorkflowIdentity` 的更新版本。 请记下 `InstanceId`，这样您就可以在工作流完成时查看该工作流的跟踪文件，然后在游戏完成之前输入猜测。 请注意用户猜测基于 `WriteLine` 活动的更新在状态窗口中的信息中显示的方式。

    ```console
    Please enter a number between 1 and 10
    5 is too high.
    Please enter a number between 1 and 10
    3 is too high.
    Please enter a number between 1 and 10
    1 is too low.
    Please enter a number between 1 and 10
    Congratulations, you guessed the number in 4 turns.
    ```

    > [!NOTE]
    > 将显示 `WriteLine` 活动的更新文本，但不显示已在本主题中添加的最终 `WriteLine` 活动的输出。 这是因为，此状态窗口将由 `PersistableIdle` 处理程序进行更新。 由于该工作流在最终活动之后完成且不会转为空闲状态，因此不会调用 `PersistableIdle` 处理程序。 但是，`Completed` 处理程序会在状态窗口中显示类似的消息。 如果需要，可以向 `Completed` 处理程序添加代码，以便从 `StringWriter` 提取文本并在状态窗口中显示。

3. 打开 Windows 资源管理器并导航到**NumberGuessWorkflowHost\bin\debug**文件夹（或**bin\release** ，具体取决于项目设置），并使用与已完成的工作流相对应的 Notepad 打开跟踪文件。 如果未记下`InstanceId`，可以使用 Windows 资源管理器中的 "**修改日期**" 信息来识别正确的跟踪文件。

    ```console
    Please enter a number between 1 and 10
    5 is too high.
    Please enter a number between 1 and 10
    3 is too high.
    Please enter a number between 1 and 10
    1 is too low.
    Please enter a number between 1 and 10
    2 is correct. You guessed it in 4 turns.
    ```

    该跟踪文件中包含已更新的 `WriteLine` 输出，包括在本主题中添加的 `WriteLine` 的输出。

4. 切换回数字猜测应用程序，然后选择在进行更新前启动的一个工作流。 您可通过查看在状态窗口下面显示的版本信息来识别当前选择的工作流的版本。 输入一些猜测，并注意状态更新与前一个版本的 `WriteLine` 活动输出相符，并且不包括用户猜测。 这是因为，这些工作流使用的是不具有 `WriteLine` 更新的前一个工作流定义。

    在下一步中[，如何：更新正在运行的工作流实例](how-to-update-the-definition-of-a-running-workflow-instance.md)的定义，将更新正在运行`v1`的工作流实例，使其包含新`v2`功能作为实例。
