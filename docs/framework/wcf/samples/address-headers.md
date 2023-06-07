---
title: 地址标头
ms.date: 03/30/2017
ms.assetid: b0c94d4a-3bde-4b4d-bb6d-9f12bc3a6940
ms.openlocfilehash: 133826bbbea62b660bdcdd884ce657528ad30873
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/09/2020
ms.locfileid: "84576000"
---
# <a name="address-headers"></a>地址标头

地址标头示例演示客户端如何使用 Windows Communication Foundation （WCF）将引用参数传递给服务。

> [!NOTE]
> 本主题的最后介绍了此示例的设置过程和生成说明。

WS-Addressing 规范将终结点引用的概念定义为对特定 Web 服务终结点寻址的方式。 在 WCF 中，使用类对终结点引用进行建模 `EndpointAddress` - `EndpointAddress` 类的地址字段的类型 `ServiceEndpoint` 。

作为终结点引用模型的一部分，每个引用都可以携带一些可添加额外标识信息的引用参数。 在 WCF 中，这些引用参数建模为类的实例 `AddressHeader` 。

在本示例中，客户端将向客户端终结点的 `EndpointAddress` 添加一个引用参数。 服务将查找此引用参数并在其“Hello”服务操作的逻辑中使用此参数的值。

## <a name="client"></a>客户端

若要使客户端发送引用参数，该客户端必须向 `AddressHeader` 的 `EndpointAddress` 添加一个 `ServiceEndpoint`。 由于 `EndpointAddress` 类是不可变的，因此必须使用 `EndpointAddressBuilder` 类来完成终结点地址的修改。 下面的代码将客户端初始化为作为其消息的一部分来发送引用参数。

```csharp
HelloClient client = new HelloClient();
EndpointAddressBuilder builder =
    new EndpointAddressBuilder(client.Endpoint.Address);
AddressHeader header =
    AddressHeader.CreateAddressHeader(IDName, IDNamespace, "John");
builder.Headers.Add(header);
client.Endpoint.Address = builder.ToEndpointAddress();
```

代码使用原始 `EndpointAddressBuilder` 作为初始值创建 `EndpointAddress`。 然后添加新创建的地址标头;调用可 `CreateAddressHeader` 创建具有特定名称、命名空间和值的标头。 此处值为“John”。 将标头添加到生成器后，`ToEndpointAddress()` 方法会将生成器（可变）转换回终结点地址（不可变），该地址将分配回给客户端终结点的“地址”字段。

现在，当客户端调用 `Console.WriteLine(client.Hello());` 时，服务能够获取此地址参数的值，如客户端生成的输出所示。

`Hello, John`

## <a name="server"></a>服务器

服务操作 `Hello()` 的实现使用当前 `OperationContext` 来检查传入消息上标头的值。

```csharp
string id = null;
// look at headers on incoming message
for (int i = 0;
     i < OperationContext.Current.IncomingMessageHeaders.Count;
     ++i)
{
    MessageHeaderInfo h = OperationContext.Current.IncomingMessageHeaders[i];
    // for any reference parameters with the correct name & namespace
    if (h.IsReferenceParameter &&
        h.Name == IDName &&
        h.Namespace == IDNamespace)
    {
        // read the value of that header
        XmlReader xr = OperationContext.Current.IncomingMessageHeaders.GetReaderAtHeader(i);
        id = xr.ReadElementContentAsString();
    }
}
return "Hello, " + id;
```

代码循环访问传入消息上的所有标头，查找具有特定名称并作为引用参数的标头。 该找到参数时，代码将读取参数的值并将其保存在“id”变量中。

#### <a name="to-set-up-build-and-run-the-sample"></a>设置、生成和运行示例

1. 确保已对[Windows Communication Foundation 示例执行了一次性安装过程](one-time-setup-procedure-for-the-wcf-samples.md)。

2. 若要生成 C# 或 Visual Basic .NET 版本的解决方案，请按照 [Building the Windows Communication Foundation Samples](building-the-samples.md)中的说明进行操作。

3. 若要以单机配置或跨计算机配置来运行示例，请按照[运行 Windows Communication Foundation 示例](running-the-samples.md)中的说明进行操作。

> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> 如果此目录不存在，请参阅[.NET Framework 4 的 Windows Communication Foundation （wcf）和 Windows Workflow Foundation （WF）示例](https://www.microsoft.com/download/details.aspx?id=21459)以下载所有 WINDOWS COMMUNICATION FOUNDATION （wcf）和 [!INCLUDE[wf1](../../../../includes/wf1-md.md)] 示例。 此示例位于以下目录：
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Client\AddressHeaders`
