---
title: WorkflowServiceHost 中的并行版本控制
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 60887eed-df40-4412-b812-41e1dd329d15
ms.openlocfilehash: 878e610bd1fe0b7e2496f251333a3ad21909788a
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96245089"
---
# <a name="side-by-side-versioning-in-workflowservicehost"></a>WorkflowServiceHost 中的并行版本控制

<xref:System.ServiceModel.Activities.WorkflowServiceHost>.NET Framework 4.5 中引入的并行版本控制提供了在单个终结点上承载工作流服务的多个版本的功能。 所提供的并行功能允许配置工作流服务，以便使用新的工作流定义来创建工作流服务的新实例，而对于正在运行的实例则使用现有的定义来完成。 本主题概述了使用 <xref:System.ServiceModel.Activities.WorkflowServiceHost> 的工作流服务并行执行。  
  
> [!NOTE]
> 若要下载示例并观看工作流服务并行版本控制的视频演练，请参阅 [使用 Web-Hosted .Xamlx 工作流服务的并行版本控制](https://go.microsoft.com/fwlink/?LinkId=393746)。  
  
## <a name="hosting-multiple-versions-in-a-workflow-service"></a>在工作流服务中承载多个版本  

 <xref:System.ServiceModel.Activities.WorkflowServiceHost> 包含两个属性，它们可配置为允许工作流的多个版本并行执行：<xref:System.ServiceModel.Activities.WorkflowServiceHost.SupportedVersions%2A> 和 <xref:System.ServiceModel.Activities.WorkflowService.DefinitionIdentity%2A>。 <xref:System.ServiceModel.Activities.WorkflowServiceHost.SupportedVersions%2A> 包含所支持的工作流服务版本，而 <xref:System.ServiceModel.Activities.WorkflowService.DefinitionIdentity%2A> 用于唯一地标识每个工作流服务。 这一点通过将 <xref:System.Activities.WorkflowIdentity> 与工作流服务关联来实现。 <xref:System.Activities.WorkflowIdentity> 包含三条标识信息。 <xref:System.Activities.WorkflowIdentity.Name%2A> 和 <xref:System.Activities.WorkflowIdentity.Version%2A> 包含一个名称和一个 <xref:System.Version>，并且是必需的；而 <xref:System.Activities.WorkflowIdentity.Package%2A> 则是可选的，可用于指定包含如程序集名称或其他所需信息的附加字符串。 <xref:System.ServiceModel.Activities.WorkflowServiceHost.SupportedVersions%2A> 集合中包含的每个工作流服务都必须具有唯一的 <xref:System.Activities.WorkflowIdentity>。 如果 <xref:System.Activities.WorkflowIdentity> 的三个属性中有任一属性不同于其他 <xref:System.Activities.WorkflowIdentity>，则它是唯一的。 `null` <xref:System.Activities.WorkflowIdentity> 是的允许值 <xref:System.ServiceModel.Activities.WorkflowService.DefinitionIdentity%2A> ，但只有一个以前版本的工作流服务可能具有 `null` <xref:System.Activities.WorkflowIdentity> 。  
  
> [!IMPORTANT]
> <xref:System.Activities.WorkflowIdentity> 不应包含任何个人身份信息 (PII)。 <xref:System.Activities.WorkflowIdentity> 由三部分组成：<xref:System.Activities.WorkflowIdentity.Name%2A> (<xref:System.String>)、<xref:System.Activities.WorkflowIdentity.Version%2A> (<xref:System.Version>) 和 <xref:System.Activities.WorkflowIdentity.Package%2A> (<xref:System.String>)。 在几个不同活动生命周期点，运行时将向任何已配置的跟踪服务发出有关用于创建实例的 <xref:System.Activities.WorkflowIdentity> 的信息。 WF 跟踪没有任何隐藏 PII（敏感用户信息）的机制。 因此，<xref:System.Activities.WorkflowIdentity> 实例不应该包含任何 PII 数据，因为运行时将在跟踪记录中发出这些数据，任何有权查看跟踪记录的人都能够看到这些数据。  
  
### <a name="rules-for-hosting-multiple-versions-of-a-workflow-service"></a>承载工作流服务的多个版本的规则  

 当用户向 <xref:System.ServiceModel.Activities.WorkflowServiceHost> 添加其他版本时，必须满足几个条件，以便以相同的一组终结点和说明来承载工作流服务。 如果任何其他版本未能满足这些条件，则 <xref:System.ServiceModel.Activities.WorkflowServiceHost> 在调用 `Open` 时会引发异常。 作为附加版本向主机提供的每个工作流定义必须满足以下要求（其中，主版本是提供给主机构造函数的工作流服务定义）。 附加的工作流版本必须：  
  
- 具有与工作流服务的主版本相同的 <xref:System.ServiceModel.Activities.WorkflowService.Name%2A>。  
  
- 在其 <xref:System.ServiceModel.Activities.Receive> 中不得有主版本中不存在的任何 <xref:System.ServiceModel.Activities.SendReply> 或 <xref:System.ServiceModel.Activities.WorkflowService.Body%2A> 活动，并且这些活动必须与操作协定相匹配。  
  
- 具有唯一的 <xref:System.ServiceModel.Activities.WorkflowService.DefinitionIdentity%2A>。 有且仅有一个工作流定义可以具有 `null`<xref:System.ServiceModel.Activities.WorkflowService.DefinitionIdentity%2A>。  
  
 允许进行一些更改。 以下各项在各版本间可能有所不同：  
  
- <xref:System.ServiceModel.Activities.WorkflowService.DefinitionIdentity%2A> 可能有与主版本不同的名称和包。  
  
- <xref:System.ServiceModel.Activities.WorkflowService.AllowBufferedReceive%2A> 值可能不同于主版本。  
  
- <xref:System.ServiceModel.Activities.WorkflowService.ConfigurationName%2A> 可能不同于主版本。  
  
- <xref:System.ServiceModel.Activities.WorkflowService.ImplementedContracts%2A> 可能不同于主版本。  
  
### <a name="configuring-the-definitionidentity"></a>配置 DefinitionIdentity  

 使用工作流设计器创建工作流服务时，将 <xref:System.ServiceModel.Activities.WorkflowService.DefinitionIdentity%2A> 使用 " **属性** " 窗口设置。 在设计器中单击服务的根活动外部，选择工作流服务，然后从 "**视图**" 菜单中选择 "**属性窗口**"。 从出现在 " **DefinitionIdentity** " 属性旁边的下拉列表中选择 " **WorkflowIdentity** "，然后展开并指定所需 <xref:System.Activities.WorkflowIdentity> 属性。 在下面的示例中， <xref:System.ServiceModel.Activities.WorkflowService.DefinitionIdentity%2A> 使用 <xref:System.Activities.WorkflowIdentity.Name%2A> `MortgageWorkflow` 和 <xref:System.Activities.WorkflowIdentity.Version%2A> 的配置 `1.0.0.0` 。 <xref:System.Activities.WorkflowIdentity.Package%2A> 是可选的，在此示例中是 `null`。  
  
 ![显示 DefinitionIdentity 属性的屏幕截图。](./media/side-by-side-versioning-in-workflowservicehost/definitionidentity-property.bmp)  
  
 如果工作流服务是自承载的，则 <xref:System.ServiceModel.Activities.WorkflowService.DefinitionIdentity%2A> 在构造工作流服务时配置。 在下面的示例中，使用与 <xref:System.ServiceModel.Activities.WorkflowService.DefinitionIdentity%2A> 上一示例相同的值来配置，并使用 <xref:System.Activities.WorkflowIdentity.Name%2A> `MortgageWorkflow` 和 <xref:System.Activities.WorkflowIdentity.Name%2A> `1.0.0.0` 。  
  
```csharp  
WorkflowService service = new WorkflowService  
{  
    Name = "MortgageWorkflowService",  
    Body = new MortgageWorkflow(),  
    DefinitionIdentity = new WorkflowIdentity  
    {  
        Name = "MortgageWorkflow",  
        Version = new Version(1, 0, 0, 0)  
    }  
};  
```  
  
```vb  
Dim service As New WorkflowService  
With service  
    .Name = "MortgageWorkflowService"  
    .Body = New MortgageWorkflow  
    .DefinitionIdentity = New WorkflowIdentity With _  
    { _  
        .Name = "MortgageWorkflow", _  
        .Version = New Version(1, 0, 0, 0) _  
    }  
End With  
```  
  
 <xref:System.ServiceModel.Activities.WorkflowService.DefinitionIdentity%2A>不是必需的，但只有一个版本的工作流服务可以为 **null** <xref:System.ServiceModel.Activities.WorkflowService.DefinitionIdentity%2A> 。  
  
> [!NOTE]
> 如果最初部署服务时未配置 <xref:System.ServiceModel.Activities.WorkflowService.DefinitionIdentity%2A>，则这一点非常有用，然后会创建一个更新的版本。  
  
### <a name="adding-a-new-version-to-a-web-hosted-workflow-service"></a>将新版本添加到 Web 承载的工作流服务  

 在 Web 承载的服务中配置新版本工作流服务的第一步是：在与服务文件同名的 `App_Code` 文件夹中创建一个新的文件夹。 如果服务的 `xamlx` 文件命名为 `MortgageWorkflow.xamlx`，则必须将该文件夹命名为 `MortgageWorkflow`。 将原始服务的 `xamlx` 文件的副本放到此文件夹中，并将其重命名为新名称，如 `MortgageWorkflowV1.xamlx`。 对主服务进行所需的更改，更新其 <xref:System.ServiceModel.Activities.WorkflowService.DefinitionIdentity%2A>，然后部署服务。 在下面的示例中，使用 <xref:System.ServiceModel.Activities.WorkflowService.DefinitionIdentity%2A><xref:System.Activities.WorkflowIdentity.Name%2A> 和 `MortgageWorkflow`<xref:System.Activities.WorkflowIdentity.Version%2A> 更新了 `2.0.0.0`。  
  
 ![显示 WorkflowIdentity 的 DefinitionIdentity 的屏幕截图。](./media/side-by-side-versioning-in-workflowservicehost/definitionidentity-workflowidentity.bmp)  
  
 当该服务重新启动时，以前的版本会自动添加到 <xref:System.ServiceModel.Activities.WorkflowServiceHost.SupportedVersions%2A> 集合，因为它位于指定的 `App_Code` 子文件夹中。 请注意，如果工作流服务的主版本具有 `null` <xref:System.ServiceModel.Activities.WorkflowService.DefinitionIdentity%2A> 以前的版本，则将不会添加以前的版本。 一个版本可能有 `null`<xref:System.ServiceModel.Activities.WorkflowService.DefinitionIdentity%2A>，但如果有多个版本，则主版本不得是具有 `null`<xref:System.ServiceModel.Activities.WorkflowService.DefinitionIdentity%2A> 的版本，否则以前的版本不会添加到 <xref:System.ServiceModel.Activities.WorkflowServiceHost.SupportedVersions%2A> 集合。  
  
### <a name="adding-a-new-version-to-a-self-hosted-workflow-service"></a>将新版本添加到自承载的工作流服务  

 将新版本添加到自承载的工作流服务时，使用主版本的工作流服务对 <xref:System.ServiceModel.Activities.WorkflowServiceHost> 进行配置，以前的版本必须显式添加到 <xref:System.ServiceModel.Activities.WorkflowServiceHost.SupportedVersions%2A> 集合。 在以下示例中，使用主工作流服务对 <xref:System.ServiceModel.Activities.WorkflowServiceHost> 进行配置（工作流服务使用 `MortgageWorkflowV2` 工作流定义），并且将使用 `MortgageWorkflowV1` 工作流定义配置的工作流服务添加到 <xref:System.ServiceModel.Activities.WorkflowServiceHost.SupportedVersions%2A> 集合。 每个工作流服务均配置一个反映工作流定义版本的唯一 <xref:System.ServiceModel.Activities.WorkflowService.DefinitionIdentity%2A>。  
  
```csharp  
// Create the primary version of the workflow service.  
WorkflowService serviceV2 = new WorkflowService  
{  
    Name = "MortgageWorkflowService",  
    Body = new MortgageWorkflowV2(),  
    DefinitionIdentity = new WorkflowIdentity  
    {  
        Name = "MortgageWorkflow",  
        Version = new Version(2, 0, 0, 0)  
    }  
};  
  
// Configure the WorkflowServiceHost with the current version  
// of the workflow service. This code requires Administrator  
// privileges to function correctly. If running from Visual  
// Studio, Visual Studio must be run with Administrator privileges.  
WorkflowServiceHost host = new WorkflowServiceHost(serviceV2,
    new Uri("http://localhost:8080/MortgageWorkflowService"));  
  
// Create the previous version of the workflow service.  
WorkflowService serviceV1 = new WorkflowService  
{  
    Name = "MortgageWorkflowService",  
    Body = new MortgageWorkflowV1(),  
    DefinitionIdentity = new WorkflowIdentity  
    {  
        Name = "MortgageWorkflow",  
        Version = new Version(1, 0, 0, 0)  
    }  
};  
  
// Add the previous version of the service to the SupportedVersions collection.  
host.SupportedVersions.Add(serviceV1);  
```  
  
```vb  
'Create the primary version of the workflow service  
Dim serviceV2 As New WorkflowService  
With serviceV2  
    .Name = "MortgageWorkflowService"  
    .Body = New MortgageWorkflowV2  
    .DefinitionIdentity = New WorkflowIdentity With _  
    { _  
        .Name = "MortgageWorkflow", _  
        .Version = New Version(2, 0, 0, 0) _  
    }  
End With  
  
'Configure the WorkflowServiceHost with the current version  
'of the workflow service. This code requires Administrator  
'privileges to function correctly. If running from Visual  
'Studio, Visual Studio must be run with Administrator privileges.  
  
Dim host As New WorkflowServiceHost(serviceV2, _  
    New Uri("http://localhost:8080/MortgageWorkflowService"))  
  
'Create the previous version of the workflow service.  
Dim serviceV1 As New WorkflowService  
With serviceV1  
    .Name = "MortgageWorkflowService"  
    .Body = New MortgageWorkflowV1  
    .DefinitionIdentity = New WorkflowIdentity With _  
    { _  
        .Name = "MortgageWorkflow", _  
        .Version = New Version(1, 0, 0, 0) _  
    }  
End With  
  
'Add the previous version of the service to the SupportedVersions collection.  
host.SupportedVersions.Add(serviceV1)  
```
