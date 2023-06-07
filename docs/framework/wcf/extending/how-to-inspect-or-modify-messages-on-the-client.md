---
title: 如何：检查或修改客户端上的消息
description: 了解如何通过实现相应的接口来检查或修改 WCF 客户端或服务上的传入或传出消息。
ms.date: 03/30/2017
ms.assetid: b8256335-f1c2-419f-b862-9f220ccad84c
ms.openlocfilehash: 1f8b75001754739b48d10ee577ae26a175e72860
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96249041"
---
# <a name="how-to-inspect-or-modify-messages-on-the-client"></a>如何：检查或修改客户端上的消息

通过实现 <xref:System.ServiceModel.Dispatcher.IClientMessageInspector?displayProperty=nameWithType> 并将其插入到客户端运行时中，可以通过 WCF 客户端检查或修改传入或传出消息。 有关详细信息，请参阅 [扩展客户端](extending-clients.md)。 服务上的等效功能为 <xref:System.ServiceModel.Dispatcher.IDispatchMessageInspector?displayProperty=nameWithType>。 有关完整的代码示例，请参阅 [消息检查](../samples/message-inspectors.md) 器示例。  
  
### <a name="to-inspect-or-modify-messages"></a>检查或修改消息  
  
1. 实现 <xref:System.ServiceModel.Dispatcher.IClientMessageInspector?displayProperty=nameWithType> 接口。  
  
2. 实现 <xref:System.ServiceModel.Description.IEndpointBehavior?displayProperty=nameWithType> 或 <xref:System.ServiceModel.Description.IContractBehavior?displayProperty=nameWithType>，具体取决于您希望在其中插入客户端消息检查器的作用域。 <xref:System.ServiceModel.Description.IEndpointBehavior?displayProperty=nameWithType> 允许您在终结点级别更改行为。 <xref:System.ServiceModel.Description.IContractBehavior?displayProperty=nameWithType> 允许您更改协定级别的行为。  
  
3. 在 <xref:System.ServiceModel.ClientBase%601.Open%2A?displayProperty=nameWithType> 上调用 <xref:System.ServiceModel.ICommunicationObject.Open%2A?displayProperty=nameWithType> 或 <xref:System.ServiceModel.ChannelFactory%601?displayProperty=nameWithType> 方法前，插入行为。 有关详细信息，请参阅 [配置和扩展运行时的行为](configuring-and-extending-the-runtime-with-behaviors.md)。  
  
## <a name="example"></a>示例  

 下面的代码示例按顺序演示以下各项：  
  
- 客户端检查器实现。  
  
- 插入检查器的终结点行为。  
  
- 一个 <xref:System.ServiceModel.Configuration.BehaviorExtensionElement> 派生类，允许您在配置文件中添加行为。  
  
- 一个配置文件，它添加终结点行为，以便在客户端运行时中插入客户端消息检查器。  
  
```csharp  
// Client message inspector  
public class SimpleMessageInspector : IClientMessageInspector  
{  
    public void AfterReceiveReply(ref System.ServiceModel.Channels.Message reply, object correlationState)  
    {  
        // Implement this method to inspect/modify messages after a message  
        // is received but prior to passing it back to the client
        Console.WriteLine("AfterReceiveReply called");  
    }  
  
    public object BeforeSendRequest(ref System.ServiceModel.Channels.Message request, IClientChannel channel)  
    {  
        // Implement this method to inspect/modify messages before they
        // are sent to the service  
        Console.WriteLine("BeforeSendRequest called");  
        return null;  
    }  
}  
```  
  
```csharp  
// Endpoint behavior  
public class SimpleEndpointBehavior : IEndpointBehavior  
{  
    public void AddBindingParameters(ServiceEndpoint endpoint, System.ServiceModel.Channels.BindingParameterCollection bindingParameters)  
    {  
         // No implementation necessary  
    }  
  
    public void ApplyClientBehavior(ServiceEndpoint endpoint, ClientRuntime clientRuntime)  
    {  
        clientRuntime.MessageInspectors.Add(new SimpleMessageInspector());  
    }  
  
    public void ApplyDispatchBehavior(ServiceEndpoint endpoint, EndpointDispatcher endpointDispatcher)  
    {  
         // No implementation necessary  
    }  
  
    public void Validate(ServiceEndpoint endpoint)  
    {  
         // No implementation necessary  
    }  
}  
```  
  
```csharp  
// Configuration element
public class SimpleBehaviorExtensionElement : BehaviorExtensionElement  
{  
    public override Type BehaviorType  
    {  
        get { return typeof(SimpleEndpointBehavior); }  
    }  
  
    protected override object CreateBehavior()  
    {  
         // Create the  endpoint behavior that will insert the message  
         // inspector into the client runtime  
        return new SimpleEndpointBehavior();  
    }  
}  
```  
  
```xml
<?xml version="1.0" encoding="utf-8" ?>  
<configuration>  
    <system.serviceModel>  
        <client>  
            <endpoint address="http://localhost:8080/SimpleService/"
                      binding="wsHttpBinding"
                      behaviorConfiguration="clientInspectorsAdded"
                      contract="ServiceReference1.IService1"  
                      name="WSHttpBinding_IService1"/>  
        </client>  
  
      <behaviors>  
        <endpointBehaviors>  
          <behavior name="clientInspectorsAdded">  
            <simpleBehaviorExtension />  
          </behavior>  
        </endpointBehaviors>  
      </behaviors>  
      <extensions>  
        <behaviorExtensions>  
          <add  
            name="simpleBehaviorExtension"  
            type="SimpleServiceLib.SimpleBehaviorExtensionElement, Host, Version=0.0.0.0, Culture=neutral, PublicKeyToken=null"/>  
        </behaviorExtensions>  
      </extensions>  
    </system.serviceModel>  
</configuration>  
```  
  
## <a name="see-also"></a>另请参阅

- <xref:System.ServiceModel.Dispatcher.IClientMessageInspector?displayProperty=nameWithType>
- <xref:System.ServiceModel.Dispatcher.IDispatchMessageInspector?displayProperty=nameWithType>
- [使用行为配置和扩展运行时](configuring-and-extending-the-runtime-with-behaviors.md)
