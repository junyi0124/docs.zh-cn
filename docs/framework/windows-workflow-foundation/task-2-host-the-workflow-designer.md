---
title: 任务 2：承载工作流设计器
ms.date: 03/30/2017
ms.assetid: 0a29b138-270d-4846-b78e-2b875e34e501
ms.openlocfilehash: 8e4c17ed182cec7748b9a1f11f76ff90aa60c39e
ms.sourcegitcommit: 5fb5b6520b06d7f5e6131ec2ad854da302a28f2e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/03/2019
ms.locfileid: "74715781"
---
# <a name="task-2-host-the-workflow-designer"></a>任务 2：承载工作流设计器

本主题介绍在 Windows Presentation Foundation （WPF）应用程序中承载 Windows 工作流设计器的实例的过程。

此过程配置包含设计器的**网格**控件，以编程方式创建 <xref:System.Activities.Presentation.WorkflowDesigner> 的实例，该实例包含默认的 <xref:System.Activities.Statements.Sequence> 活动，注册设计器元数据以为所有内置活动提供设计器支持，并在 WPF 应用程序中承载工作流设计器。

## <a name="to-host-the-workflow-designer"></a>承载工作流设计器

1. 打开你在[任务1：创建新的 Windows Presentation Foundation 应用程序](task-1-create-a-new-wpf-app.md)中创建的 HostingApplication 项目。

2. 调整窗口的大小，使其更易于使用工作流设计器。 为此，请在设计器中选择 " **mainwindow.xaml** "，按 F4 以显示 "**属性**" 窗口，并在 "**布局**" 部分中，将 "**宽度**" 设置为600，将 "**高度**" 设置为值350。

3. 设置网格名称，方法是在设计器中选择 "**网格**" 面板（单击**mainwindow.xaml**中的框），然后将 "**属性**" 窗口顶部的 "**名称**" 属性设置为 "grid1"。

4. 在 "**属性**" 窗口中，单击 "`ColumnDefinitions`" 属性旁边的省略号（ **...** ）以打开 "**集合编辑器**" 对话框。

5. 在 "**集合编辑器**" 对话框中，单击 "**添加**" 按钮三次，以便在布局中插入三列。 第一列将包含 "**工具箱**"，第二列将承载工作流设计器，第三列将用于属性检查器。

6. 将中间列的 `Width` 属性设置为值 "4 *"。

7. 单击“确定” 以保存更改。 以下 XAML 将添加到*mainwindow.xaml*文件中：

    ```xaml
    <Grid Name="grid1">
        <Grid.ColumnDefinitions>
            <ColumnDefinition />
            <ColumnDefinition Width="4*" />
            <ColumnDefinition />
        </Grid.ColumnDefinitions>
    </Grid>
    ```

8. 在**解决方案资源管理器**中，右键单击*mainwindow.xaml* ，然后选择 "**查看代码**"。 按照以下这些步骤修改代码：

    1. 添加下列命名空间：

        ```csharp
        using System.Activities;
        using System.Activities.Core.Presentation;
        using System.Activities.Presentation;
        using System.Activities.Presentation.Metadata;
        using System.Activities.Presentation.Toolbox;
        using System.Activities.Statements;
        using System.ComponentModel;
        ```

    2. 若要声明一个私有成员字段以保存 <xref:System.Activities.Presentation.WorkflowDesigner>的实例，请将以下代码添加到 `MainWindow` 类：

        ```csharp
        public partial class MainWindow : Window
        {
            private WorkflowDesigner wd;

            public MainWindow()
            {
                InitializeComponent();
            }
        }
        ```

    3. 将下面的 `AddDesigner` 方法添加到 `MainWindow` 类。 实现创建 <xref:System.Activities.Presentation.WorkflowDesigner>的实例，向其中添加 <xref:System.Activities.Statements.Sequence> 活动，并将其放在 grid1**网格**的中间列中。

        ```csharp
        private void AddDesigner()
        {
            // Create an instance of WorkflowDesigner class.
            this.wd = new WorkflowDesigner();

            // Place the designer canvas in the middle column of the grid.
            Grid.SetColumn(this.wd.View, 1);

            // Load a new Sequence as default.
            this.wd.Load(new Sequence());

            // Add the designer canvas to the grid.
            grid1.Children.Add(this.wd.View);
        }
        ```

    4. 注册设计器元数据，为所有内置活动添加设计器支持。 这使您可以将活动从 "工具箱" 拖放到工作流设计器中的原始 <xref:System.Activities.Statements.Sequence> 活动。 为此，请将 `RegisterMetadata` 方法添加到 `MainWindow` 类：

        ```csharp
        private void RegisterMetadata()
        {
            var dm = new DesignerMetadata();
            dm.Register();
        }
        ```

        有关注册活动设计器的详细信息，请参阅[如何：创建自定义活动设计器](how-to-create-a-custom-activity-designer.md)。

    5. 在 `MainWindow` 类构造函数中，添加对上文声明的方法的调用，以便注册设计器支持元数据，并创建 <xref:System.Activities.Presentation.WorkflowDesigner>。

        ```csharp
        public MainWindow()
        {
            InitializeComponent();

            // Register the metadata.
            RegisterMetadata();

            // Add the WFF Designer.
            AddDesigner();
        }
        ```

        > [!NOTE]
        > `RegisterMetadata` 方法注册包含 <xref:System.Activities.Statements.Sequence> 活动的内置活动的设计器元数据。 由于 `AddDesigner` 方法使用 <xref:System.Activities.Statements.Sequence> 活动，因此必须先调用 `RegisterMetadata` 方法。

9. 按<kbd>F5</kbd>生成并运行解决方案。

10. 请参阅[任务3：创建 "工具箱" 和 "PropertyGrid" 窗格](task-3-create-the-toolbox-and-propertygrid-panes.md)，了解如何向重新承载工作流设计器添加 **"工具箱" 和 "** **PropertyGrid** " 支持。

## <a name="see-also"></a>另请参阅

- [重新托管工作流设计器](rehosting-the-workflow-designer.md)
- [任务 1：新建 Windows Presentation Foundation 应用程序](task-1-create-a-new-wpf-app.md)
- [任务 3：创建“工具箱”和“属性网格”窗格](task-3-create-the-toolbox-and-propertygrid-panes.md)
