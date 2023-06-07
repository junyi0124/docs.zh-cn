---
title: 通道工厂
ms.date: 03/30/2017
ms.assetid: 09b53aa1-b13c-476c-a461-e82fcacd2a8b
ms.openlocfilehash: 2aa44c4ef274fa548d490b0d8a648457a7b1e03b
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/09/2020
ms.locfileid: "84600654"
---
# <a name="channel-factory"></a>通道工厂

此示例演示客户端应用程序如何使用 <xref:System.ServiceModel.ChannelFactory> 类而不是生成的客户端创建通道。 此示例基于实现计算器服务的[入门](getting-started-sample.md)。

> [!NOTE]
> 本主题的最后介绍了此示例的设置过程和生成说明。

此示例使用 <xref:System.ServiceModel.ChannelFactory%601> 类来创建到服务终结点的通道。 通常，若要创建到服务终结点的通道，可以使用 "Svcutil.exe"[元数据实用工具（）](../servicemodel-metadata-utility-tool-svcutil-exe.md)生成客户端类型，并创建生成的类型的实例。 还可以通过使用 <xref:System.ServiceModel.ChannelFactory%601> 类创建通道，如该示例所示。 下面的示例代码创建的服务与[入门](getting-started-sample.md)中的服务相同。

```csharp
EndpointAddress address = new EndpointAddress("http://localhost/servicemodelsamples/service.svc");
WSHttpBinding binding = new WSHttpBinding();
ChannelFactory<ICalculator> factory = new
                    ChannelFactory<ICalculator>(binding, address);
ICalculator channel = factory.CreateChannel();
```

> [!IMPORTANT]
> 如果你在跨计算机方案中运行该示例，则必须将前面代码中的“localhost”替换为运行服务的计算机的完全限定名称。 此示例不使用配置来设置终结点地址，因此这必须通过代码完成。

一旦创建了通道，便可以像调用生成的客户端那样调用服务操作。

```csharp
// Call the Add service operation.
double value1 = 100.00D;
double value2 = 15.99D;
double result = channel.Add(value1, value2);
Console.WriteLine("Add({0},{1}) = {2}", value1, value2, result);
```

若要关闭通道，必须将其强制转换为 <xref:System.ServiceModel.IClientChannel> 接口。 这是因为生成的通道是在使用 `ICalculator` 接口的客户端应用程序中声明的，该应用程序具有 `Add` 和 `Subtract` 等方法，但没有 `Close` 方法。 `Close` 方法在 <xref:System.ServiceModel.ICommunicationObject> 接口上产生。

```csharp
// Close the channel.
 ((IClientChannel)client).Close();
```

运行示例时，操作请求和响应将显示在客户端控制台窗口中。 在客户端窗口中按 Enter 可以关闭客户端应用程序。

```console
Add(100,15.99) = 115.99
Subtract(145,76.54) = 68.46
Multiply(9,81.25) = 731.25
Divide(22,7) = 3.14285714285714

Press <ENTER> to terminate client.
```

### <a name="to-set-up-build-and-run-the-sample"></a>设置、生成和运行示例

1. 确保已对[Windows Communication Foundation 示例执行了一次性安装过程](one-time-setup-procedure-for-the-wcf-samples.md)。

2. 若要生成 C# 或 Visual Basic .NET 版本的解决方案，请按照 [Building the Windows Communication Foundation Samples](building-the-samples.md)中的说明进行操作。 请注意，该示例不会启用元数据发布。 必须先对该示例启用元数据发布才能重新生成客户端类型。

3. 若要以单机配置或跨计算机配置来运行示例，请按照[运行 Windows Communication Foundation 示例](running-the-samples.md)中的说明进行操作。

### <a name="to-run-the-sample-cross-machine"></a>跨计算机运行示例

1. 将下面代码中的“localhost”替换为运行服务的计算机的完全限定名称。

    ```csharp
    EndpointAddress address = new EndpointAddress("http://localhost/servicemodelsamples/service.svc");
    ```

> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> 如果此目录不存在，请参阅[.NET Framework 4 的 Windows Communication Foundation （wcf）和 Windows Workflow Foundation （WF）示例](https://www.microsoft.com/download/details.aspx?id=21459)以下载所有 WINDOWS COMMUNICATION FOUNDATION （wcf）和 [!INCLUDE[wf1](../../../../includes/wf1-md.md)] 示例。 此示例位于以下目录：
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Client\ChannelFactory`
