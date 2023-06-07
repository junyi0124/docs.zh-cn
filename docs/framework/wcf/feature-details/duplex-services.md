---
title: 双工服务
description: 了解如何在 WCF 中创建双工服务协定，这允许两个终结点通过客户端创建的通道互相发送消息。
ms.date: 05/09/2018
dev_langs:
- csharp
- vb
ms.assetid: 396b875a-d203-4ebe-a3a1-6a330d962e95
ms.openlocfilehash: a43bb63a0ccf1a34b79dce755c19f7ed4cb6c16c
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2020
ms.locfileid: "85247346"
---
# <a name="duplex-services"></a>双工服务

双工服务协定是一种消息交换模式，其中双方终结点都可独立向对方发送消息。 因此，双工服务可以将消息发送回客户端终结点，从而提供类似事件的行为。 当客户端连接到服务并为服务提供可用来将消息发送回客户端的通道时，就会发生双工通信。 请注意，双工服务的类似事件的行为仅在会话中起作用。

若要创建双工协定，需要创建一对接口。 第一个接口是描述客户端可调用的操作的服务协定接口。 该服务协定必须在属性中指定一个*回调协定* <xref:System.ServiceModel.ServiceContractAttribute.CallbackContract%2A?displayProperty=nameWithType> 。 回调协定是定义服务可在客户端终结点上调用的操作的接口。 虽然系统提供的双工绑定利用了会话，但是双工协定不需要会话。

下面是双工协定的示例。

[!code-csharp[c_DuplexServices#0](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_duplexservices/cs/service.cs#0)]
[!code-vb[c_DuplexServices#0](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_duplexservices/vb/service.vb#0)]

`CalculatorService` 类实现 `ICalculatorDuplex` 主接口。 服务使用 <xref:System.ServiceModel.InstanceContextMode.PerSession> 实例模式来维护每个会话的结果。 名为 `Callback` 的私有属性访问指向客户端的回调通道。 服务使用该回调通过回调接口将消息返送给客户端，如下面的示例代码所示。

[!code-csharp[c_DuplexServices#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_duplexservices/cs/service.cs#1)]
[!code-vb[c_DuplexServices#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_duplexservices/vb/service.vb#1)]

客户端必须提供一个实现双工协定回调接口的类，以便从服务接收消息。 下面的示例代码演示了一个实现 `CallbackHandler` 接口的 `ICalculatorDuplexCallback` 类。

[!code-csharp[c_DuplexServices#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_duplexservices/cs/client.cs#2)]
[!code-vb[c_DuplexServices#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_duplexservices/vb/client.vb#2)]

为双工协定生成的 WCF 客户端需要在 <xref:System.ServiceModel.InstanceContext> 构造时提供一个类。 此 <xref:System.ServiceModel.InstanceContext> 类用作实现回调接口并处理从服务发送回的消息的对象所在的位置。 <xref:System.ServiceModel.InstanceContext> 类是用 `CallbackHandler` 类的实例构造的。 此对象处理通过回调接口从服务发送到客户端的消息。

[!code-csharp[c_DuplexServices#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_duplexservices/cs/client.cs#3)]
[!code-vb[c_DuplexServices#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_duplexservices/vb/client.vb#3)]

服务的配置必须设置为提供同时支持会话通信和双工通信的绑定。 `wsDualHttpBinding` 元素支持会话通信，并通过提供双 HTTP 连接（一个连接对应于一个方向）来允许进行双工通信。

在客户端，必须配置一个服务器可用来连接客户端的地址，如下面的示例配置所示。

> [!NOTE]
> 未能使用安全会话通过身份验证的非双工客户端通常会引发 <xref:System.ServiceModel.Security.MessageSecurityException>。 但是，如果使用安全会话的双工客户端未能通过身份验证，客户端则收到 <xref:System.TimeoutException>。

如果使用 `WSHttpBinding` 元素创建客户端/服务，并且不包含客户端回调终结点，您将收到以下错误。

```console
HTTP could not register URL
htp://+:80/Temporary_Listen_Addresses/<guid> because TCP port 80 is being used by another application.
```

下面的示例代码演示如何以编程方式指定客户端终结点地址。

```csharp
WSDualHttpBinding binding = new WSDualHttpBinding();
EndpointAddress endptadr = new EndpointAddress("http://localhost:12000/DuplexTestUsingCode/Server");
binding.ClientBaseAddress = new Uri("http://localhost:8000/DuplexTestUsingCode/Client/");
```

```vb
Dim binding As New WSDualHttpBinding()
Dim endptadr As New EndpointAddress("http://localhost:12000/DuplexTestUsingCode/Server")
binding.ClientBaseAddress = New Uri("http://localhost:8000/DuplexTestUsingCode/Client/")
```

下面的示例代码演示了如何在配置中指定客户端终结点地址。

```xml
<client>
    <endpoint name ="ServerEndpoint"
          address="http://localhost:12000/DuplexTestUsingConfig/Server"
          bindingConfiguration="WSDualHttpBinding_IDuplexTest"
            binding="wsDualHttpBinding"
           contract="IDuplexTest" />
</client>
<bindings>
    <wsDualHttpBinding>
        <binding name="WSDualHttpBinding_IDuplexTest"
          clientBaseAddress="http://localhost:8000/myClient/" >
            <security mode="None"/>
         </binding>
    </wsDualHttpBinding>
</bindings>
```

> [!WARNING]
> 当服务或客户端关闭其通道时，双工模型不会自动检测。 因此，如果客户端意外终止，则默认情况下将不会通知该服务; 或者，如果服务意外终止，则将不会通知客户端。 如果你使用断开连接的服务，则 <xref:System.ServiceModel.CommunicationException> 会引发异常。 客户端和服务可以实现自己的协议以相互通知对方（如果它们这么选择）。 有关错误处理的详细信息，请参阅[WCF 错误处理](../wcf-error-handling.md)。

## <a name="see-also"></a>请参阅

- [双工](../samples/duplex.md)
- [指定客户端运行时行为](../specifying-client-run-time-behavior.md)
- [如何：创建通道工厂并用它创建和管理通道](how-to-create-a-channel-factory-and-use-it-to-create-and-manage-channels.md)
