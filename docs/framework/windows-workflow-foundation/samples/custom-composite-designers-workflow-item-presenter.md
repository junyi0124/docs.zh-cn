---
title: 自定义复合设计器 — 工作流项演示器
ms.date: 03/30/2017
ms.assetid: f85224cf-9e30-44a5-9a81-3bc438a34364
ms.openlocfilehash: d1047b8be35545e83eaa8788b53751b6b0056984
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/25/2019
ms.locfileid: "75338045"
---
# <a name="custom-composite-designers---workflow-item-presenter"></a>自定义复合设计器 — 工作流项演示器

<xref:System.Activities.Presentation.WorkflowItemPresenter> 是 WF 设计器编程模型中的一种密钥类型，它允许创建可放置任意活动的 "放置区"。 此示例演示如何生成一个表示 "放置区域" 的活动设计器。

此示例演示：

- 使用 <xref:System.Activities.Presentation.WorkflowItemPresenter> 创建自定义活动设计器。

- 使用元数据存储区注册自定义设计器。

- 以声明方式和命令方式编程重新承载的工具箱。

## <a name="sample-details"></a>示例详细信息

此示例的代码演示：

- 针对 `SimpleNativeActivity` 类生成自定义活动设计器。

- 使用 <xref:System.Activities.Presentation.WorkflowItemPresenter> 创建自定义活动设计器。

```xaml
<sap:ActivityDesigner x:Class="Microsoft.Samples.UsingWorkflowItemPresenter.SimpleNativeDesigner"
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

 请注意，应使用 WPF 数据绑定来绑定到 `ModelItem.Body`。 `ModelItem` 是 <xref:System.Activities.Presentation.ActivityDesigner> 上的属性，该属性引用设计器所用于的基础对象，在本例中为**命名为 simplenativeactivity**。

## <a name="set-up-build-and-run-the-sample"></a>设置、生成和运行示例

1. 在 Visual Studio 中打开解决方案。

2. 按**F5**编译并运行该应用程序。

> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> 如果此目录不存在，请参阅[.NET Framework 4 的 Windows Communication Foundation （wcf）和 Windows Workflow Foundation （WF）示例](https://www.microsoft.com/download/details.aspx?id=21459)以下载所有 WINDOWS COMMUNICATION FOUNDATION （wcf）和 [!INCLUDE[wf1](../../../../includes/wf1-md.md)] 示例。 此示例位于以下目录：
>
> `<InstallDrive>:\WF_WCF_Samples\WF\Basic\CustomActivities\CustomActivityDesigners\WorkflowItemPresenter`

## <a name="see-also"></a>另请参阅

- <xref:System.Activities.Presentation.WorkflowItemPresenter>
- [使用工作流设计器开发应用程序](/visualstudio/workflow-designer/developing-applications-with-the-workflow-designer)
