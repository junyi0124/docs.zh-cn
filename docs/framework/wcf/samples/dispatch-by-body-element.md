---
title: 按正文元素调度
ms.date: 03/30/2017
ms.assetid: f64a3c04-62b4-47b2-91d9-747a3af1659f
ms.openlocfilehash: ddff361179c2ef071ca4df076e78b238de9041a1
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96292579"
---
# <a name="dispatch-by-body-element"></a>按正文元素调度

本示例演示如何实现用于将传入消息分配到操作的可选算法。  
  
 默认情况下，服务模型调度程序会根据消息的 WS-Addressing“Action”标头或 HTTP SOAP 请求中的等效信息为传入消息选择适当的处理方法。  
  
 有些不遵循 WS-I Basic Profile 1.1 准则的 SOAP 1.1 Web 服务堆栈并不根据 Action URI 来调度消息，而是根据 SOAP 正文内第一个元素的 XML 限定名来调度消息。 同样，这些堆栈的客户端可能会发送具有空标头或任意 HTTP SoapAction 标头的消息，这是 SOAP 1.1 规范所允许的。  
  
 为了更改向方法调度消息的方式，此示例在 <xref:System.ServiceModel.Dispatcher.IDispatchOperationSelector> 上实现 `DispatchByBodyElementOperationSelector` 扩展性接口。 此类根据消息正文的第一个元素来选择操作。  
  
 类构造函数需要一个用 `XmlQualifiedName` 和字符串对填充的字典，其中限定名称指示 SOAP 正文的第一个子级的名称，而字符串指示匹配的操作名称。 `defaultOperationName` 是操作的名称，该操作接收无法与此字典匹配的所有消息：  
  
```csharp
class DispatchByBodyElementOperationSelector : IDispatchOperationSelector  
{  
    Dictionary<XmlQualifiedName, string> dispatchDictionary;  
    string defaultOperationName;  
  
    public DispatchByBodyElementOperationSelector(Dictionary<XmlQualifiedName,string> dispatchDictionary, string defaultOperationName)  
    {  
        this.dispatchDictionary = dispatchDictionary;  
        this.defaultOperationName = defaultOperationName;  
    }  
}
```  
  
 <xref:System.ServiceModel.Dispatcher.IDispatchOperationSelector> 实现的生成非常简单，因为在接口上只有一个方法：<xref:System.ServiceModel.Dispatcher.IDispatchOperationSelector.SelectOperation%2A>。 此方法的任务是检查传入消息并返回一个字符串，该字符串为当前终结点服务协定上的方法的名称。  
  
 在此示例中，操作选择器使用 <xref:System.Xml.XmlDictionaryReader> 获取传入消息正文的 <xref:System.ServiceModel.Channels.Message.GetReaderAtBodyContents%2A>。 此方法已经将读取器定位在消息正文的第一个子级上，因此完全可以获取当前元素的名称和命名空间 URI 并将它们结合成 `XmlQualifiedName`，然后用它来从操作选择器保存的字典中查找相应的操作。  
  
```csharp
public string SelectOperation(ref System.ServiceModel.Channels.Message message)  
{  
    XmlDictionaryReader bodyReader = message.GetReaderAtBodyContents();  
    XmlQualifiedName lookupQName = new  
       XmlQualifiedName(bodyReader.LocalName, bodyReader.NamespaceURI);  
    message = CreateMessageCopy(message,bodyReader);  
    if (dispatchDictionary.ContainsKey(lookupQName))  
    {  
         return dispatchDictionary[lookupQName];  
    }  
    else  
    {  
        return defaultOperationName;  
    }  
}  
```  
  
 访问消息正文时，不管使用的是 <xref:System.ServiceModel.Channels.Message.GetReaderAtBodyContents%2A> 还是可以访问消息正文内容的任何其他方法，都将导致该消息被标记为“已读”，这意味着该消息不能进行任何进一步处理。 因此，操作选择器将使用下面代码演示的方法创建传入消息的副本。 因为在检查过程中读取器的位置没有更改，可以由新创建的消息引用，消息属性和消息头也会复制到新创建的消息中，因此可以得到与原始消息完全相同的克隆：  
  
```csharp
private Message CreateMessageCopy(Message message,
                                     XmlDictionaryReader body)  
{  
    Message copy = Message.CreateMessage(message.Version,message.Headers.Action,body);  
    copy.Headers.CopyHeaderFrom(message,0);  
    copy.Properties.CopyProperties(message.Properties);  
    return copy;  
}  
```  
  
## <a name="adding-an-operation-selector-to-a-service"></a>向服务添加操作选择器  

 服务调度操作选择器是 Windows Communication Foundation (WCF) 调度程序的扩展。 在双工协定回调通道上选择方法时，还可以使用客户端操作选择器，其工作方式类似于本文说明的调度操作选择器，但未显式包括在此示例中。  
  
 和多数服务模型扩展一样，调度操作选择器通过使用行为来添加到调度程序。 *行为* 是配置对象，它将一个或多个扩展添加到调度运行时 (或添加到客户端运行时) 或更改其设置。  
  
 由于操作选择器具有协定范围，因此本示例要实现的适当行为是 <xref:System.ServiceModel.Description.IContractBehavior>。 由于接口是在 <xref:System.Attribute> 派生类上实现的（如下面的代码所示），因此可以以声明方式将行为添加到任何服务协定。 每当打开 <xref:System.ServiceModel.ServiceHost> 并生成调度运行时时，都会自动添加作为协定、操作和服务实现上的属性的所有行为或作为服务配置中的元素的所有行为，随后要求这些行为提供扩展或修改默认设置。  
  
 为了简洁起见，下面的代码摘录仅演示 <xref:System.ServiceModel.Description.IContractBehavior.ApplyDispatchBehavior%2A> 方法的实现，此方法影响本示例中调度程序的配置更改。 未演示其他方法，因为它们不执行任何操作就返回到调用方。  
  
```csharp
[AttributeUsage(AttributeTargets.Class|AttributeTargets.Interface)]  
class DispatchByBodyElementBehaviorAttribute : Attribute, IContractBehavior  
{  
    // public void AddBindingParameters(...)
    // public void ApplyClientBehavior(...)  
    // public void Validate(...)  
```  
  
 首先，<xref:System.ServiceModel.Description.IContractBehavior.ApplyDispatchBehavior%2A> 实现通过遍历服务终结点的 <xref:System.ServiceModel.Description.OperationDescription> 中的 <xref:System.ServiceModel.Description.ContractDescription> 元素为操作选择器设置查找字典。 然后检查每个操作说明是否存在 `DispatchBodyElementAttribute` 行为，即也在此示例中定义的 <xref:System.ServiceModel.Description.IOperationBehavior> 的实现。 虽然此类也是一种行为，但它是被动行为，不会主动向调度运行时提供任何配置更改。 其所有方法都不执行任何操作就返回到调用方。 此操作行为的存在只是为了新调度机制所要求的元数据（即在存在时将会选择一种操作的正文元素的限定名）可以与相应的操作相关联。  
  
 如果找到这样的行为，将会向字典添加使用 XML 限定名（`QName` 属性）和操作名（`Name` 属性）创建的值对。  
  
 填充字典后，将使用此信息构造一个新的 `DispatchByBodyElementOperationSelector` 并将其设置为调度运行时的操作选择器：  
  
```csharp
public void ApplyDispatchBehavior(ContractDescription contractDescription, ServiceEndpoint endpoint, System.ServiceModel.Dispatcher.DispatchRuntime dispatchRuntime)  
{  
    Dictionary<XmlQualifiedName,string> dispatchDictionary =
                     new Dictionary<XmlQualifiedName,string>();  
    foreach( OperationDescription operationDescription in
                              contractDescription.Operations )  
    {  
        DispatchBodyElementAttribute dispatchBodyElement =
   operationDescription.Behaviors.Find<DispatchBodyElementAttribute>();  
        if ( dispatchBodyElement != null )  
        {  
             dispatchDictionary.Add(dispatchBodyElement.QName,
                              operationDescription.Name);  
        }  
    }  
    dispatchRuntime.OperationSelector =
            new DispatchByBodyElementOperationSelector(  
               dispatchDictionary,
               dispatchRuntime.UnhandledDispatchOperation.Name);  
    }  
}  
```  
  
## <a name="implementing-the-service"></a>实现服务  

 本示例中实现的行为直接影响如何解释和调度来自网络的消息，这是服务协定的功能。 因此，在选择使用该行为的任何服务实现中，均应在服务协定级别声明该行为。  
  
 示例项目服务将 `DispatchByBodyElementBehaviorAttribute` 协定行为应用于 `IDispatchedByBody` 服务协定，并标记两个操作 `OperationForBodyA()` 和 `OperationForBodyB()` 一个 `DispatchBodyElementAttribute` 操作行为。 如前面所述，在打开实现此协定的服务的服务主机时，调度程序生成器将选取此元数据。  
  
 由于操作选择器只根据消息正文元素进行调度并忽略“Action”，因此需要通过将通配符“*”分配给 `ReplyAction` 的 <xref:System.ServiceModel.OperationContractAttribute> 属性来告诉运行时不要对返回的答复检查“Action”标头。 此外，需要将 "Action" 属性的默认操作设置为通配符 " \* "。 此默认操作接收所有无法调度的消息并且没有 `DispatchBodyElementAttribute`：  
  
```csharp
[ServiceContract(Namespace="http://Microsoft.ServiceModel.Samples"),  
                            DispatchByBodyElementBehavior]  
public interface IDispatchedByBody  
{  
    [OperationContract(ReplyAction="*"),
     DispatchBodyElement("bodyA","http://tempuri.org")]  
    Message OperationForBodyA(Message msg);  
    [OperationContract(ReplyAction = "*"),
     DispatchBodyElement("bodyB", "http://tempuri.org")]  
    Message OperationForBodyB(Message msg);  
    [OperationContract(Action="*", ReplyAction="*")]  
    Message DefaultOperation(Message msg);  
}  
```  
  
 示例服务的实现非常简单。 每个方法都可将接收的消息包装成答复消息并将其回送到客户端。  
  
## <a name="running-and-building-the-sample"></a>运行和生成示例  

 运行示例时，操作响应的正文内容将显示在客户端控制台窗口中，类似于下面的（格式化）输出。  
  
 客户端向服务发送三个消息，它们的正文内容元素分别命名为 `bodyA`、`bodyB` 和 `bodyX`。 从前面的说明和演示的服务协定可以推断，带有 `bodyA` 元素的传入消息将调度到 `OperationForBodyA()` 方法。 因为对于带有 `bodyX` 正文元素的消息没有显式调度目标，因此该消息将被调度到 `DefaultOperation()`。 每个服务操作都会将接收到的消息正文包装成特定于方法的元素并返回此元素，对于本示例，这样做是为了明确地关联输入和输出消息：  
  
```xml  
<?xml version="1.0" encoding="IBM437"?>  
<replyBodyA xmlns="http://tempuri.org">  
   <q:bodyA xmlns:q="http://tempuri.org">test</q:bodyA>  
</replyBodyA>  
<?xml version="1.0" encoding="IBM437"?>  
<replyBodyB xmlns="http://tempuri.org">  
  <q:bodyB xmlns:q="http://tempuri.org">test</q:bodyB>  
</replyBodyB>  
<?xml version="1.0" encoding="IBM437"?>  
<replyDefault xmlns="http://tempuri.org">  
   <q:bodyX xmlns:q="http://tempuri.org">test</q:bodyX>  
</replyDefault>  
```  
  
#### <a name="to-set-up-build-and-run-the-sample"></a>设置、生成和运行示例  
  
1. 确保已对 [Windows Communication Foundation 示例执行了一次性安装过程](one-time-setup-procedure-for-the-wcf-samples.md)。  
  
2. 若要生成解决方案，请按照 [生成 Windows Communication Foundation 示例](building-the-samples.md)中的说明进行操作。  
  
3. 若要以单机配置或跨计算机配置来运行示例，请按照 [运行 Windows Communication Foundation 示例](running-the-samples.md)中的说明进行操作。  
  
> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> 如果此目录不存在，请参阅[Windows Communication Foundation (wcf) ，并 Windows Workflow Foundation (的 WF](https://www.microsoft.com/download/details.aspx?id=21459)) .NET Framework Windows Communication Foundation ([!INCLUDE[wf1](../../../../includes/wf1-md.md)] 此示例位于以下目录：  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Extensibility\Interop\AdvancedDispatchByBody`  
