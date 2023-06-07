---
title: 使用 Pick 活动
ms.date: 03/30/2017
ms.assetid: b89be812-a247-4025-b0e3-ffb20db027a6
ms.openlocfilehash: df8570a61c7bfbfacc00b0896156135ecf2a0c32
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96267450"
---
# <a name="using-the-pick-activity"></a>使用 Pick 活动

此示例演示如何使用 <xref:System.Activities.Statements.Pick> 活动。

 <xref:System.Activities.Statements.Pick> 活动提供基于事件的控制建模。 其行为与 C# `switch` 语句相似, 也就是只在 `switch` 语句中执行一个分支。 与 `switch` 语句中基于某个值执行一个分支不同的是，<xref:System.Activities.Statements.Pick> 活动基于一个活动的完成情况来执行一个分支。

 此示例提示用户在给定的时间期限内在控制台中键入自己的用户名。 此示例中的 <xref:System.Activities.Statements.Pick> 活动有两个分支，这两个分支的执行将取决于用户是否在五秒钟之内键入了其用户名。 如果用户在五秒钟之内键入了其用户名，则将执行第一个包含一个自定义 `ReadLine` 活动的分支；否则将执行另外一个包含一个 <xref:System.Activities.Statements.Delay> 活动的分支。 当用户在控制台中键入其用户名之后，此用户名将会在控制台中输出。 如果在五秒之内未完成输入，则操作超时。

## <a name="demonstrates"></a>演示

 <xref:System.Activities.Statements.Pick> 活动。

## <a name="discussion"></a>讨论 (Discussion)

 此示例包含一个设计器工作流和一个编码工作流。

 设计器工作流示例的设计器版本演示如何在设计器中创建工作流。 包含以下文件：

- Program.cs：包含执行示例工作流的 `Main` 函数。

- ReadString.cs：一个从控制台读取一些输入的自定义活动。

- Sequence1.xaml：一个通过使用 Pick 的设计器创建的工作流。

 编码的工作流示例的编码版本演示如何在设计器中创建工作流。 包含以下文件：

- Program.cs：包含执行示例工作流的 `Main` 函数。

- ReadString.cs：一个从控制台读取一些输入的自定义活动。

#### <a name="to-use-this-sample"></a>使用此示例

1. 使用 Visual Studio 2010，打开 "选择 .sln" 解决方案文件。

2. 要生成解决方案，按 Ctrl+Shift+B。

3. 若要运行解决方案，请按 F5。

> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> 如果此目录不存在，请参阅[Windows Communication Foundation (wcf) ，并 Windows Workflow Foundation (的 WF](https://www.microsoft.com/download/details.aspx?id=21459)) .NET Framework Windows Communication Foundation ([!INCLUDE[wf1](../../../../includes/wf1-md.md)] 此示例位于以下目录：  
>
> `<InstallDrive>:\WF_WCF_Samples\WF\Basic\Built-InActivities\Pick`
