---
title: 工作流服务主机可扩展性
ms.date: 03/30/2017
ms.assetid: c0e8f7bb-cb13-49ec-852f-b85d7c23972f
ms.openlocfilehash: 67a7830c564cabafb9441e86eb0d87164d854ee5
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96258447"
---
# <a name="workflow-service-host-extensibility"></a>工作流服务主机可扩展性

[!INCLUDE[netfx_current_long](../../../../includes/netfx-current-long-md.md)]提供用于承载工作流服务的 <xref:System.ServiceModel.Activities.WorkflowServiceHost> 类。 当在托管应用程序或 Windows 服务中自承载工作流服务时，将使用该类。 同样，当使用 Internet 信息服务 (IIS) 或 Windows 进程激活服务 (WAS) 承载工作流服务时，也将使用该类。 通过 <xref:System.ServiceModel.Activities.WorkflowServiceHost> 类提供的扩展点，您可以添加自定义扩展、更改空闲行为以及承载非服务工作流（即不使用消息传递活动的工作流）。  
  
## <a name="workflow-service-host-extensions"></a>工作流服务主机扩展  

 <xref:System.ServiceModel.Activities.WorkflowServiceHost> 包含一个 <xref:System.ServiceModel.Activities.WorkflowServiceHost.WorkflowExtensions%2A> 类型的 <xref:System.Activities.Hosting.WorkflowInstanceExtensionManager> 属性，该属性提供向 <xref:System.ServiceModel.Activities.WorkflowServiceHost> 添加扩展的方法。 使用 <xref:System.Activities.Hosting.WorkflowInstanceExtensionManager.Add%2A> 方法可以为各工作流服务实例添加扩展。 从永久性存储中创建或加载工作流服务实例时，会调用指定的委托以创建新扩展。 使用 <xref:System.Activities.Hosting.WorkflowInstanceExtensionManager.Add%2A> 方法可以为各工作流服务主机添加扩展，所有工作流服务实例共享一个扩展实例。  
  
## <a name="react-to-unhandled-exceptions"></a>响应未经处理的异常  

 使用  <xref:System.ServiceModel.Activities.Description.WorkflowUnhandledExceptionBehavior> 可以指定工作流服务中出现未经处理的异常时所采取的操作。 <xref:System.ServiceModel.Activities.Description.WorkflowUnhandledExceptionBehavior.Action%2A> 属性指定下列 <xref:System.ServiceModel.Activities.Description.WorkflowUnhandledExceptionAction> 值之一：  
  
- <xref:System.ServiceModel.Activities.Description.WorkflowUnhandledExceptionAction.Abandon> - 中止工作流服务实例。  
  
- <xref:System.ServiceModel.Activities.Description.WorkflowUnhandledExceptionAction.AbandonAndSuspend> - 回滚到最后保存的状态，并挂起工作流服务实例。 仅当已至少保存一次工作流时，才会发生此状况。 否则，将中止工作流实例。  
  
- <xref:System.ServiceModel.Activities.Description.WorkflowUnhandledExceptionAction.Cancel> - 取消实例。  
  
- <xref:System.ServiceModel.Activities.Description.WorkflowUnhandledExceptionAction.Terminate> - 终止实例。  
  
 可在代码中配置该行为，如下面的示例所示。  
  
```csharp  
host.Description.Behaviors.Add(new WorkflowUnhandledExceptionBehavior { Action = WorkflowUnhandledExceptionAction.Abandon });  
```  
  
 也可在配置文件中配置该行为，如下面的示例所示。  
  
```xml
<behaviors>  
      <serviceBehaviors>  
        <behavior>  
          <serviceMetadata httpGetEnabled="True"/>  
          <serviceDebug includeExceptionDetailInFaults="False" />  
          <workflowUnhandledExceptionBehavior action="Abandon" />
        </behavior>  
      </serviceBehaviors>  
</behaviors>
```  
  
## <a name="hosting-non-service-workflows"></a>承载非服务工作流  

 <xref:System.ServiceModel.Activities.WorkflowServiceHost> 可用于承载非服务工作流、未以 <xref:System.ServiceModel.Activities.Receive> 活动开头的工作流或未使用消息传递活动的工作流。 工作流服务通常以 <xref:System.ServiceModel.Activities.Receive> 活动开头。 如果 <xref:System.ServiceModel.Activities.WorkflowServiceHost> 在接收工作流服务的消息时未处于运行状态（或未保存），则会创建一个新的工作流服务实例。 如果工作流不以 Receive 活动开头，则无法通过发送消息来启动此工作流，因为没有用于接收消息的活动。 若要承载非服务工作流，请从 <xref:System.ServiceModel.Activities.WorkflowHostingEndpoint> 派生一个类，并重写 <xref:System.ServiceModel.Activities.WorkflowHostingEndpoint.OnGetInstanceId%2A>、<xref:System.ServiceModel.Activities.WorkflowHostingEndpoint.OnGetCreationContext%2A> 和 <xref:System.ServiceModel.Activities.WorkflowHostingEndpoint.OnResolveBookmark%2A>。 如果您想提供首选实例 ID，请重写 <xref:System.ServiceModel.Activities.WorkflowHostingEndpoint.OnGetInstanceId%2A>。 重写 <xref:System.ServiceModel.Activities.WorkflowHostingEndpoint.OnGetCreationContext%2A> 可以创建自定义工作流创建上下文或填充现有 <xref:System.ServiceModel.Activities.WorkflowCreationContext> 的实例。 重写 <xref:System.ServiceModel.Activities.WorkflowHostingEndpoint.OnResolveBookmark%2A> 可以从传入消息中手动提取书签。 如果重写此方法，则必须在其主体中调用 <xref:System.ServiceModel.Activities.WorkflowHostingResponseContext.SendResponse%2A>，以便响应发送给 WorkflowHostingEndpoint 的消息。 如果不这样做，则可能会最终超过 <xref:System.ServiceModel.Description.ServiceThrottlingBehavior.MaxConcurrentCalls%2A> 限制。 在双向协定中，您可能能够检测到未能调用 <xref:System.ServiceModel.Activities.WorkflowHostingResponseContext.SendResponse%2A>，因为客户端未能接收响应。 在单向协定中，您无法识别有关未能调用 <xref:System.ServiceModel.Activities.WorkflowHostingResponseContext.SendResponse%2A> 的错误，直至超过 <xref:System.ServiceModel.Description.ServiceThrottlingBehavior.MaxConcurrentCalls%2A> 中止值之后，此时为时已晚。 有关详细信息，请参阅 [WorkflowHostingEndpoint Resume 书签](../../windows-workflow-foundation/samples/workflowhostingendpoint-resume-bookmark.md)若要创建非服务工作流的新实例，请声明一个用于定义创建新实例的操作的服务协定。 创建操作应采用 IDictionary \<string, object> 来传递任何所需的工作流参数。 该协定由 <xref:System.ServiceModel.Activities.WorkflowHostingEndpoint> 派生类隐式实现。 当承载工作流时，请通过调用 <xref:System.ServiceModel.Activities.WorkflowHostingEndpoint> 向主机添加 <xref:System.ServiceModel.Activities.WorkflowServiceHost.AddServiceEndpoint%2A> 派生类的实例，然后调用 <xref:System.ServiceModel.Channels.CommunicationObject.Open%2A>。 若要创建工作流的实例，请创建所用服务协定类型的 <xref:System.ServiceModel.ChannelFactory%601>，并调用 <xref:System.ServiceModel.ChannelFactory%601.CreateChannel%2A>。 然后，可调用在服务协定中定义的创建操作。  
  
## <a name="see-also"></a>另请参阅

- [工作流服务](workflow-services.md)
- [消息传递活动](messaging-activities.md)
