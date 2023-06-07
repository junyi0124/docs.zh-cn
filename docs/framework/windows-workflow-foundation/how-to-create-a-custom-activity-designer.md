---
title: 如何：创建自定义活动设计器
description: 本文介绍如何创建一个工作流基础自定义活动设计器，该设计器具有放置区，可在其中放置任意活动。
ms.date: 03/30/2017
ms.assetid: 2f3aade6-facc-44ef-9657-a407ef8b9b31
ms.openlocfilehash: 015efd1e482e2b531d28b9caec411c76116c9653
ms.sourcegitcommit: 9a4488a3625866335e83a20da5e9c5286b1f034c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2020
ms.locfileid: "83419780"
---
# <a name="how-to-create-a-custom-activity-designer"></a>如何：创建自定义活动设计器

通常实现自定义活动设计器，以便其相关活动可与其他活动（这些活动的设计器可与它们一起放置到设计图面上）组合在一起。 此功能要求自定义活动设计器提供一个可放置任意活动的 "放置区"，以及用于管理设计图面上生成的元素集合的方法。 本主题介绍如何创建一个包含上述放置区的自定义活动设计器，以及如何创建一个可提供管理设计器元素集合所需的编辑功能的自定义活动设计器。

自定义活动设计器通常继承自 <xref:System.Activities.Presentation.ActivityDesigner>，后者是任何无特定设计器的活动的默认基活动设计器类型。 此类型提供与属性网格交互以及配置管理颜色和图标等基本方面的设计时体验。

<xref:System.Activities.Presentation.ActivityDesigner> 采用两个帮助器控件，即 <xref:System.Activities.Presentation.WorkflowItemPresenter> 和 <xref:System.Activities.Presentation.WorkflowItemsPresenter>，以便更易于开发自定义活动设计器。 这两个控件处理常用功能，例如，拖放子元素，删除、选择和添加这些子元素。 <xref:System.Activities.Presentation.WorkflowItemPresenter>允许内部的单个子 UI 元素（提供 "放置区域"），而 <xref:System.Activities.Presentation.WorkflowItemsPresenter> 可以提供支持多个 UI 元素，包括排序、移动、删除和添加子元素等附加功能。

在实现自定义活动设计器的过程中，需要突出显示的情景的其他关键部分考虑到，使用 WPF 数据绑定将视觉对象编辑绑定到在设计器中编辑的内容的内存中存储的实例。 这是通过模型项树实现的，它还负责启用更改通知和跟踪事件（例如状态的更改）。

本主题概述了两个过程。

1. 第一个过程介绍如何使用 <xref:System.Activities.Presentation.WorkflowItemPresenter> 创建一个自定义活动设计器，用于提供接收其他活动的放置区。 此过程基于[自定义复合设计器—工作流项呈现器](./samples/custom-composite-designers-workflow-item-presenter.md)示例。

2. 第二个过程介绍如何使用 <xref:System.Activities.Presentation.WorkflowItemsPresenter> 创建一个自定义活动设计器，用于提供编辑包含的元素的集合所需的功能。 此过程基于[自定义复合设计器—工作流项演示者](./samples/custom-composite-designers-workflow-items-presenter.md)示例。

## <a name="to-create-a-custom-activity-designer-with-a-drop-zone-using-workflowitempresenter"></a>使用 WorkflowItemPresenter 创建带有放置区的自定义活动设计器

1. 启动 Visual Studio 2010。

2. 在 "**文件**" 菜单上，指向 "**新建**"，然后选择 "**项目 ...**"。

     **“新建项目”** 对话框随即打开。

3. 在 "**已安装的模板**" 窗格中，从首选语言类别中选择 " **Windows** "。

4. 在 "**模板**" 窗格中，选择 " **WPF 应用程序**"。

5. 在 "**名称**" 框中，输入 `UsingWorkflowItemPresenter` 。

6. 在 "**位置**" 框中，输入要保存项目的目录，或者单击 "**浏览**" 导航到该目录。

7. 在 "**解决方案**" 框中，接受默认值。

8. 单击“确定” 。

9. 右键单击**解决方案资源管理器**中*的右击 mainwindows.xaml*文件，在 " **Microsoft Visual Studio** " 对话框中选择 "**删除**并确认 **" 确定 "** 。

10. 在**解决方案资源管理器**中右键单击 "右击 usingworkflowitempresenter" 项目，依次选择 "**添加**"、"**新建项 ...** " 若要打开 "**添加新项**" 对话框，请从左侧的 "**已安装的模板**" 部分中选择 " **WPF** " 类别。

11. 选择 "**窗口（WPF）** " 模板，将其命名为 `RehostingWFDesigner` ，然后单击 "**添加**"。

12. 打开*rehostingwfdesigner.xaml*文件并将以下代码粘贴到其中以定义应用程序的 UI：

    ```xaml
    <Window x:Class=" UsingWorkflowItemPresenter.RehostingWFDesigner"
            xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:sapt="clr-namespace:System.Activities.Presentation.Toolbox;assembly=System.Activities.Presentation"
            xmlns:sys="clr-namespace:System;assembly=mscorlib"
            Title="Window1" Height="600" Width="900">
        <Window.Resources>
            <sys:String x:Key="AssemblyName">System.Activities, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35</sys:String>
        </Window.Resources>
        <Grid>
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="2*"/>
                <ColumnDefinition Width="7*"/>
                <ColumnDefinition Width="3*"/>
            </Grid.ColumnDefinitions>
            <Border Grid.Column="0">
                <sapt:ToolboxControl Name="Toolbox">
                    <sapt:ToolboxCategory CategoryName="Basic">
                        <sapt:ToolboxItemWrapper AssemblyName="{StaticResource AssemblyName}" >
                            <sapt:ToolboxItemWrapper.ToolName>
                                System.Activities.Statements.Sequence
                            </sapt:ToolboxItemWrapper.ToolName>
                           </sapt:ToolboxItemWrapper>
                        <sapt:ToolboxItemWrapper  AssemblyName="{StaticResource AssemblyName}">
                            <sapt:ToolboxItemWrapper.ToolName>
                                System.Activities.Statements.WriteLine
                            </sapt:ToolboxItemWrapper.ToolName>

                        </sapt:ToolboxItemWrapper>
                        <sapt:ToolboxItemWrapper  AssemblyName="{StaticResource AssemblyName}">
                            <sapt:ToolboxItemWrapper.ToolName>
                                System.Activities.Statements.If
                            </sapt:ToolboxItemWrapper.ToolName>

                        </sapt:ToolboxItemWrapper>
                        <sapt:ToolboxItemWrapper  AssemblyName="{StaticResource AssemblyName}">
                            <sapt:ToolboxItemWrapper.ToolName>
                                System.Activities.Statements.While
                            </sapt:ToolboxItemWrapper.ToolName>

                        </sapt:ToolboxItemWrapper>
                    </sapt:ToolboxCategory>
                </sapt:ToolboxControl>
            </Border>
            <Border Grid.Column="1" Name="DesignerBorder"/>
            <Border Grid.Column="2" Name="PropertyBorder"/>
        </Grid>
    </Window>
    ```

13. 若要将活动设计器与活动类型相关联，您必须向元数据存储注册活动设计器。 为此，请将 `RegisterMetadata` 方法添加到 `RehostingWFDesigner` 类中。 在 `RegisterMetadata` 方法的范围内，创建一个 <xref:System.Activities.Presentation.Metadata.AttributeTableBuilder> 对象，并调用 <xref:System.Activities.Presentation.Metadata.AttributeTableBuilder.AddCustomAttributes%2A> 方法以向其添加特性。 调用 <xref:System.Activities.Presentation.Metadata.MetadataStore.AddAttributeTable%2A> 方法以向元数据存储添加 <xref:System.Activities.Presentation.Metadata.AttributeTable>。 下面的代码包含设计器的重新承载逻辑。 它注册元数据，将 `SimpleNativeActivity` 放入工具箱中，并创建工作流。 将此代码放入*RehostingWFDesigner.xaml.cs*文件。

    ```csharp
    using System;
    using System.Activities.Core.Presentation;
    using System.Activities.Presentation;
    using System.Activities.Presentation.Metadata;
    using System.Activities.Presentation.Toolbox;
    using System.Activities.Statements;
    using System.ComponentModel;
    using System.Windows;

    namespace UsingWorkflowItemPresenter
    {
        // Interaction logic for RehostingWFDesigner.xaml
        public partial class RehostingWFDesigner
        {
            public RehostingWFDesigner()
            {
                InitializeComponent();
            }

            protected override void OnInitialized(EventArgs e)
            {
                base.OnInitialized(e);
                // Register metadata.
                (new DesignerMetadata()).Register();
                RegisterCustomMetadata();
                // Add custom activity to toolbox.
                Toolbox.Categories.Add(new ToolboxCategory("Custom activities"));
                Toolbox.Categories[1].Add(new ToolboxItemWrapper(typeof(SimpleNativeActivity)));

                // Create the workflow designer.
                var wd = new WorkflowDesigner();
                wd.Load(new Sequence());
                DesignerBorder.Child = wd.View;
                PropertyBorder.Child = wd.PropertyInspectorView;

            }

            void RegisterCustomMetadata()
            {
                var builder = new AttributeTableBuilder();
                builder.AddCustomAttributes(typeof(SimpleNativeActivity), new DesignerAttribute(typeof(SimpleNativeDesigner)));
                MetadataStore.AddAttributeTable(builder.CreateTable());
            }
        }
    }
    ```

14. 在解决方案资源管理器中右键单击 "引用" 目录，然后选择 "**添加引用 ...** " 显示 "**添加引用**" 对话框。

15. 单击 " **.net** " 选项卡，找到名为 " **system.web**" 的程序集，选择它并单击 **"确定"**。

16. 使用相同的过程，添加对下列程序集的引用：

    1. System.Data.DataSetExtensions.dll

    2. System.Activities.Presentation.dll

    3. System.ServiceModel.Activities.dll

17. 打开*app.xaml*文件，将 system.windows.application.startupuri 的值更改为 "rehostingwfdesigner.xaml"。

18. 在**解决方案资源管理器**中右键单击 "右击 usingworkflowitempresenter" 项目，依次选择 "**添加**"、"**新建项 ...** " 若要打开 "**添加新项**" 对话框，请选择**左侧的**"**已安装的模板**" 部分。

19. 选择 "**活动设计器**" 模板，将其命名为 `SimpleNativeDesigner` ，然后单击 "**添加**"。

20. 打开*SimpleNativeDesigner*文件并将以下代码粘贴到其中。 请注意，此代码使用 <xref:System.Activities.Presentation.ActivityDesigner> 作为根元素，并演示如何使用绑定将 <xref:System.Activities.Presentation.WorkflowItemPresenter> 集成到设计器中，以便可以在复合活动设计器中显示子类型。

    > [!NOTE]
    > <xref:System.Activities.Presentation.ActivityDesigner> 的架构只允许向自定义活动设计器定义中添加一个子元素；不过，这个元素可以是 `StackPanel`、`Grid` 或某些其他复合 UI 元素。

    ```xaml
    <sap:ActivityDesigner x:Class=" UsingWorkflowItemPresenter.SimpleNativeDesigner"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:sap="clr-namespace:System.Activities.Presentation;assembly=System.Activities.Presentation"
        xmlns:sapv="clr-namespace:System.Activities.Presentation.View;assembly=System.Activities.Presentation">
        <sap:ActivityDesigner.Resources>
            <DataTemplate x:Key="Collapsed">
                <StackPanel>
                    <TextBlock>This is the collapsed view</TextBlock>
                </StackPanel>
            </DataTemplate>
            <DataTemplate x:Key="Expanded">
                <StackPanel>
                    <TextBlock>Custom Text</TextBlock>
                    <sap:WorkflowItemPresenter Item="{Binding Path=ModelItem.Body, Mode=TwoWay}"
                                            HintText="Please drop an activity here" />
                </StackPanel>
            </DataTemplate>
            <Style x:Key="ExpandOrCollapsedStyle" TargetType="{x:Type ContentPresenter}">
                <Setter Property="ContentTemplate" Value="{DynamicResource Collapsed}"/>
                <Style.Triggers>
                    <DataTrigger Binding="{Binding Path=ShowExpanded}" Value="true">
                        <Setter Property="ContentTemplate" Value="{DynamicResource Expanded}"/>
                    </DataTrigger>
                </Style.Triggers>
            </Style>
        </sap:ActivityDesigner.Resources>
        <Grid>
            <ContentPresenter Style="{DynamicResource ExpandOrCollapsedStyle}" Content="{Binding}" />
        </Grid>
    </sap:ActivityDesigner>
    ```

21. 在**解决方案资源管理器**中右键单击 "右击 usingworkflowitempresenter" 项目，依次选择 "**添加**"、"**新建项 ...** " 若要打开 "**添加新项**" 对话框，请选择**左侧的**"**已安装的模板**" 部分。

22. 选择 "**代码" 活动**模板，将其命名为 `SimpleNativeActivity` ，然后单击 "**添加**"。

23. 通过在 `SimpleNativeActivity` *SimpleNativeActivity.cs*文件中输入以下代码来实现类：

    ```csharp
    using System.Activities;

    namespace UsingWorkflowItemPresenter
    {
        public sealed class SimpleNativeActivity : NativeActivity
        {
            // this property contains an activity that will be scheduled in the execute method
            // the WorkflowItemPresenter in the designer is bound to this to enable editing
            // of the value
            public Activity Body { get; set; }

            protected override void CacheMetadata(NativeActivityMetadata metadata)
            {
               metadata.AddChild(Body);
               base.CacheMetadata(metadata);

            }

            protected override void Execute(NativeActivityContext context)
            {
                context.ScheduleActivity(Body);
            }
        }
    }
    ```

24. 从 "**生成**" 菜单中选择 "**生成解决方案**"。

25. 从 "**调试**" 菜单中选择 "**启动（不调试**）" 以打开 "重新承载自定义设计" 窗口。

### <a name="to-create-a-custom-activity-designer-using-workflowitemspresenter"></a>使用 WorkflowItemsPresenter 创建自定义活动设计器

1. 第二个自定义活动设计器的过程与第二个修改类似，第一种是对第二个应用程序进行命名 `UsingWorkflowItemsPresenter` 。 另外，此应用程序不会定义新的自定义活动。

2. 主要区别包含在*CustomParallelDesigner*和*RehostingWFDesigner.xaml.cs*文件中。 下面是*CustomParallelDesigner*文件中用于定义 UI 的代码：

    ```xaml
    <sap:ActivityDesigner x:Class=" UsingWorkflowItemsPresenter.CustomParallelDesigner"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:sap="clr-namespace:System.Activities.Presentation;assembly=System.Activities.Presentation"
        xmlns:sapv="clr-namespace:System.Activities.Presentation.View;assembly=System.Activities.Presentation">
        <sap:ActivityDesigner.Resources>
            <DataTemplate x:Key="Collapsed">
                <TextBlock>This is the Collapsed View</TextBlock>
            </DataTemplate>
            <DataTemplate x:Key="Expanded">
                <StackPanel>
                    <TextBlock HorizontalAlignment="Center">This is the</TextBlock>
                    <TextBlock HorizontalAlignment="Center">extended view</TextBlock>
                    <sap:WorkflowItemsPresenter HintText="Drop Activities Here"
                                        Items="{Binding Path=ModelItem.Branches}">
                        <sap:WorkflowItemsPresenter.SpacerTemplate>
                            <DataTemplate>
                                <Ellipse Width="10" Height="10" Fill="Black"/>
                            </DataTemplate>
                        </sap:WorkflowItemsPresenter.SpacerTemplate>
                        <sap:WorkflowItemsPresenter.ItemsPanel>
                            <ItemsPanelTemplate>
                                <StackPanel Orientation="Horizontal"/>
                            </ItemsPanelTemplate>
                        </sap:WorkflowItemsPresenter.ItemsPanel>
                    </sap:WorkflowItemsPresenter>
                </StackPanel>
            </DataTemplate>
            <Style x:Key="ExpandOrCollapsedStyle" TargetType="{x:Type ContentPresenter}">
                <Setter Property="ContentTemplate" Value="{DynamicResource Collapsed}"/>
                <Style.Triggers>
                    <DataTrigger Binding="{Binding Path=ShowExpanded}" Value="true">
                        <Setter Property="ContentTemplate" Value="{DynamicResource Expanded}"/>
                    </DataTrigger>
                </Style.Triggers>
            </Style>
        </sap:ActivityDesigner.Resources>
        <Grid>
            <ContentPresenter Style="{DynamicResource ExpandOrCollapsedStyle}" Content="{Binding}"/>
        </Grid>
    </sap:ActivityDesigner>
    ```

3. 下面是*RehostingWFDesigner.xaml.cs*文件中的代码，提供重新承载逻辑：

    ```csharp
    using System;
    using System.Activities.Core.Presentation;
    using System.Activities.Presentation;
    using System.Activities.Presentation.Metadata;
    using System.Activities.Statements;
    using System.ComponentModel;
    using System.Windows;

    namespaceUsingWorkflowItemsPresenter
    {
        public partial class RehostingWfDesigner : Window
        {
            public RehostingWfDesigner()
            {
                InitializeComponent();
            }

            protected override void OnInitialized(EventArgs e)
            {
                base.OnInitialized(e);
                // Register metadata.
                (new DesignerMetadata()).Register();
                RegisterCustomMetadata();

                // Create the workflow designer.
                var wd = new WorkflowDesigner();
                wd.Load(new Sequence());
                DesignerBorder.Child = wd.View;
                PropertyBorder.Child = wd.PropertyInspectorView;

            }

            void RegisterCustomMetadata()
            {
                var builder = new AttributeTableBuilder();
                builder.AddCustomAttributes(typeof(Parallel), new DesignerAttribute(typeof(CustomParallelDesigner)));
                MetadataStore.AddAttributeTable(builder.CreateTable());
            }
        }
    }
    ```

## <a name="see-also"></a>另请参阅

- <xref:System.Activities.Presentation.ActivityDesigner>
- <xref:System.Activities.Presentation.WorkflowItemPresenter>
- <xref:System.Activities.Presentation.WorkflowItemsPresenter>
- <xref:System.Activities.Presentation.WorkflowViewElement>
- <xref:System.Activities.Presentation.Model.ModelItem>
- [自定义工作流设计体验](customizing-the-workflow-design-experience.md)
