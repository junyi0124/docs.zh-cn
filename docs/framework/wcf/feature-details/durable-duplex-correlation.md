---
title: 持久双工相关
ms.date: 03/30/2017
ms.assetid: 8eb0e49a-6d3b-4f7e-a054-0d4febee2ffb
ms.openlocfilehash: eb879c583b4454cd0062396d86e157a90db4652f
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96254228"
---
# <a name="durable-duplex-correlation"></a>持久双工相关

当工作流服务要求向初始调用方发送回调时，持久双工相关性（也称为回调相关性）非常有用。 与 WCF 双工不同的是，该回调可以在将来的任何时间发生，并且不会绑定到同一通道或通道生存期；唯一要求是，调用方应具有一个用于侦听回调消息的活动终结点。 这样，这两个工作流服务便可在长期运行的对话中进行通信。 本主题概述了持久双工相关性。  
  
## <a name="using-durable-duplex-correlation"></a>使用持久双工相关性  

 若要使用持久双工相关性，这两个服务必须使用启用了上下文且支持双向操作的绑定，如 <xref:System.ServiceModel.NetTcpContextBinding> 或 <xref:System.ServiceModel.WSHttpContextBinding>。 调用服务使用所需绑定向其客户端 <xref:System.ServiceModel.WSHttpContextBinding.ClientCallbackAddress%2A> 注册 <xref:System.ServiceModel.Endpoint>。 接收服务将接收初始调用中的上述数据，然后在回调调用服务的 <xref:System.ServiceModel.Endpoint> 活动中对自己的 <xref:System.ServiceModel.Activities.Send> 使用此数据。 在本示例中，这两个服务互相通信。 第一个服务对第二个服务调用某个方法，然后等待答复。 第二个服务知晓回调方法的名称，但在设计时，实现该方法的服务的终结点是未知的。  
  
> [!NOTE]
> 仅当终结点的 <xref:System.ServiceModel.Channels.AddressingVersion> 配置有 <xref:System.ServiceModel.Channels.AddressingVersion.WSAddressing10%2A> 时，才能使用持久双工。 如果不是，则 <xref:System.InvalidOperationException> 引发异常，并出现以下消息： "消息包含具有 [AddressingVersion](http://schemas.xmlsoap.org/ws/2004/08/addressing)的终结点引用的回调上下文标头。 仅当 AddressingVersion 配置为 "WSAddressing10" 时，才能传输回调上下文。
  
 在下面的示例中，使用 <xref:System.ServiceModel.Endpoint> 承载工作流服务，该服务将创建回调 <xref:System.ServiceModel.WSHttpContextBinding>。  
  
```csharp  
// Host WF Service 1.  
string baseAddress1 = "http://localhost:8080/Service1";  
WorkflowServiceHost host1 = new WorkflowServiceHost(GetWF1(), new Uri(baseAddress1));  
  
// Add the callback endpoint.  
WSHttpContextBinding Binding1 = new WSHttpContextBinding();  
host1.AddServiceEndpoint("ICallbackItemsReady", Binding1, "ItemsReady");  
  
// Add the service endpoint.  
host1.AddServiceEndpoint("IService1", Binding1, baseAddress1);  
  
// Open the first workflow service.  
host1.Open();  
Console.WriteLine("Service1 waiting at: {0}", baseAddress1);  
```  
  
 实现此工作流服务的工作流通过其 <xref:System.ServiceModel.Activities.Send> 活动初始化回调相关性，并从与 <xref:System.ServiceModel.Activities.Receive> 相关的 <xref:System.ServiceModel.Activities.Send> 活动引用此回调终结点。 下面的示例表示通过 `GetWF1` 方法返回的工作流。  
  
```csharp  
Variable<CorrelationHandle> CallbackHandle = new Variable<CorrelationHandle>();  
  
Receive StartOrder = new Receive  
{  
    CanCreateInstance = true,  
    ServiceContractName = "IService1",  
    OperationName = "StartOrder"  
};  
  
Send GetItems = new Send  
{  
    CorrelationInitializers =
    {  
        new CallbackCorrelationInitializer  
        {  
            CorrelationHandle = CallbackHandle  
        }  
    },  
    ServiceContractName = "IService2",  
    OperationName = "StartItems",  
    Endpoint = new Endpoint  
    {  
        AddressUri = new Uri("http://localhost:8081/Service2"),  
        Binding = new WSHttpContextBinding  
        {  
            ClientCallbackAddress = new Uri("http://localhost:8080/Service1/ItemsReady")
        }  
    }  
};  
  
Receive ItemsReady = new Receive  
{  
    ServiceContractName = "ICallbackItemsReady",  
    OperationName = "ItemsReady",  
    CorrelatesWith = CallbackHandle,  
};  
  
Activity wf = new Sequence  
{  
    Variables =  
    {  
        CallbackHandle  
    },  
    Activities =  
    {  
        StartOrder,  
        new WriteLine  
        {  
            Text = "WF1 - Started"  
        },  
        GetItems,  
        new WriteLine  
        {  
            Text = "WF1 - Request Submitted"  
        },  
        ItemsReady,  
        new WriteLine  
        {  
            Text = "WF1 - Items Received"  
        }  
     }  
};  
```  
  
 使用系统提供的基于上下文的绑定来承载第二个工作流服务。  
  
```csharp  
// Host WF Service 2.  
string baseAddress2 = "http://localhost:8081/Service2";  
WorkflowServiceHost host2 = new WorkflowServiceHost(GetWF2(), new Uri(baseAddress2));  
  
// Add the service endpoint.  
WSHttpContextBinding Binding2 = new WSHttpContextBinding();  
host2.AddServiceEndpoint("IService2", Binding2, baseAddress2);  
  
// Open the second workflow service.  
host2.Open();  
Console.WriteLine("Service2 waiting at: {0}", baseAddress2);  
```  
  
 实现此工作流服务的工作流以 <xref:System.ServiceModel.Activities.Receive> 活动开头。 此 Receive 活动初始化此服务的回调相关性，延迟一段时间以模拟长时间运行的工作，然后使用第一次调用第一个服务时传递的回调上下文回调来该服务。 下面的示例表示通过调用 `GetWF2` 返回的工作流。 请注意，<xref:System.ServiceModel.Activities.Send> 活动包含占位符地址 `http://www.contoso.com`；在运行时使用的实际地址即为提供的回调地址。  
  
```csharp  
Variable<CorrelationHandle> ItemsCallbackHandle = new Variable<CorrelationHandle>();  
  
Receive StartItems = new Receive  
{  
    CorrelationInitializers =
    {  
        new CallbackCorrelationInitializer  
        {  
            CorrelationHandle = ItemsCallbackHandle  
        }  
    },  
    CanCreateInstance = true,  
    ServiceContractName = "IService2",  
    OperationName = "StartItems"  
};  
  
Send ItemsReady = new Send  
{  
    CorrelatesWith = ItemsCallbackHandle,  
    Endpoint = new Endpoint  
    {  
        // The callback address on the binding is used  
        // instead of this placeholder address.  
        AddressUri = new Uri("http://www.contoso.com"),  
  
        Binding = new WSHttpContextBinding()  
    },  
    OperationName = "ItemsReady",  
    ServiceContractName = "ICallbackItemsReady"  
};  
  
Activity wf = new Sequence  
{  
    Variables =  
    {  
        ItemsCallbackHandle  
    },  
    Activities =  
    {  
        StartItems,  
        new WriteLine  
        {  
            Text = "WF2 - Request Received"  
        },  
        new Delay  
        {  
            Duration = TimeSpan.FromMinutes(90)  
        },  
        new WriteLine  
        {  
            Text = "WF2 - Sending items"  
        },  
        ItemsReady,  
        new WriteLine  
        {  
            Text = "WF2 - Items sent"  
        }  
     }  
};  
```  
  
 当对第一个工作流调用 `StartOrder` 方法时，将显示以下输出，该输出显示经过两个工作流的执行流。  
  
```output  
Service1 waiting at: http://localhost:8080/Service1  
Service2 waiting at: http://localhost:8081/Service2  
Press enter to exit.
WF1 - Started  
WF2 - Request Received  
WF1 - Request Submitted  
WF2 - Sending items  
WF2 - Items sent  
WF1 - Items Received  
```  
  
 在本示例中，两个工作流均使用 <xref:System.ServiceModel.Activities.CallbackCorrelationInitializer> 显式管理相关性。 由于上述示例工作流中仅存在一个相关性，因此默认的 <xref:System.ServiceModel.Activities.CorrelationHandle> 管理足以满足需求。
