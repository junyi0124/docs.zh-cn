---
title: 使用 ModelItem 编辑上下文
ms.date: 03/30/2017
ms.assetid: 7f9f1ea5-0147-4079-8eca-be94f00d3aa1
ms.openlocfilehash: 2ab002f902833d3b1a69ea0b03b5ca589f4492d1
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96275965"
---
# <a name="using-the-modelitem-editing-context"></a>使用 ModelItem 编辑上下文

<xref:System.Activities.Presentation.Model.ModelItem> 编辑上下文是主机应用程序用来与设计器进行通信的对象。 <xref:System.Activities.Presentation.EditingContext> 公开两个可以使用的方法：<xref:System.Activities.Presentation.EditingContext.Items%2A> 和 <xref:System.Activities.Presentation.EditingContext.Services%2A>  
  
## <a name="the-items-collection"></a>项集合  

 <xref:System.Activities.Presentation.EditingContext.Items%2A> 集合用于访问主机与设计器之间共享的数据或所有设计器都可以使用的数据。 通过 <xref:System.Activities.Presentation.ContextItemManager> 类访问时，此集合具有以下功能：  
  
1. <xref:System.Activities.Presentation.ContextItemManager.GetValue%2A>  
  
2. <xref:System.Activities.Presentation.ContextItemManager.Subscribe%2A>  
  
3. <xref:System.Activities.Presentation.ContextItemManager.Unsubscribe%2A>  
  
4. <xref:System.Activities.Presentation.ContextItemManager.SetValue%2A>  
  
## <a name="the-services-collection"></a>服务集合  

 <xref:System.Activities.Presentation.EditingContext.Services%2A> 集合用于访问设计器用来与主机交互的服务或访问所有设计器都使用的服务。 此集合有以下需要注意的方法：  
  
1. <xref:System.Activities.Presentation.ServiceManager.Publish%2A>  
  
2. <xref:System.Activities.Presentation.ServiceManager.Subscribe%2A>  
  
3. <xref:System.Activities.Presentation.ServiceManager.Unsubscribe%2A>  
  
4. <xref:System.Activities.Presentation.ServiceManager.GetService%2A>  
  
## <a name="assigning-a-designer-an-activity"></a>为活动指定设计器  

 为指定活动要使用的设计器，使用了 Designer 属性。  
  
```csharp  
[Designer(typeof(MyClassDesigner))]  
public sealed class MyClass : CodeActivity  
{
}
```  
  
## <a name="creating-a-service"></a>创建服务  

 若要创建一个服务，充当设计器与主机之间的信息管道，必须创建一个接口和一个实现。 接口由 <xref:System.Activities.Presentation.ServiceManager.Publish%2A> 方法用来定义服务的成员，而实现则包含服务的逻辑。 在下面的代码示例中，创建了一个服务接口和一个实现。  
  
```csharp  
public interface IMyService  
    {  
        IEnumerable<string> GetValues(string DisplayName);  
    }  
  
    public class MyServiceImpl : IMyService  
    {  
        public IEnumerable<string> GetValues(string DisplayName)  
        {  
            return new string[]  {
                DisplayName + " One",
                DisplayName + " Two",  
                "Three " + DisplayName  
            } ;  
        }  
    }  
```  
  
## <a name="publishing-a-service"></a>发布服务  

 为使设计器能够使用服务，必须先由主机使用 <xref:System.Activities.Presentation.ServiceManager.Publish%2A> 方法将服务发布。  
  
```csharp  
this.Context.Services.Publish<IMyService>(new MyServiceImpl);  
```  
  
## <a name="subscribing-to-a-service"></a>订阅服务  

 设计器在 <xref:System.Activities.Presentation.ServiceManager.Subscribe%2A> 方法中使用 <xref:System.Activities.Presentation.WorkflowViewElement.OnModelItemChanged%2A> 方法来获得对服务的访问。 下列代码段演示了订阅服务的方法。  
  
```csharp  
protected override void OnModelItemChanged(object newItem)  
{  
    if (!subscribed)  
    {  
        this.Context.Services.Subscribe<IMyService>(  
            servInstance =>  
            {  
                listBox1.ItemsSource = servInstance.GetValues(this.ModelItem.Properties["DisplayName"].ComputedValue.ToString());  
            }  
            );  
        subscribed = true;
    }  
}  
```  
  
## <a name="sharing-data-using-the-items-collection"></a>使用项集合共享数据  

 使用项集合与使用服务集合类似，但使用的是 <xref:System.Activities.Presentation.ContextItemManager.SetValue%2A>，而不是 Publish。 此集合更适合在设计器与主机之间的共享简单的数据，不适合复杂的功能。  
  
## <a name="editingcontext-host-items-and-services"></a>EditingContext 主机项和服务  

 .NET Framework 提供了许多通过编辑上下文访问的内置项和服务。  
  
 项：  
  
- <xref:System.Activities.Presentation.Hosting.AssemblyContextControlItem>：管理将在控件（如表达式编辑器）的工作流内部使用的引用和本地程序集的列表。  
  
- <xref:System.Activities.Presentation.Hosting.ReadOnlyState>：指示设计器是否处于只读状态。  
  
- <xref:System.Activities.Presentation.View.Selection>：定义当前所选对象的集合。  
  
- <xref:System.Activities.Presentation.Hosting.WorkflowCommandExtensionItem>:  
  
- <xref:System.Activities.Presentation.WorkflowFileItem>：提供有关当前编辑会话所基于的文件的信息。  
  
 服务：  
  
- <xref:System.Activities.Presentation.Model.AttachedPropertiesService>：允许使用 <xref:System.Activities.Presentation.Model.AttachedPropertiesService.AddProperty%2A> 将属性添加到当前实例中。  
  
- <xref:System.Activities.Presentation.View.DesignerView>：允许访问设计器画布的属性。  
  
- <xref:System.Activities.Presentation.IActivityToolboxService>：允许更新工具箱的内容。  
  
- <xref:System.Activities.Presentation.Hosting.ICommandService>：用于将设计器命令（如上下文菜单）与自定义的服务实现集成。  
  
- <xref:System.Activities.Presentation.Debug.IDesignerDebugView>：为设计器调试器提供功能。  
  
- <xref:System.Activities.Presentation.View.IExpressionEditorService>：提供对“表达式编辑器”对话框的访问。  
  
- <xref:System.Activities.Presentation.IIntegratedHelpService>：为设计器提供集成的帮助功能。  
  
- <xref:System.Activities.Presentation.Validation.IValidationErrorService>：使用 <xref:System.Activities.Presentation.Validation.IValidationErrorService.ShowValidationErrors%2A> 提供对验证错误的访问。  
  
- <xref:System.Activities.Presentation.IWorkflowDesignerStorageService>：提供内部服务以存储和检索数据。 此服务由 .NET Framework 在内部使用，不用于外部使用。  
  
- <xref:System.Activities.Presentation.IXamlLoadErrorService>：使用 <xref:System.Activities.Presentation.IXamlLoadErrorService.ShowXamlLoadErrors%2A> 提供对 XAML 加载错误集合的访问。  
  
- <xref:System.Activities.Presentation.Services.ModelService>：由设计器用来与正在编辑的工作流模型进行交互。  
  
- <xref:System.Activities.Presentation.Model.ModelTreeManager>：使用 <xref:System.Activities.Presentation.Model.ModelItem.Root%2A> 提供对模型项树根的访问。  
  
- <xref:System.Activities.Presentation.UndoEngine>：提供撤消和重做功能。  
  
- <xref:System.Activities.Presentation.Services.ViewService>：将可视元素映射到基础模型项。  
  
- <xref:System.Activities.Presentation.View.ViewStateService>：存储模型项的视图状态。  
  
- <xref:System.Activities.Presentation.View.VirtualizedContainerService>：用于自定义虚拟容器 UI 行为。  
  
- <xref:System.Activities.Presentation.Hosting.WindowHelperService>：用于注册和注销事件通知的委托。 还允许设置一个窗口所有者。
