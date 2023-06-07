---
title: 单向
ms.date: 03/30/2017
ms.assetid: 74e3e03d-cd15-4191-a6a5-1efa2dcb9e73
ms.openlocfilehash: 7732b63cccb98ac54d99a0430dbaf0c8abfdaaa5
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96245024"
---
# <a name="one-way"></a>单向

此示例演示具有单向服务操作的服务协定。 客户端不会像在双向服务操作中那样等待服务操作完成。 此示例基于 [入门](getting-started-sample.md) ，并使用 `wsHttpBinding` 绑定。 此示例中的服务是自承载控制台应用程序，通过它可以观察接收和处理请求的服务。 客户端也是一个控制台应用程序。  
  
> [!NOTE]
> 本主题的最后介绍了此示例的设置过程和生成说明。  
  
 若要创建单向服务协定，请定义服务协定，将 <xref:System.ServiceModel.OperationContractAttribute> 类应用于每个操作，并将 <xref:System.ServiceModel.OperationContractAttribute.IsOneWay%2A> 设置为 `true`，如下面的示例代码所示：  
  
```csharp
[ServiceContract(Namespace="http://Microsoft.ServiceModel.Samples")]  
public interface IOneWayCalculator  
{  
    [OperationContract(IsOneWay=true)]  
    void Add(double n1, double n2);  
    [OperationContract(IsOneWay = true)]  
    void Subtract(double n1, double n2);  
    [OperationContract(IsOneWay = true)]  
    void Multiply(double n1, double n2);  
    [OperationContract(IsOneWay = true)]  
    void Divide(double n1, double n2);  
}  
```  
  
 为了演示客户端不会等待服务操作完成，此示例中的服务代码实现了五秒钟的延迟，如下面的示例代码所示：  
  
```csharp
// This service class implements the service contract.  
// This code writes output to the console window.  
 [ServiceBehavior(ConcurrencyMode = ConcurrencyMode.Multiple,  
    InstanceContextMode = InstanceContextMode.PerCall)]  
public class CalculatorService : IOneWayCalculator  
{  
    public void Add(double n1, double n2)  
    {  
        Console.WriteLine("Received Add({0},{1}) - sleeping", n1, n2);  
        System.Threading.Thread.Sleep(1000 * 5);  
        double result = n1 + n2;  
        Console.WriteLine("Processing Add({0},{1}) - result: {2}", n1, n2, result);  
    }  
    ...  
}  
```  
  
 当客户端调用服务时，调用不等待服务操作完成即返回。  
  
 运行示例时，客户端和服务活动将显示在服务和客户端控制台窗口中。 您可以看到服务从客户端接收消息。 在每个控制台窗口中按 Enter 可以同时关闭服务和客户端。  
  
 客户端在服务之前完成，说明了客户端没有等待单向服务操作完成。 客户端输出如下所示：  
  
```console  
Add(100,15.99)  
Subtract(145,76.54)  
Multiply(9,81.25)  
Divide(22,7)  
  
Press <ENTER> to terminate client.  
```  
  
 服务输出如下所示：  
  
```console  
The service is ready.  
Press <ENTER> to terminate service.  
  
Received Add(100,15.99) - sleeping  
Received Subtract(145,76.54) - sleeping  
Received Multiply(9,81.25) - sleeping  
Received Divide(22,7) - sleeping  
Processing Add(100,15.99) - result: 115.99  
Processing Subtract(145,76.54) - result: 68.46  
Processing Multiply(9,81.25) - result: 731.25  
Processing Divide(22,7) - result: 3.14285714285714  
```  
  
> [!NOTE]
> HTTP 从定义上讲是一个请求/响应协议；当发出请求时，即返回响应。 即使对于通过 HTTP 公开的单向服务操作，也是如此。 当调用操作时，服务在执行服务操作之前返回 HTTP 状态码 202。 此状态码表示请求已被接受进行处理，但处理尚未完成。 调用操作的客户端在从服务收到 202 响应之前处于阻止状态。 当使用绑定（配置为使用会话）发送多个单向消息时，这可能会产生某些意外行为。 此示例中使用的 `wsHttpBinding` 绑定配置为默认使用会话来建立安全上下文。 默认情况下，会话中的消息一定会按照它们的发送顺序到达。 因此，当发送会话中的第二个消息时，在处理完第一个消息之前不会处理第二个消息。 这样的结果是，在处理完上一个消息之前，客户端不会收到消息的 202 响应。 因此，客户端似乎是阻止了每个后续的操作调用。 为了避免此行为，此示例对运行库进行了配置，以便将消息并发调度给不同的实例进行处理。 本示例将 <xref:System.ServiceModel.ServiceBehaviorAttribute.InstanceContextMode%2A> 设置为 `PerCall`，以使每条消息可以由不同的实例来处理。 <xref:System.ServiceModel.ServiceBehaviorAttribute.ConcurrencyMode%2A> 设置为 `Multiple`，以允许多个线程同时调度消息。  
  
### <a name="to-set-up-build-and-run-the-sample"></a>设置、生成和运行示例  
  
1. 确保已对 [Windows Communication Foundation 示例执行了一次性安装过程](one-time-setup-procedure-for-the-wcf-samples.md)。  
  
2. 若要生成 C# 或 Visual Basic .NET 版本的解决方案，请按照 [Building the Windows Communication Foundation Samples](building-the-samples.md)中的说明进行操作。  
  
3. 若要以单机配置或跨计算机配置来运行示例，请按照 [运行 Windows Communication Foundation 示例](running-the-samples.md)中的说明进行操作。  
  
> [!NOTE]
> 请在运行客户端之前运行服务，并在关闭服务之前关闭客户端。 这样可以避免当客户端由于服务已关闭而无法彻底关闭安全会话时出现客户端异常。  
  
> [!IMPORTANT]
> 您的计算机上可能已安装这些示例。 在继续操作之前，请先检查以下（默认）目录：  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> 如果此目录不存在，请参阅[Windows Communication Foundation (wcf) ，并 Windows Workflow Foundation (的 WF](https://www.microsoft.com/download/details.aspx?id=21459)) .NET Framework Windows Communication Foundation ([!INCLUDE[wf1](../../../../includes/wf1-md.md)] 此示例位于以下目录：  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Contract\Service\Oneway`  
