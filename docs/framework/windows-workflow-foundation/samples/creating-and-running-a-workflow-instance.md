---
title: 创建和运行工作流实例
ms.date: 03/30/2017
ms.assetid: 19d27f47-0491-4569-8f53-51bc1d940e80
ms.openlocfilehash: d895cfa9e924ecf4d1571cf67f1c1e7069e25da5
ms.sourcegitcommit: 5fb5b6520b06d7f5e6131ec2ad854da302a28f2e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/03/2019
ms.locfileid: "74715202"
---
# <a name="creating-and-running-a-workflow-instance"></a>创建和运行工作流实例

此示例演示如何运行工作流实例。 它演示如何以同步方式和异步方式执行这一操作。

## <a name="demonstrates"></a>演示文本

<xref:System.Activities.WorkflowInvoker>, <xref:System.Activities.WorkflowApplication>.

## <a name="discussion"></a>讨论

此示例的第一部分使用 <xref:System.Activities.WorkflowInvoker.Invoke%2A>。 这是执行工作流的最基本方法。 使用 <xref:System.Activities.WorkflowInvoker.Invoke%2A> 执行的工作流以同步方式执行。

该示例的第二部分使用 <xref:System.Activities.WorkflowApplication> 类。 利用 <xref:System.Activities.WorkflowApplication>，您可以对每个实例进行更多控制，包括与正在运行的工作流进行交互的能力和异步运行工作流的能力。

> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> 如果此目录不存在，请参阅[.NET Framework 4 的 Windows Communication Foundation （wcf）和 Windows Workflow Foundation （WF）示例](https://www.microsoft.com/download/details.aspx?id=21459)以下载所有 WINDOWS COMMUNICATION FOUNDATION （wcf）和 [!INCLUDE[wf1](../../../../includes/wf1-md.md)] 示例。 此示例位于以下目录：
>
> `<InstallDrive>:\WF_WCF_Samples\WF\Basic\Execution\CreatingWorkflowInstances`

## <a name="see-also"></a>另请参阅

- [使用 WorkflowInvoker 和 WorkflowApplication](../using-workflowinvoker-and-workflowapplication.md)
