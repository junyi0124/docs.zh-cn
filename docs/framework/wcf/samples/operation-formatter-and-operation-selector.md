---
title: 操作格式化程序和操作选择器
ms.date: 03/30/2017
ms.assetid: 1c27e9fe-11f8-4377-8140-828207b98a0e
ms.openlocfilehash: a49b466a940b63b70509ba76e62a9b9c5a36ad61
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96260014"
---
# <a name="operation-formatter-and-operation-selector"></a>操作格式化程序和操作选择器

此示例演示如何使用 Windows Communication Foundation (WCF) 扩展点来允许不同于 WCF 所需的格式的消息数据。 默认情况下，WCF 格式化程序应在元素下包含方法参数 `soap:body` 。 但是，此示例演示如何实现一个自定义操作格式化程序，用于分析 HTTP GET 查询字符串中的参数数据并使用该数据调用方法。  
  
 该示例基于实现服务协定的 [入门](getting-started-sample.md) `ICalculator` 。 它演示如何更改 Add、Subtract、Multiply 和 Divide 消息，以便对客户端到服务器请求使用 HTTP GET，对服务器到客户端响应使用带有 POX 消息的 HTTP POST。  
  
 为此，此示例提供了以下功能：  
  
- `QueryStringFormatter`，它分别为客户端和服务器实现 <xref:System.ServiceModel.Dispatcher.IClientMessageFormatter> 和 <xref:System.ServiceModel.Dispatcher.IDispatchMessageFormatter>，并处理查询字符串中的数据。  
  
- `UriOperationSelector`，它在服务器上实现 <xref:System.ServiceModel.Dispatcher.IDispatchOperationSelector>，以便基于 GET 请求中的操作名称执行操作调度。  
  
- `EnableHttpGetRequestsBehavior` 终结点行为（和对应的配置），它向运行库中添加必要的操作选择器。  
  
- 演示如何在运行库中插入新的操作格式化程序。  
  
- 在此示例中，客户端和服务都是控制台应用程序 (.exe)。  
  
> [!NOTE]
> 本主题的最后介绍了此示例的设置过程和生成说明。  
  
## <a name="key-concepts"></a>关键概念  

 `QueryStringFormatter` -操作格式化程序是 WCF 中的组件，该组件负责将消息转换为参数对象数组，并将参数对象数组转换为消息。 这在客户端是使用 <xref:System.ServiceModel.Dispatcher.IClientMessageFormatter> 接口完成的，在服务器上是使用 <xref:System.ServiceModel.Dispatcher.IDispatchMessageFormatter> 接口完成的。 通过这些接口，用户可以从 `Serialize` 和 `Deserialize` 方法获得请求和响应消息。  
  
 在此示例中，`QueryStringFormatter` 同时实现了这两个接口，并且在客户端和服务器上均已实现。  
  
 请求：  
  
- 此示例使用 <xref:System.ComponentModel.TypeConverter> 类将请求消息中的参数数据转换为字符串，并将字符串转换为参数数据。 如果 <xref:System.ComponentModel.TypeConverter> 对某个特定的类型不可用，示例格式化程序将引发异常。  
  
- 在客户端上的 `IClientMessageFormatter.SerializeRequest` 方法中，格式化程序创建具有相应“收件人”地址的 URI，并将操作名称作为后缀附加在后面。 此名称用于调度给服务器上的相应操作。 然后它采用参数对象数组，并使用 <xref:System.ComponentModel.TypeConverter> 类转换的参数名称和值将参数数据序列化为 URI 查询字符串。 接着，<xref:System.ServiceModel.Channels.MessageHeaders.To%2A> 和 <xref:System.ServiceModel.Channels.MessageProperties.Via%2A> 属性设置为该 URI。 通过 <xref:System.ServiceModel.Channels.MessageProperties> 属性访问 <xref:System.ServiceModel.Channels.Message.Properties%2A>。  
  
- 在服务器上的 `IDispatchMessageFormatter.DeserializeRequest` 方法中，格式化程序在传入请求消息属性中检索 `Via` URI。 它将 URI 查询字符串中的名称-值对分析为参数名称和值，并使用参数名称和值填充传递给该方法的参数数组。 请注意，因为操作调度已经发生，所以此方法中忽略了操作名称后缀。  
  
 响应：  
  
- 在此示例中，HTTP GET 只用于请求。 该格式化程序将响应的发送委托给本来用于生成 XML 消息的原始格式化程序。 此示例的目的之一是演示如何实现此类委托格式化程序。  
  
### <a name="uripathsuffixoperationselector-class"></a>UriPathSuffixOperationSelector 类  

 用户可以通过 <xref:System.ServiceModel.Dispatcher.IDispatchOperationSelector> 接口实现自己的逻辑，决定应为哪个操作调度特定的消息。  
  
 在此示例中，必须在服务器上实现 `UriPathSuffixOperationSelector` 以选择合适的操作，因为操作名称包含在 HTTP GET URI 中，而不是包含在消息的操作标头中。 此示例设置为只允许不区分大小写的操作名称。  
  
 `SelectOperation` 方法采用传入消息，并在其消息属性中查找 `Via` URI。 它从 URI 中提取操作名称后缀，查看内部表以获取应将该消息调度给的操作名称，并返回该操作名称。  
  
### <a name="enablehttpgetrequestsbehavior-class"></a>EnableHttpGetRequestsBehavior 类  

 `UriPathSuffixOperationSelector` 组件可以通过编程方式或者通过终结点行为进行设置。 此示例实现了 `EnableHttpGetRequestsBehavior` 行为，这是在服务的应用程序配置文件中指定的。  
  
 在服务器上：  
  
 <xref:System.ServiceModel.Dispatcher.DispatchRuntime.OperationSelector%2A> 设置为 <xref:System.ServiceModel.Dispatcher.IDispatchOperationSelector> 实现。  
  
 默认情况下，WCF 使用完全匹配地址筛选器。 传入消息中的 URI 包含一个操作名称后缀，后跟包含参数数据的查询字符串，因此终结点行为还将地址筛选器更改为前缀匹配筛选器。 它使用 WCF <xref:System.ServiceModel.Dispatcher.PrefixEndpointAddressMessageFilter> 实现此目的。  
  
### <a name="installing-operation-formatters"></a>安装操作格式化程序  

 指定格式化程序的操作行为是唯一的。 默认情况下始终为每个操作实现这样一个行为，以创建必要的操作格式化程序。 但是，这些行为看起来就像另一个操作行为；它们不能通过其他任何属性进行标识。 若要安装替换行为，实现必须查找默认情况下由 WCF 类型加载程序安装的特定格式化程序行为，并将其替换或添加兼容的行为，以便在默认行为之后运行。  
  
 这些操作格式化程序行为可以在调用 <xref:System.ServiceModel.Channels.CommunicationObject.Open%2A?displayProperty=nameWithType> 之前通过编程方式进行设置，或者通过指定一个在默认行为之后执行的操作行为来进行设置。 但是，通过终结点行为（并进而通过配置）并不能轻松完成设置，因为行为模式不允许用一个行为替换另一个行为，或者通过其他方式修改说明树。  
  
 在客户端上：  
  
 必须实现 <xref:System.ServiceModel.Dispatcher.IClientMessageFormatter> 实现，以使其能够将请求转换为 HTTP GET 请求，并委托原始格式化程序做出响应。 这是通过调用 `EnableHttpGetRequestsBehavior.ReplaceFormatterBehavior` 帮助器方法实现的。  
  
 这必须在调用 `CreateChannel` 之前完成。  
  
```csharp  
void ReplaceFormatterBehavior(OperationDescription operationDescription, EndpointAddress address)  
{  
    // Remove the DataContract behavior if it is present.  
    IOperationBehavior formatterBehavior = operationDescription.Behaviors.Remove<DataContractSerializerOperationBehavior>();  
    if (formatterBehavior == null)  
    {  
        // Remove the XmlSerializer behavior if it is present.  
        formatterBehavior = operationDescription.Behaviors.Remove<XmlSerializerOperationBehavior>();  
        ...  
    }  
  
    // Remember what the innerFormatterBehavior was.  
    DelegatingFormatterBehavior delegatingFormatterBehavior = new DelegatingFormatterBehavior(address);  
    delegatingFormatterBehavior.InnerFormatterBehavior = formatterBehavior;  
   operationDescription.Behaviors.Add(delegatingFormatterBehavior);  
}  
```  
  
 在服务器上：  
  
- 必须实现 <xref:System.ServiceModel.Dispatcher.IDispatchMessageFormatter> 接口，以使其能够读取 HTTP GET 请求，并委托原始格式化程序编写响应。 这是通过调用与客户端相同的 `EnableHttpGetRequestsBehavior.ReplaceFormatterBehavior` 帮助器方法实现的（请参见前面的代码示例）。  
  
- 这必须在调用 <xref:System.ServiceModel.Channels.CommunicationObject.Open%2A> 之前完成。 在此示例中，我们演示如何在调用 <xref:System.ServiceModel.Channels.CommunicationObject.Open%2A> 之前手动修改格式化程序。 实现同一目标的另一种方式是从 <xref:System.ServiceModel.ServiceHost> 派生一个类，以便在打开之前调用 `EnableHttpGetRequestsBehavior.ReplaceFormatterBehavior`（有关示例，请参见承载文档和示例）。  
  
### <a name="user-experience"></a>用户体验  

 在服务器上：  
  
- 服务器 `ICalculator` 实现不需要更改。  
  
- 服务的 App.config 必须使用自定义 POX 绑定，该绑定将 `messageVersion` 元素的 `textMessageEncoding` 属性设置为 `None`。  
  
    ```xml  
    <bindings>  
      <customBinding>  
        <binding name="poxBinding">  
          <textMessageEncoding messageVersion="None" />  
          <httpTransport />  
        </binding>  
      </customBinding>  
    </bindings>  
    ```  
  
- 服务的 App.config 还必须指定自定义 `EnableHttpGetRequestsBehavior`（通过将其添加到行为扩展部分并使用它来实现）。  
  
    ```xml  
    <behaviors>  
      <endpointBehaviors>  
        <behavior name="enableHttpGetRequestsBehavior">  
          <enableHttpGetRequests />  
        </behavior>  
      </endpointBehaviors>  
    </behaviors>  
  
    <extensions>  
      <behaviorExtensions>  
        <!-- Enabling HTTP GET requests: Behavior Extension -->  
        <add
          name="enableHttpGetRequests"           type="Microsoft.ServiceModel.Samples.EnableHttpGetRequestsBehaviorElement, QueryStringFormatter, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />  
      </behaviorExtensions>  
    </extensions>  
    ```  
  
- 在调用 <xref:System.ServiceModel.Channels.CommunicationObject.Open%2A> 之前添加操作格式化程序。  
  
 在客户端上：  
  
- 客户端实现不需要更改。  
  
- 客户端 App.config 必须使用自定义 POX 绑定，该绑定将 `messageVersion` 元素的 `textMessageEncoding` 属性设置为 `None`。 与服务的一个区别在于，客户端必须允许手动寻址，以便修改传出“收件人”地址。  
  
    ```xml  
    <bindings>  
      <customBinding>  
        <binding name="poxBinding">  
          <textMessageEncoding messageVersion="None" />  
          <httpTransport manualAddressing="True" />  
        </binding>  
      </customBinding>  
    </bindings>  
    ```  
  
- 客户端 App.config 必须指定与服务器相同的自定义 `EnableHttpGetRequestsBehavior`。  
  
- 在调用 <xref:System.ServiceModel.ChannelFactory%601.CreateChannel> 之前添加操作格式化程序。  
  
 运行示例时，操作请求和响应将显示在客户端控制台窗口中。 所有四个操作（Add、Subtract、Multiply 和 Divide）都必须成功。  
  
> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> 如果此目录不存在，请参阅[Windows Communication Foundation (wcf) ，并 Windows Workflow Foundation (的 WF](https://www.microsoft.com/download/details.aspx?id=21459)) .NET Framework Windows Communication Foundation ([!INCLUDE[wf1](../../../../includes/wf1-md.md)] 此示例位于以下目录：  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Extensibility\Formatters\QueryStringFormatter`  
  
##### <a name="to-set-up-build-and-run-the-sample"></a>设置、生成和运行示例  
  
1. 确保已对 [Windows Communication Foundation 示例执行了一次性安装过程](one-time-setup-procedure-for-the-wcf-samples.md)。  
  
2. 若要生成解决方案，请按照 [生成 Windows Communication Foundation 示例](building-the-samples.md)中的说明进行操作。  
  
3. 若要以单机配置或跨计算机配置来运行示例，请按照 [运行 Windows Communication Foundation 示例](running-the-samples.md)中的说明进行操作。  
