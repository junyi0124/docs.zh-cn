---
title: 在自定义设计器中使用 ExpressionTextBox
ms.date: 03/30/2017
ms.assetid: f82e73e7-a256-4a4d-82b7-c0d62f4ab5e7
ms.openlocfilehash: dfb53096be59abd9924a024880d4125ca774d4b8
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96267475"
---
# <a name="using-the-expressiontextbox-in-a-custom-activity-designer"></a>在自定义设计器中使用 ExpressionTextBox

此示例演示如何在自定义活动设计器中使用 <xref:System.Activities.Presentation.View.ExpressionTextBox>。 自定义活动 `MultiAssign` 将两个字符串值分配给两个字符串变量。 某些 <xref:System.Activities.Presentation.View.ExpressionTextBox> 控件绑定到 <xref:System.Activities.InArgument>，而某些控件绑定到 <xref:System.Activities.OutArgument>。

## <a name="sample-details"></a>示例详细信息

 `ArgumentToExpressionConverter` 是在将表达式绑定到参数时使用的类型转换器。 根据需要，必须将 `ConverterParameter` 设置为 `In` 或 `Out`。 不支持 `InOut`。

 `UseLocationExpression`在 s 中使用属性 `OutArgument` 来指定表达式应为左值 ( "左值" 或 "位置值" ) 表达式。 在大多数情况下，左值表达式是有效的 Visual Basic 标识符，用于指示要返回的 `OutArgument` 是变量还是参数名称。

 在此示例中，将 `MaxLines` 特性设置为 1，未设置 `MinLines`。 这表示无论用户键入多少文本，<xref:System.Activities.Presentation.View.ExpressionTextBox> 都是大小固定的一行。 若要允许 <xref:System.Activities.Presentation.View.ExpressionTextBox> 增大以容纳用户输入，请将 `MaxLines` 设置为大于 `MinLines`。

 ExpressionTextBox 只能绑定到参数，不能绑定到 CLR 属性。

#### <a name="to-use-this-sample"></a>使用此示例

1. 使用 Visual Studio 2010 打开 ExpressionTextBoxSample 文件。

2. 要生成解决方案，按 Ctrl+Shift+B。

#### <a name="to-run-this-sample"></a>运行本示例的步骤

1. 向解决方案添加新工作流控制台应用程序。

2. 从新的工作流控制台应用程序项目添加对 **ExpressionTextBoxSample** 项目的引用。

3. 生成解决方案。

4. 将 " **MultiAssign** " 活动从 "工具箱" 拖放到工作流中。

> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> 如果此目录不存在，请参阅[Windows Communication Foundation (wcf) ，并 Windows Workflow Foundation (的 WF](https://www.microsoft.com/download/details.aspx?id=21459)) .NET Framework Windows Communication Foundation ([!INCLUDE[wf1](../../../../includes/wf1-md.md)] 此示例位于以下目录：  
>
> `<InstallDrive>:\WF_WCF_Samples\WF\Basic\CustomActivities\CustomActivityDesigners\ExpressionTextBox`  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Activities.Presentation.View.ExpressionTextBox>
- [使用工作流设计器开发应用程序](/visualstudio/workflow-designer/developing-applications-with-the-workflow-designer)
