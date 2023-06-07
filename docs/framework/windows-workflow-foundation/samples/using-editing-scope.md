---
title: 使用编辑范围
ms.date: 03/30/2017
ms.assetid: 79306f9e-318b-4687-9863-8b93d1841716
ms.openlocfilehash: 738ed7a21d28963e9e37c24f2e14887d00b9533b
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96267632"
---
# <a name="using-editing-scope"></a>使用编辑范围

此示例演示如何对一组更改进行批处理，以便可以在一个原子单元中撤消它们。 默认情况下，活动设计器作者采取的操作会自动集成到撤消/重复系统中。  
  
## <a name="demonstrates"></a>演示  

 编辑范围域和撤消/重做。  
  
## <a name="discussion"></a>讨论 (Discussion)  

 此示例演示如何在一个工作单元中对针对 <xref:System.Activities.Presentation.Model.ModelItem> 树的一组更改进行批处理。 请注意，直接从 WPF 设计器绑定到 <xref:System.Activities.Presentation.Model.ModelItem> 值时，将会自动应用更改。 此示例演示通过命令性代码进行要批处理的多个更改（而不是单个更改）时必须执行的操作。  
  
 在此示例中，添加了三个活动。 当编辑开始时，对 <xref:System.Activities.Presentation.Model.ModelItem.BeginEdit%2A> 的实例调用 <xref:System.Activities.Presentation.Model.ModelItem>。  将对在此编辑范围中对 <xref:System.Activities.Presentation.Model.ModelItem> 树所做的更改进行批处理。 <xref:System.Activities.Presentation.Model.ModelItem.BeginEdit%2A> 命令返回一个可用于控制此实例的 <xref:System.Activities.Presentation.Model.EditingScope>。 可以调用 <xref:System.Activities.Presentation.Model.EditingScope.OnComplete%2A> 提交编辑范围或调用 <xref:System.Activities.Presentation.Model.EditingScope.OnRevert%2A> 还原编辑范围。  
  
 还可以嵌套 <xref:System.Activities.Presentation.Model.EditingScope> 对象，这样便可以将多个更改集作为一个更大的编辑范围的一部分进行跟踪，并单独控制。 如果来自多个对话框的更改必须单独提交或还原，并且需要将所有更改视为单个原子操作，在这种场合下则可以使用此功能。 在此示例中，使用 <xref:System.Collections.ObjectModel.ObservableCollection%601> 类型的 <xref:System.Activities.Presentation.Model.ModelEditingScope> 来对编辑范围进行堆栈处理。 使用 <xref:System.Collections.ObjectModel.ObservableCollection%601> 的目的是为了能够在设计器图面上观察到嵌套的深度。  
  
## <a name="to-set-up-build-and-run-the-sample"></a>设置、生成和运行示例  
  
1. 生成和运行示例，然后使用左边的按钮修改工作流。  
  
2. 单击 " **打开编辑范围**"。  
  
    1. 此命令将调用 <xref:System.Activities.Presentation.Model.ModelItem.BeginEdit%2A>；后者会创建一个编辑范围，并将其推入到编辑堆栈中。  
  
    2. 然后将三个活动添加到选定的 <xref:System.Activities.Presentation.Model.ModelItem> 中。 请注意，如果尚未使用 <xref:System.Activities.Presentation.Model.ModelItem.BeginEdit%2A> 打开编辑范围，则这三个新活动将会显示在设计器画布上。 因为此操作仍然在 <xref:System.Activities.Presentation.Model.EditingScope> 中挂起，所以不会更新设计器。  
  
3. 按 " **关闭编辑范围** " 以提交编辑范围。 三个活动出现在设计器中。  
  
> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> 如果此目录不存在，请参阅[Windows Communication Foundation (wcf) ，并 Windows Workflow Foundation (的 WF](https://www.microsoft.com/download/details.aspx?id=21459)) .NET Framework Windows Communication Foundation ([!INCLUDE[wf1](../../../../includes/wf1-md.md)] 此示例位于以下目录：  
>
> `<InstallDrive>:\WF_WCF_Samples\WF\Basic\CustomActivities\CustomActivityDesigners\UsingEditingScope`
