---
title: 自定义复合设计器 — 工作流项演示器
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 70055c4b-1173-47a3-be80-b5bce6f59e9a
ms.openlocfilehash: 081dce85946fab85cff474508c46770c762b9e76
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/25/2019
ms.locfileid: "75338728"
---
# <a name="custom-composite-designers---workflow-items-presenter"></a>自定义复合设计器 — 工作流项演示器

<xref:System.Activities.Presentation.WorkflowItemsPresenter?displayProperty=nameWithType> 是 WF 设计器编程模型中的一个重要类型，可用于编辑包含元素的集合。 此示例演示如何生成一个呈现此类可编辑集合的活动设计器。

此示例演示：

- 使用 <xref:System.Activities.Presentation.WorkflowItemsPresenter?displayProperty=nameWithType> 创建自定义活动设计器。

- 使用 "折叠" 视图和 "已扩展" 视图创建活动设计器。

- 在重新承载的应用程序中重写默认设计器。

## <a name="set-up-build-and-run-the-sample"></a>设置、生成和运行示例

1. 在 Visual Studio 2010 中，为 Visual Basic C#打开或的 usingworkflowitemspresenter.sln 示例解决方案。

2. 生成和运行解决方案。

   此时将打开一个重新承载工作流设计器应用程序，你可以将活动拖动到画布上。

## <a name="sample-highlights"></a>示例重点

此示例的代码演示了以下内容：

- 构建的设计器所针对的活动：`Parallel`

- 使用 <xref:System.Activities.Presentation.WorkflowItemsPresenter?displayProperty=nameWithType> 创建自定义活动设计器。 需要指出的一些事项：

  - 请注意，应使用 WPF 数据绑定来绑定到 `ModelItem.Branches`。 `ModelItem` 是 `WorkflowElementDesigner` 的属性，它引用设计器所用于的基础对象，在此例中为 `Parallel`。

  - <xref:System.Activities.Presentation.WorkflowItemsPresenter.SpacerTemplate?displayProperty=nameWithType> 可用于在集合中的各个项之间显示一个可视对象。

  - <xref:System.Activities.Presentation.WorkflowItemsPresenter.ItemsPanel?displayProperty=nameWithType> 是一个模板，可用于确定集合中的项的布局。 在此例中，使用水平堆叠面板。

  下面的示例代码演示了此过程。

  ```xaml
  <sad:WorkflowItemsPresenter HintText="Drop Activities Here"
                                Items="{Binding Path=ModelItem.Branches}">
      <sad:WorkflowItemsPresenter.SpacerTemplate>
        <DataTemplate>
          <Ellipse Width="10" Height="10" Fill="Black"/>
        </DataTemplate>
      </sad:WorkflowItemsPresenter.SpacerTemplate>
      <sad:WorkflowItemsPresenter.ItemsPanel>
        <ItemsPanelTemplate>
          <StackPanel Orientation="Horizontal"/>
        </ItemsPanelTemplate>
      </sad:WorkflowItemsPresenter.ItemsPanel>
    </sad:WorkflowItemsPresenter>
  ```

- 执行 `DesignerAttribute` 与 `Parallel` 类型的关联，然后输出报告的特性。

  - 首先，注册所有默认的设计器。

    以下是代码示例。

    ```csharp
    // register metadata
    (new DesignerMetadata()).Register();
    RegisterCustomMetadata();
    ```

    ```vb
    ' register metadata
    Dim metadata = New DesignerMetadata()
    metadata.Register()
    ' register custom metadata
    RegisterCustomMetadata()
    ```

  - 然后，在 `RegisterCustomMetadata` 方法中重写此并行处理。

    下面的代码分别使用 C# 和 Visual Basic 演示了此过程。

    ```csharp
    void RegisterCustomMetadata()
    {
          AttributeTableBuilder builder = new AttributeTableBuilder();
          builder.AddCustomAttributes(typeof(Parallel), new DesignerAttribute(typeof(CustomParallelDesigner)));
          MetadataStore.AddAttributeTable(builder.CreateTable());
    }
    ```

    ```vb
    Sub RegisterCustomMetadata()
       Dim builder As New AttributeTableBuilder()
       builder.AddCustomAttributes(GetType(Parallel), New DesignerAttribute(GetType(CustomParallelDesigner)))
       MetadataStore.AddAttributeTable(builder.CreateTable())
    End Sub
    ```

- 最后，请注意使用不同的数据模板和触发器来基于 `IsRootDesigner` 属性选择适当的模板。

  以下是代码示例。

  ```xaml
  <sad:ActivityDesigner x:Class="Microsoft.Samples.CustomParallelDesigner"
      xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
      xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
      xmlns:sad="clr-namespace:System.Activities.Design;assembly=System.Activities.Design"
      xmlns:sadv="clr-namespace:System.Activities.Design.View;assembly=System.Activities.Design">
    <sad:ActivityDesigner.Resources>
      <DataTemplate x:Key="Expanded">
        <StackPanel>
          <TextBlock>This is the Expanded View</TextBlock>
          <sad:WorkflowItemsPresenter HintText="Drop Activities Here"
                                      Items="{Binding Path=ModelItem.Branches}">
            <sad:WorkflowItemsPresenter.SpacerTemplate>
              <DataTemplate>
                <Ellipse Width="10" Height="10" Fill="Black"/>
              </DataTemplate>
            </sad:WorkflowItemsPresenter.SpacerTemplate>
            <sad:WorkflowItemsPresenter.ItemsPanel>
              <ItemsPanelTemplate>
                <StackPanel Orientation="Horizontal"/>
              </ItemsPanelTemplate>
            </sad:WorkflowItemsPresenter.ItemsPanel>
          </sad:WorkflowItemsPresenter>
        </StackPanel>
      </DataTemplate>
      <DataTemplate x:Key="Collapsed">
        <TextBlock>This is the Collapsed View</TextBlock>
      </DataTemplate>
      <Style x:Key="ExpandOrCollapsedStyle" TargetType="{x:Type ContentPresenter}">
        <Setter Property="ContentTemplate" Value="{DynamicResource Collapsed}"/>
        <Style.Triggers>
          <DataTrigger Binding="{Binding Path=IsRootDesigner}" Value="true">
            <Setter Property="ContentTemplate" Value="{DynamicResource Expanded}"/>
          </DataTrigger>
        </Style.Triggers>
      </Style>
    </sad: ActivityDesigner.Resources>
    <Grid>
      <ContentPresenter Style="{DynamicResource ExpandOrCollapsedStyle}" Content="{Binding}"/>
    </Grid>
  </sad: ActivityDesigner>
  ```

> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> 如果此目录不存在，请参阅[.NET Framework 4 的 Windows Communication Foundation （wcf）和 Windows Workflow Foundation （WF）示例](https://www.microsoft.com/download/details.aspx?id=21459)以下载所有 WINDOWS COMMUNICATION FOUNDATION （wcf）和 [!INCLUDE[wf1](../../../../includes/wf1-md.md)] 示例。 此示例位于以下目录：
>
> `<InstallDrive>:\WF_WCF_Samples\WF\Basic\CustomActivities\CustomActivityDesigners\WorkflowItemsPresenter`

## <a name="see-also"></a>另请参阅

- <xref:System.Activities.Presentation.WorkflowItemsPresenter>
- [使用工作流设计器开发应用程序](/visualstudio/workflow-designer/developing-applications-with-the-workflow-designer)
